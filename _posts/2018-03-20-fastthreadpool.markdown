---
layout: post
title:  fastthreadpool
tagline: A fast and lightweight threading pool
date:   2018-03-20 23:00:37 +0100
categories: posts
---
Existing implementations of thread pools have a relatively high overhead in certain situations.
Especially `apply_async` in `multiprocessing.pool.ThreadPool` and `submit` in `concurrent.futures`.
`ThreadPoolExecutor` at all (see the benchmarks in the *docs* directory).

In case of `ThreadPoolExecutor` don't use `wait`.
**It can be extremely slow!** If you've only a small number of jobs and the jobs have a relatively
long processing time, then these overheads don't count. But in case of high number of jobs with
short processing time the overhead of the above implementations will noticeably slow down the
processing speed. The `fastthreadpool` module solves this issue, because it has a very small
overhead in all situations.

Although `fastthreadpool` is lightweight it has some additional cool features like methods for
**later scheduling**, **repeating events** and **generator functions** for worker callback functions.

In addition to get the best performance I've also written a fast and lightweight semaphore which
is more than **20 times** faster than the one which comes with the Python installation.

Some reasons why `fastthreadpool` is so fast:
 - Avoid locks as much as possible
 - Use deque instead of Queue
 - Do not create a class instance for every work item

The first test in benchmarks.py has a minimum worker callback function which just returns the given
parameter. The main thread calculates the sum of the returned values. This is the most extreme case
where the overhead of the thread pool implementation counts as much as possible. This is not the
typical use case but it shows the overhead of the different thread pool implementations very well.

The results show that `map` in `ThreadPool` performs good, but `map` in `fastthreadpool` is a bit
more efficient.

But `apply_async` has a very bad performance, even worse
than `submit` in `ThreadPoolExecutor`. `submit` in `fastthreadpool` performs very well.

| Thread Pool        |  Function   |  Time  |
| ------------------ |:-----------:|:------:|
| single threaded    | for loop    |  0.378 |
| fastthreadpool     | map         |  0.166 |
| ThreadPool         | map_async   |  0.280 |
| ThreadPoolExecutor | map         | 53.072 |
| fastthreadpool     | submit      |  2.679 |
| ThreadPool         | apply_async | 76.350 |
| ThreadPoolExecutor | submit      | 59.161 |

A more typical case shows the last example where the worker threads serialize  and compress data.

| Thread Pool        |  Function   |  Time  |
| ------------------ |:-----------:|:------:|
| single threaded    | for loop    |  0.628 |
| fastthreadpool     | map         |  0.598 |
| ThreadPool         | map_async   |  0.609 |
| ThreadPoolExecutor | map         |  1.192 |
| fastthreadpool     | submit      |  0.659 |
| ThreadPool         | apply_async |  1.317 |
| ThreadPoolExecutor | submit      |  1.169 |

As you can see the worker threads are still **2 times** faster with `fastthreadpool` when submitting
single jobs to the pool than the other 2 thread pool implementations.

Again this example shows clearly if speed matter then avoid `concurrent.futures`. Although it has a
nice interface it is really slow.

For examples how to use `fastthreadpool` please have a look at the examples directory.

Check out the [fastthreadpool][fastthreadpool] module on github, licensed under the MIT license.

[fastthreadpool]: https://github.com/brmmm3/fastthreadpool

