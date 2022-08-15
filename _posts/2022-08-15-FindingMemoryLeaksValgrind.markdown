---
layout: post
title:  "C++ : Finding memory leaks with Valgrind"
date:   2022-08-15 12:46:00 -0400
categories: c++
---

From the man page, here's the definition of Valgrind : Valgrind is a flexible program for debugging and profiling Linux executables. It consists of a core, which provides a
synthetic CPU in software, and a series of debugging and profiling tools.

It can does many things, but in this post we will focus on the memcheck tool which is the default.

## A few Valgrind options

The main option which interest us is **leak-check**. The four values below are available : 

- **no** : The check will be disabled.
- **summary** (default) : Will tell you how many leaks occurred,but without any details.
- **yes** or **full** : Will provides details of each individual leak found.

We will use the value **full** for our different tests.

Another that will be useful is the **show-leak-kinds** option. 

You can specify **all** of a combination of comma separated list of values : **definite** (default), 
**indirect**, **possible** (default) and **reachable**. We will see in details each type of leak later.

The **show-error-list=yes (-s)** option will show the list of detected errors and the list of used 
suppressions at exit. 

Finally, the **quiet (-q)** option will only print error messages so there will be no Valgrind header, Heap 
summary and Leak summary.

## Different kind of leaks

### Definite

This kind of leak appears when you forgot to delete an allocated memory block and that you cannot
delete anymore.

Here's an example of a definite leak. The getRandomInt function generate a number between min and
max, assigne that value to a new int allocated on the heap and returns only the value. I know it's 
pretty stupid to do this, but it's only demo purpose. :)

```c++
#include <iostream>
#include <random>

int getRandomInt(int min, int max)
{
    std::random_device rand_dev;
    std::default_random_engine rand_engine(rand_dev());
    std::uniform_int_distribution<int> uniform_dist(min, max);
    int *retval = new int(uniform_dist(rand_engine));
    return *retval;
}

int main()
{
    std::cout << "The random number is " <<  getRandomInt(1, 5) << std::endl;
    return 0;
}
```

Here is the result of running the application with valgrind with **show-leak-kinds=definite**.

```bash
valgrind --leak-check=full --show-leak-kinds=definite ./build/valgrind_demo 
==195035== Memcheck, a memory error detector
==195035== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==195035== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==195035== Command: ./build/valgrind_demo
==195035== 
The random number is 4
==195035== 
==195035== HEAP SUMMARY:
==195035==     in use at exit: 4 bytes in 1 blocks
==195035==   total heap usage: 3 allocs, 2 frees, 73,732 bytes allocated
==195035== 
==195035== 4 bytes in 1 blocks are definitely lost in loss record 1 of 1
==195035==    at 0x4849013: operator new(unsigned long) (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==195035==    by 0x10A4B3: getRandomIntPtr(int, int) (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==195035==    by 0x10A54B: main (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==195035== 
==195035== LEAK SUMMARY:
==195035==    definitely lost: 4 bytes in 1 blocks
==195035==    indirectly lost: 0 bytes in 0 blocks
==195035==      possibly lost: 0 bytes in 0 blocks
==195035==    still reachable: 0 bytes in 0 blocks
==195035==         suppressed: 0 bytes in 0 blocks
==195035== 
==195035== For lists of detected and suppressed errors, rerun with: -s
==195035== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
```

Let's analyze this log. First in the header, Valgrind informs us that we are using the Memcheck tool.
Then we can see the HEAP SUMMARY section, as the name suggests, sums up all the heap usage while
the software is running.

### Indirect

### Possible

### Reachable
