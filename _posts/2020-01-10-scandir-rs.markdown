---
layout: post
title:  scandir-rs
tagline: A faster alternative to os.walk. It also comes with additional features.
date:   2020-01-10 11:40:00 +0100
categories: posts
---

Because speed matters...

With the increased speed of SSDs single threaded file access does not always fully utilize the disk. The bottleneck more and more is the CPU itself.

When I started learning the great new language Rust I came across a crate called jwalk. This crate does directory scanning in parallel with a thread pool. The benchmarks are amazing. So I thought I write a Python module as a faster alternative to os.walk which uses jwalk. The result is the new module scandir-rs which can be found on pypi.org.

The API is a bit different and provides more features. But it should be easy to replace os.walk and os.scandir with scandir-rs.

## Usage examples

Get statistics of a directory:

```python
import scandir_rs as scandir
print(scandir.count.count("~/workspace", metadata_ext=True))
```

The same, but asynchronously in background using a class instance and a context manager:

```python
import scandir_rs as scandir
C = scandir.count.Count("~/workspace", metadata_ext=True))
with C:
    while C.busy():
        statistics = C.statistics
        # Do something
```

``os.walk()`` example:

```python
import scandir_rs as scandir
for root, dirs, files in scandir.walk.Walk("~/workspace", iter_type=scandir.ITER_TYPE_WALK):
    # Do something
```

``os.scandir()`` example:

```python
import scandir_rs as scandir
for entry in scandir.scandir.Scandir("~/workspace", metadata_ext=True):
    # Do something
```

## Benchmarks

Now let's have a look at some benchmarks. In the below table the line **scandir_rs.walk.Walk** returns comparable results to ``os.walk``.

### Linux with Ryzen 5 2400G and SSD

#### Directory *~/workspace* with

- 22845 directories
- 321354 files
- 130 symlinks
- 22849 hardlinks
- 4 devices
- 1 pipes
- 4.6GB size and 5.4GB usage on disk

| Time [s] | Method                                              |
|----------|-----------------------------------------------------|
| 0.547    | os.walk (Python 3.7)                                |
| 0.132    | scandir_rs.count.count                              |
| 0.142    | scandir_rs.count.Count                              |
| 0.237    | scandir_rs.walk.Walk                                |
| 0.224    | scandir_rs.walk.toc                                 |
| 0.242    | scandir_rs.walk.collect                             |
| 0.262    | scandir_rs.scandir.entries                          |
| 0.344    | scandir_rs.scandir.entries(metadata=True)           |
| 0.336    | scandir_rs.scandir.entries(metadata_ext=True)       |
| 0.280    | scandir_rs.scandir.Scandir.collect                  |
| 0.262    | scandir_rs.scandir.Scandir.iter                     |
| 0.330    | scandir_rs.scandir.Scandir.iter(metadata_ext=True)  |

Up to **2 times faster** on Linux.

### Windows 10 with Laptop Core i7-4810MQ @ 2.8GHz Laptop, MTF SSD

#### Directory *C:\Windows* with

- 84248 directories
- 293108 files
- 44.4GB size and 45.2GB usage on disk

| Time [s] | Method                                              |
|----------|-----------------------------------------------------|
| 26.881   | os.walk (Python 3.7)                                |
| 4.094    | scandir_rs.count.count                              |
| 3.654    | scandir_rs.count.Count                              |
| 3.978    | scandir_rs.walk.Walk                                |
| 3.848    | scandir_rs.walk.toc                                 |
| 3.777    | scandir_rs.walk.collect                             |
| 3.987    | scandir_rs.scandir.entries                          |
| 3.905    | scandir_rs.scandir.entries(metadata=True)           |
| 4.062    | scandir_rs.scandir.entries(metadata_ext=True)       |
| 3.934    | scandir_rs.scandir.Scandir.collect                  |
| 3.981    | scandir_rs.scandir.Scandir.iter                     |
| 3.821    | scandir_rs.scandir.Scandir.iter(metadata_ext=True)  |

Up to **6.7 times faster** on Windows 10.

#### Directory *C:\testdir* with

- 185563 directories
- 1641277 files
- 2696 symlinks
- 97GB size and 100.5GB usage on disk

| Time [s] | Method                                              |
|----------|-----------------------------------------------------|
| 151.143  | os.walk (Python 3.7)                                |
| 7.549    | scandir_rs.count.count                              |
| 7.531    | scandir_rs.count.Count                              |
| 8.710    | scandir_rs.walk.Walk                                |
| 8.625    | scandir_rs.walk.toc                                 |
| 8.599    | scandir_rs.walk.collect                             |
| 9.014    | scandir_rs.scandir.entries                          |
| 9.208    | scandir_rs.scandir.entries(metadata=True)           |
| 8.925    | scandir_rs.scandir.entries(metadata_ext=True)       |
| 9.243    | scandir_rs.scandir.Scandir.collect                  |
| 8.462    | scandir_rs.scandir.Scandir.iter                     |
| 8.380    | scandir_rs.scandir.Scandir.iter(metadata_ext=True)  |

Up to **17.4 times faster** on Windows 10.

Check out the [scandir-rs][scandir-rs] module on github, licensed under the MIT license.

[scandir-rs]: https://github.com/brmmm3/scandir-rs
