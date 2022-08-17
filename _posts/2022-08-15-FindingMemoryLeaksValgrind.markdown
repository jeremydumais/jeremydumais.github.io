---
layout: post
title:  "C++ : Finding memory leaks with Valgrind"
date:   2022-08-15 12:46:00 -0400
categories: c++
---

From the man page, here's the definition of Valgrind : Valgrind is a flexible program for debugging 
and profiling Linux executable. It consists of a core, which provides a synthetic CPU in software, 
and a series of debugging and profiling tools.

It can do many things, but in this post we will focus on the memcheck tool which is the default.

A memory leak occurs whenever a memory block allocated on the heap is not freed and is no longer 
accessible. On long-running processes such as services, this can cause 
[Software aging](https://en.wikipedia.org/wiki/Software_aging).

## A few Valgrind options

The main option which interest us is **leak-check**. The four values below are available : 

- **no** : The check will be disabled.
- **summary** (default) : Will tell you how many leaks occurred,but without any details.
- **yes** or **full** : Will provide details of each individual leak found.

We will use the value **full** for our different tests.

Another that will be useful is the **show-leak-kinds** option. 

You can specify **all** or a comma separated list of values : **definite** (default), **indirect**, **possible** 
(default) and **reachable**. We will see in details each type of leak later.

The **show-error-list=yes (-s)** option will show the list of detected errors and the list of used 
suppressions at exit. 

Finally, the **quiet (-q)** option will only print error messages so there will be no Valgrind header, Heap 
summary and Leak summary.

## Different kind of leaks

*We will take the time to analyze the complete log of Valgrind via the Definite section and for the 
other sections we will go more briefly*

### Definite

This kind of leak appears when you forgot to delete an allocated memory block and that you cannot
delete anymore.

Here's an example of a definite leak. The getRandomInt function generates a number between min and
max, assign that value to a new int allocated on the heap and returns only the value. I know it's 
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
==195035==    by 0x10A4B3: getRandomInt(int, int) (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
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

```bash
==195035== Memcheck, a memory error detector
==195035== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==195035== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==195035== Command: ./build/valgrind_demo
==195035== 
```

The next part is the output produced by our program during its execution.

```bash
The random number is 4
==195035==
```

After comes the heap summary. This section sums up all the heap allocated block that occurred during
program execution.

```bash
==195035== HEAP SUMMARY:
==195035==     in use at exit: 4 bytes in 1 blocks
==195035==   total heap usage: 3 allocs, 2 frees, 73,732 bytes allocated
==195035==
```

The next part is the detail section. This is where the leaks will be reported. For example on this
one we can see that 1 block of 4 bytes (the size of an int on my system) is definitely lost. Of 
course, that makes sense because I didn't delete the memory allocated at the end of the function 
getRandomInt. 

```bash
==195035== 4 bytes in 1 blocks are definitely lost in loss record 1 of 1
==195035==    at 0x4849013: operator new(unsigned long) (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==195035==    by 0x10A4B3: getRandomInt(int, int) (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==195035==    by 0x10A54B: main (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==195035== 
```

*Note : For Valgrind to show the line numbers in your code you must compile your code with debug 
flags. In g++ it's the -g option and if you are using CMake, you can configure your project with the
-DCMAKE_BUILD_TYPE=Debug flag.*

The last part is the leak summary that sums up all the leaks that were detected while the software
was running.

```bash
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

### Indirect

An indirect leak appears if you lose access to a memory block that was only accessible through 
another definitely lost block.

Here's an example of an indirect leak :

```c++
struct node
{
    int number;
    struct node *parent;
};

int main()
{
    node *childItem = new node { .number = 1, .parent = new node { .number = 0, .parent = nullptr } };
    childItem = nullptr;
    return 0;
}
```

First, we create a new node element whose parent field will point to another new node.
Since we didn't keep track of the parent node pointer, as soon as we lose the childItem pointer we 
have an indirect leak for the parent node.

As we can see below, Valgrind reports a 16 bytes block of indirectly lost (our parent node) as well 
as another 16 bytes block of definitely lost (childItem).

```bash
==6397== 16 bytes in 1 blocks are indirectly lost in loss record 1 of 2
==6397==    at 0x4842003: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==6397==    by 0x1091A8: main (main.cpp:11)
==6397== 
==6397== LEAK SUMMARY:
==6397==    definitely lost: 16 bytes in 1 blocks
==6397==    indirectly lost: 16 bytes in 1 blocks
```

### Possible

You will encounter the type of possible leak whenever heap-allocated memory has not been freed for 
which Valgrind cannot be sure if a pointer exists or not.

In the code below, we have a global int pointer that is used by the main function. The pointer of
that int pointer is passed to the generateNumbers function that allocates the size of 2 int 
(8 bytes of memory on my system).

So after generating the numbers, we are incrementing the pointer (numbers) to the second element so 
Valgrind cannot determine if we still have somewhere another pointer to the first element. That's
why this is reported as a possible leak.

```c++
#include <cstdlib>

int *numbers;

void generateNumbers(int **numbers)
{
    *numbers = malloc(sizeof(int) * 2);
}

int main()
{
    generateNumbers(&numbers);
    numbers++;
    return 0;
}
```

*Valgrind output:*

```bash
==213374== 8 bytes in 1 blocks are possibly lost in loss record 1 of 1
==213374==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==213374==    by 0x109162: generateNumbers(int**) (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==213374==    by 0x109186: main (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==213374==
```

### Reachable

Concerning reachable, this happens whenever heap-allocated memory has not been freed for which 
the program, when reaching the end, still has a pointer to that memory block.

As an example, we will take the same code we used for the Possible leak and remove the numbers++ 
line. That way we still have a pointer to the first item in the allocated block. For Valgrind, this
is still reachable.

```c++
#include <cstdlib>

int *numbers;

void generateNumbers(int **numbers)
{
    *numbers = malloc(sizeof(int) * 2);
}

int main()
{
    generateNumbers(&numbers);
    return 0;
}
```

*Valgrind output:*

```bash
==213828== 8 bytes in 1 blocks are still reachable in loss record 1 of 1
==213828==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==213828==    by 0x109162: generateNumbers(int**) (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==213828==    by 0x109186: main (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==213828== 
```

### Suppressed

What is the suppressed section? From time to time you will be confronted to false positive or 
Valgrind will report memory leaks from 3rd party libraries that you have no control on so you can 
add those leak detection to a suppression file.

Let's suppose I have a project that uses a third party shared library that is already compiled : 
**lib3rdpartylib.so** that of course has a memory leak.

*3rdparty.hpp*
```c++
#pragma once

int getValue();
```

*3rdparty.cpp*
```c++
#include "3rdparty.hpp"

int getValue()
{
    int *retval = new int(99);
    return *retval;
}

```

*Our simple program main.cpp file*
```c++
#include "3rdparty.hpp"
#include <iostream>

int main()
{
    std::cout << "Number : " << getValue() << std::endl;
    return 0;
}

```

Here's the Valgrind output when we run our program with the command 

*valgrind --leak-check=full --show-leak-kinds=all ./build/valgrind_demo*
```bash
==13138== Memcheck, a memory error detector
==13138== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==13138== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==13138== Command: ./build/valgrind_demo
==13138== 
Number : 99
==13138== 
==13138== HEAP SUMMARY:
==13138==     in use at exit: 4 bytes in 1 blocks
==13138==   total heap usage: 3 allocs, 2 frees, 73,732 bytes allocated
==13138== 
==13138== 4 bytes in 1 blocks are definitely lost in loss record 1 of 1
==13138==    at 0x4849013: operator new(unsigned long) (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==13138==    by 0x485C12E: getValue() (in /home/jed/Programming/blog/valgrind/build/lib3rdpartylib.so)
==13138==    by 0x109216: main (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==13138== 
==13138== LEAK SUMMARY:
==13138==    definitely lost: 4 bytes in 1 blocks
==13138==    indirectly lost: 0 bytes in 0 blocks
==13138==      possibly lost: 0 bytes in 0 blocks
==13138==    still reachable: 0 bytes in 0 blocks
==13138==         suppressed: 0 bytes in 0 blocks
==13138== 
==13138== For lists of detected and suppressed errors, rerun with: -s
==13138== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
```

We can see that the leak comes from the lib3rdpartylib.so in the function getValue(). If we want to
suppress this leak because we don't have any control of it, we can use the --gen-suppressions=all
to generate suppression block for each leak.

```bash
valgrind --leak-check=full --show-leak-kinds=all --gen-suppressions=all --quiet ./build/valgrind_demo
Number : 99
==14870== 4 bytes in 1 blocks are definitely lost in loss record 1 of 1
==14870==    at 0x4849013: operator new(unsigned long) (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==14870==    by 0x485C12E: getValue() (in /home/jed/Programming/blog/valgrind/build/lib3rdpartylib.so)
==14870==    by 0x109216: main (in /home/jed/Programming/blog/valgrind/build/valgrind_demo)
==14870== 
{
   <insert_a_suppression_name_here>
   Memcheck:Leak
   match-leak-kinds: definite
   fun:_Znwm
   fun:_Z8getValuev
   fun:main
}

```

The trick is to copy/paste the block { .. } into a file in your project folder ex: .valgrind.supp an
passed it to Valgrind with the option \-\-suppressions

*.valgrind.supp*
```text
{
   <insert_a_suppression_name_here>
   Memcheck:Leak
   match-leak-kinds: definite
   fun:_Znwm
   fun:_Z8getValuev
   fun:main
}
```

Running the command again we can now see that the 3rd party library memory leak is now suppressed.

```bash
valgrind --leak-check=full --show-leak-kinds=all --suppressions=.valgrind.supp ./build/valgrind_demo
==15041== Memcheck, a memory error detector
==15041== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==15041== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==15041== Command: ./build/valgrind_demo
==15041== 
Number : 99
==15041== 
==15041== HEAP SUMMARY:
==15041==     in use at exit: 4 bytes in 1 blocks
==15041==   total heap usage: 3 allocs, 2 frees, 73,732 bytes allocated
==15041== 
==15041== LEAK SUMMARY:
==15041==    definitely lost: 0 bytes in 0 blocks
==15041==    indirectly lost: 0 bytes in 0 blocks
==15041==      possibly lost: 0 bytes in 0 blocks
==15041==    still reachable: 0 bytes in 0 blocks
==15041==         suppressed: 4 bytes in 1 blocks
==15041== 
==15041== For lists of detected and suppressed errors, rerun with: -s
==15041== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 1 from 1)
```

For a few leaks this solution may me manageable, but when you need to repeat the same steps for
hundreds or thousands of leaks this can be problematic. wxWidgets put on their wiki a quick solution
to address multiple leaks using a conversion script. Refer to the [wiki entry](https://wiki.wxwidgets.org/Valgrind_Suppression_File_Howto)
for more details.

## Conclusion

Valgrind with Memcheck is an outstanding tool to find memory leaks in your different projects.
It can save you a lot of trouble in the future. There are many more options that we did not cover 
here. Please refer to the [Valgrind manual](https://valgrind.org/docs/manual/manual.html).

My suggestion to familiarize yourself with Valgrind is to start with a small project. It will be
easier to learn with a small code base.
