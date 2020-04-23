---
layout: post
title:  A socketserver with threads vs uvloop
tagline: A fast and lightweight threading pool
date:   2020-01-10 11:47:37 +0100
categories: posts
---
The ``uvloop`` project is great with an amazing performance, and a good replacement for the default asyncio module, if Linux is used. Unfortunately ``uvloop`` is not available for Windows.

Just for interest I wanted to know how a multi threaded version competes with the ``uvloop``, which is single threaded.
Is a comparable performance possible, or is a solution with multiple threads even faster?

So I took the echoserver.py example from the ``uvloop`` project and extended it with support for ``fastthreadpool``.

Here is a simplified code example of a socket server with ``fastthreadpool``:

```python
def pool_echo_server(address, threads, size):
    sock = socket(AF_INET, SOCK_STREAM)
    sock.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
    sock.bind(address)
    sock.listen(threads)
    with sock:
        while True:
            client, addr = sock.accept()
            pool.submit(pool_echo_client, client, size)

def pool_echo_client(client, size):
    client.setsockopt(IPPROTO_TCP, TCP_NODELAY, 1)
    b = bytearray(size)
    bl = [ b ]
    with client:
        try:
            while True:
                client.recvmsg_into(bl)
                client.sendall(b)
        except:
            pass

pool = fastthreadpool.Pool(8)
pool.submit(pool_echo_server, addr, 8, 4096)
pool.join()
```

For a complete example please have a look into the examples/bench directory.

Folowing benchmarks were executed on a Ryzen 7 with Linux Mint 18.3, kernel 4.13.0-37-generic and Python 3.6. echoserver.py and echoclient.py were executed on the same machine.
echoclient.py was always run with 5 parallel workers. Only the message size was modified for the different tests. I've only compared ``uvloop`` using simple protocol with the multi threaded version.

| Module  | Server buffer size | Message size | Messages/s | MB/s
| ------- |:------------------:|:------------:|:----------:|:---:
| uvloop  |                    |  1000 bytes  |  128220    | 122,28
| uvloop  |                    |  4kB         |  108581    | 424,14
| uvloop  |                    |  64kB        |   26004    | **1625,25**
| threads | 4kB                |  1000 bytes  |  423226    | 403,62
| threads | 4kB                |  4kB         |  112033    | 437,63
| threads | 8kB                |  4kB         |  256438    | 1001,71
| threads | 16kB               |  4kB         |  381320    | 1489,53
| threads | 4kB                |  64kB        |    6690    | 418,13
| threads | 64kB               |  64kB        |   66672    | **4167**
| threads | 128kB              |  64kB        |   62236    | 3889,75
| threads | 256kB              |  64kB        |   60292    | 3768,25

Folowing benchmarks were executed on a Core i5-8250U with Linux Mint 18.3, kernel 5.2.3-050203-generic and Python 3.7.2. echoserver.py and echoclient.py were executed on the same machine.
echoclient.py was always run with 5 parallel workers. Only the message size was modified for the different tests.

| Module  | Server buffer size | Message size | Messages/s | MB/s
| ------- |:------------------:|:------------:|:----------:|:---:
| uvloop  |                    |  1000 bytes  |  34628     | 33,21
| uvloop  |                    |  4kB         |  35975     | 140,53
| uvloop  |                    |  64kB        |   2170     | 135,68
| uvloop/streams  |            |  1000 bytes  |  65588     | 62,55
| uvloop/streams  |            |  4kB         |  61856     | 241,63
| uvloop/streams  |            |  64kB        |       | **1625,25**
| uvloop/protocol  |           |  1000 bytes  |  34628     | 33,21
| uvloop/protocol  |           |  4kB         |  35975     | 140,53
| uvloop/protocol  |           |  64kB        |   26004    | **1625,25**
| asyncio |                    |  1000 bytes  |  128220    | 122,28
| asyncio |                    |  4kB         |  108581    | 424,14
| asyncio |                    |  64kB        |   26004    | **1625,25**
| asyncio/streams |            |  4kB         |  108581    | 424,14
| asyncio/streams |            |  4kB         |  108581    | 424,14
| asyncio/streams |            |  64kB        |   26004    | **1625,25**
| asyncio/protocol |           |  64kB        |   26004    | **1625,25**
| asyncio/protocol |           |  4kB         |  108581    | 424,14
| asyncio/protocol |           |  64kB        |   26004    | **1625,25**
| threads | 4kB                |  1000 bytes  |  423226    | 403,62
| threads | 4kB                |  4kB         |  112033    | 437,63
| threads | 8kB                |  4kB         |  256438    | 1001,71
| threads | 16kB               |  4kB         |  381320    | 1489,53
| threads | 4kB                |  64kB        |    6690    | 418,13
| threads | 64kB               |  64kB        |   66672    | **4167**
| threads | 128kB              |  64kB        |   62236    | 3889,75
| threads | 256kB              |  64kB        |   60292    | 3768,25

The results show clearly that the multi threaded version performs much better if the buffer size on the server side is adjusted perfectly depending on the message size. The magic why the multi threaded example is so fast is that it uses recvmsg_into which writes the received data into a preallocated buffer.
