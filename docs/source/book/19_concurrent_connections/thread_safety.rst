Thread safety
----------------------

When working with threads there are several recommendations and rules. If they are respected, it is easier to work with threads and it is likely that there will be no problem with threads. Of course, from time to time, there will be tasks that will require violations of recommendations. However, before doing so, it is better to try to meet the task by adhering to recommendations. If this is not possible, then we should look for ways to secure the solution so that the data is not damaged.

Very important feature of working with threads: with a small number of threads and small test tasks "everything works". For example, printing output when connected to 20 devices in 5 threads will work normally. But when connected to a large number of devices with a large number of threads, it turns out that sometimes messages "will fit" on each other. This peculiarity appears very often, so do not trust the variant when "everything works" on basic examples, follow the rules of working with threads.

Before dealing with rules we have to deal with the term "thread safety". Thread safety is a concept that describes work with multithreaded programs. The code is considered to be thread-safe if it can work normally with multiple threads.

For example, print() function is not thread-safe. This is demonstrated by the fact that when code executes print() from different threads, messages in the output can be mixed. There could be output with a part of message from the first thread, then a part from the second thread, then a part from the first thread, and so on. That is, print() function does not work normally (as it should be) in thread. In this case, it is said that print() function is not  thread-safe.

In general, there is no problem if each thread works with its own resources. For example, each thread writes data to its own file. However, this is not always possible or can complicate the solution.

.. note::

    print() has problems because we write from different threads into one standard output stream  but print() is not thread-safe.

If you have to write from different threads to the same resource, there are two options:

1. Write to the same resource after the work in thread is finished. For example, a function has been executed in threads 1, 2 and 3, its result is obtained in turn (consecutively) from each thread, and then written into a file. 
2. Use a thread-safe alternative (not always available and/or easy). For example, use a logging module instead of print() function.

Recommendations when working with threads:

1. Do not write to the same resource from different threads if resource or what you write is not intended for multithreading. It is easy to find out by google something like "python write to file from threads".

  * There are nuances to this recommendation. For example, you can write from different threads to the same file if you use a Lock or a thread-safe queue. These options are often difficult to use and are not considered in the book. It’s likely that 95 percent of problems you’ll be facing can be solved without them.
  * This category includes writing/changing lists/dictionaries/sets from different threads. These objects are inherently thread-safe but there is no guarantee that when you change the same list from different threads, the data in the list will be correct. If you want to use a common container for different threads, use queue from Queue module. It’s thread-safe and you can work with it from different threads.

2. If there is a possibility, avoid communication between threads in the course of their work. This is not an easy task and it is best to avoid it.
3. Follow the KISS (Keep it simple, stupid) principle - try to make the solution as simple as possible.

.. note::

    These recommendations are generally written for those who are just beginning to program on Python. However, they tend to be relevant to most programmers who write applications for users rather than frameworks.
    

Module concurrent.futures which will be considered further, simplifies implementation of the first principle "Do not write to the same resource from different threads... ". The module interface itself encourages this, but of course it does not prohibit breaking it.

However, before getting to know concurrent.futures, you should consider the fundamentals of logging module. It will be used instead of print() function which is not inherently thread-safe.
