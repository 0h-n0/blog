---
layout: post
title:  Python and Lua
tagline: Python and Lua
date:   2019-07-28 11:20:37 +0100
categories: posts
---
Python is great, but pure Python code sometimes has one problem: It's slow.

Fortunately there are several great solutions to improve the performance, like Numpy, Cython, Numba, Pypy.

All of the above solutions have different drawbacks:

Numpy and Numba are big modules. In addition Numpy is not always fast enough.
Pypy is not 100% compatible and a heavy solution if it is used in addition to CPython.
Cython is complex and you need a C-compiler.
Recently I've found one more solution which maybe is not that well known: Lua integration in Python programs.

Lua is another scripting language with dynamic data types.

So I asked myself:

Does it make sense to have another scripting language inside Python scripts?

Let's have a look at a simple example: Mandelbrot

First of all the pure Python example:

```python
from numpy import complex, arange

def PyMandelbrotIter(c):
    z = 0
    for iters in range(200):
        if abs(z) >= 2:
            return iters
        z = z ** 2 + c
    return iters

def PyMandelbrot(size):
    image = Image.new('RGB', (size, size))
    pix = image.load()

    t1 = time.time()
    xPts = arange(-1.5, 0.5, 2.0 / size)
    yPts = arange(-1, 1, 2.0 / size)

    for xx, x in enumerate(xPts):
        for yy, y in enumerate(yPts):
            pix[xx, yy] = PyMandelbrotIter(complex(x, y))
    dt = time.time() - t1
    print(f"dt={dt:.2f}")
    image.show()
```

Runtimes of this example on a Core i7 laptop with Python 3.7 and Windows 10:

| Size | dt [s] |
:---:|:---:
320 | 3.32
640 | 13.54
1280 | 55.59


Now the Lua example integrated in a Python script:

```python
from lupa import LuaRuntime

lua_code = '''\
function(N, i, total)
  local char, unpack = string.char, table.unpack
  local result = ""
  local M, ba, bb, buf = 2/N, 2^(N%8+1)-1, 2^(8-N%8), {}
  local start_line, end_line = N/total * (i-1), N/total * i - 1
  for y=start_line,end_line do
    local Ci, b, p = y*M-1, 1, 0
    for x=0,N-1 do
      local Cr = x*M-1.5
      local Zr, Zi, Zrq, Ziq = Cr, Ci, Cr*Cr, Ci*Ci
      b = b + b
      for i=1,49 do
        Zi = Zr*Zi*2 + Ci
        Zr = Zrq-Ziq + Cr
        Ziq = Zi*Zi
        Zrq = Zr*Zr
        if Zrq+Ziq > 4.0 then b = b + 1; break; end
      end
      if b >= 256 then p = p + 1; buf[p] = 511 - b; b = 1; end
    end
    if b ~= 1 then p = p + 1; buf[p] = (ba-b)*bb; end
      result = result .. char(unpack(buf, 1, p))
    end
    return result
end
'''

def LuaMandelbrot(thrCnt, size):

    def LuaMandelbrotFunc(i, lua_func):
        results[i] = lua_func(size, i + 1, thrCnt)

    t1 = time.time()
    lua_funcs = [LuaRuntime(encoding=None).eval(lua_code) for _ in range(thrCnt)]

    results = [None] * thrCnt

    threads = [threading.Thread(target=LuaMandelbrotFunc, args=(i, lua_func))
               for i, lua_func in enumerate(lua_funcs)]
    for thread in threads:
        thread.start()
    for thread in threads:
        thread.join()

    result_buffer = b''.join(results)
    dt = time.time() - t1
    print(f"dt={dt:.2f}")

    image = Image.frombytes('1', (size, size), result_buffer)
    image.show()
```

Runtimes of this example on a performance laptop with Python 3.7 and Windows 10:

Size | dt [s]<br>1 thread | dt [s]<br>2 threads | dt [s]<br>4 threads | dt [s]<br>8 threads | dt [s]<br>16 threads
:----:| -----------:|:----------:|:----------:|:----------:|:----------:
320  |  0.22       |  0.11      |  0.07      |  0.06      |  0.04
640  |  0.68       |  0.38      |  0.26      |  0.21      |  0.17
1280 |  2.71       |  1.50      |  1.05      |  0.81      |  0.66

The above results are very impressive. As you can see Lua is really much faster. And as you can also see: **You can parallelize with threads!**

The module ``lupa``, which comes with a Lua interpreter and a JIT compiler, is a very interesting alternative for speeding up long running tasks.

The Lua solution has the following advantages:

- The ``lupa`` module is very small.
- Lua is much faster than Python.
- You can run Lua scripts in parallel with threads.
- Lua is very easy to read and code.
- You can easily integrate Lua scripts in Python code.
- You can easily access Python objects within Lua and vice versa.
- There are many extension modules available for Lua (~2600, see [luarocks.org](https://luarocks.org)).

Give ``lupa`` a try. It's easy to use and really great!
