---
layout: post
title:  "C++ : Passing arguments by value, by reference and by pointer"
date:   2020-04-19 15:48:00 -0400
categories: c++
---

The C++ language offers incredible performance of course if you understand well its subtleties. How 
you pass arguments when calling a function or method can make a big difference. The same goes for 
returning method values. Arguments can either be passed by value, by reference or by pointer.


### Pass by value

When we pass an argument by value, we are supplying a copy of the element to the function or method.

*Example :*
```c++
#include <iostream>

using namespace std;

void printLine(string line);

int main(int argc, char* argv[])
{
    string my_line = "Line original";
    cout << "main function:\t\t" << my_line << endl;
    printLine(my_line);
    cout << "main function:\t\t" << my_line << endl;
    return 0;
}

void printLine(string line)
{
    line = "Line updated";
    cout << "printLine function:\t" << line << endl;
}
```

*Execution trace*
```bash
jed@jed-Ubuntu:~/Programming/test$ g++ main.cpp && ./a.out
main function:          Line original
printLine function:     Line updated
main function:          Line original
```

We can see here that the my_line variable is passed as an argument to the printLine function but 
although the line variable is modified inside the function, this has no effect on the my_line 
variable, because when the function exits, the method displays it on the screen and it has its 
original value.

This passing mode should be preferred for core C++ types as listed here: 
[https://www.learncpp.com/cpp-tutorial/73-passing-arguments-by-reference/](https://www.learncpp.com/cpp-tutorial/73-passing-arguments-by-reference/)

Most of these types have a smaller memory footprint than a pointer (4 bytes on x86 systems and 8 
bytes on x64 systems) so it's best to pass them by value.

#### Passing by reference (& symbol)

Pass-by-reference does not use any additional resources compared to pass-by-value, which must 
create a copy of the variable on each call. In the case of a recursive function or in the case 
where the argument has a fairly large memory footprint, this can quickly affect the performance of 
the execution. In order to optimize the resources used, it is in most cases preferable to use 
pass-by-reference.

*Example :*
```c++
#include <iostream>

using namespace std;

void printLine(string &line);

int main(int argc, char* argv[])
{
    string my_line = "Line original";
    cout << "main function:\t\t" << my_line << endl;
    printLine(my_line);
    cout << "main function:\t\t" << my_line << endl;
    return 0;
}

void printLine(string &line)
{
    line = "Line updated";
    cout << "printLine function:\t" << line << endl;
}
```

*Execution trace*
```bash
jed@jed-Ubuntu:~/Programming/test$ g++ main.cpp && ./a.out
main function:          Line original
printLine function:     Line updated
main function:          Line updated
```

We can see in this example that the my_line variable is passed as an argument to the printLine 
function and that the modification made to the line argument inside the function also had the 
effect of modifying the my_line variable. We can clearly see this at the output of the function 
when the content of the my_line variable is displayed on the screen and it has the value 
"Line updated". The line argument was therefore a reference pointing to the memory space of the 
my_line variable.

### Pass by constant reference

Fortunately you can get the best of both worlds by passing the variable by constant reference. In 
this way, no additional resources are used, we cannot modify the argument and we ensure that we 
have no side effects.

If we go back to the previous example, here is the syntax for switching to constant reference:
```c++
void printLine(const string &line);
```

If we run the code below, we can see that modifying the value of the line argument is prohibited.
```c++
void printLine(const string &line)
{
    line = "Line updated";
    cout << "printLine function:\t" << line << endl;
}
```

*Execution trace*
```bash
jed@jed-Ubuntu:~/Programming/test$ g++ main.cpp && ./a.out
main.cpp: In function ‘void printLine(const string&)’:
main.cpp:18:12: error: passing ‘const string {aka const std::__cxx11::basic_string<char>}’ as ‘this’ argument discards qualifiers [-fpermissive]
     line = "Line updated";
            ^~~~~~~~~~~~~~
```

Pass-by-reference mode (constant or not) should be used for all custom types (classes and struct) 
that will always have a (non-null) value.

### Pass by pointer

It is still possible to use the good old pass by pointer method but this one is a bit more complex. 
We'll talk about it again later. Let's see for now how to use our example seen above, but passing 
by pointer.

*Example :*
```c++
#include <iostream>

using namespace std;

void printLine(string *line); 

int main(int argc, char * argv[]) 
{
    string my_line = "Line original";
    cout << "main function:\t\t" << my_line << endl;
    printLine(&my_line);
    cout << "main function:\t\t" << my_line << endl;
    return 0;
}

void printLine(string *line)
{
    *line = "Line updated";
    cout << "printLine function:\t" << *line << endl;
}
```

*Execution trace*
```bash
jed@jed-MS-7593:~/Programming/test$ g++ src/main.cpp && ./a.out
main function:          Line original
printLine function:     Line updated
main function:          Line updated
```

We get exactly the same result as passing by reference. No copy of value has been made, in fact 
what we passed as an argument is a copy of the pointer of the variable my_line which contains the 
memory address from where the value "Line original" is stored.

Notice in line 18 that we must use the dereferencing operator * to assign "Line updated" to the 
memory space pointed to by the pointer. Same principle on line 19 to retrieve the data from the 
memory address in question.

Here's what happens if you don't use the dereferencing operator * on line 19:
```bash
jed@jed-MS-7593:~/Programming/test$ g++ src/main.cpp && ./a.out
main function:          Line original
printLine function:     0x7ffc1d2856d0
main function:          Line updated
```

It is the memory address contained by the pointer which is displayed instead of the value of what 
is contained at this address.

### Pass by constant pointer

There are two ways to do it:

- Using the const string * line notation ensures that the contents of the pointer cannot be changed.
- Using the string * const line notation ensures that the pointer address cannot be changed.

```c++
void printLine(const string * line)
{
    *line = "Line updated"; //Error we cannot change the value contained by the pointer
    cout << "printLine function:\t" << *line << endl;
}
```
```c++
void printLine(string * const line)
{
    string lineUpdated = "Line updated";
    line = &lineUpdated; //Error cannot change pointer address
    cout << "printLine function:\t" << *line << endl;
}
```

We can also use the combination of the two methods in order to have the total: const string * const 
line.
```c++
void printLine(const string * const line)
{
    string lineUpdated = "Line updated";
    line = &lineUpdated; //Error cannot change pointer address
    *line = lineUpdated; //Error we cannot change the value contained by the pointer
    cout << "printLine function:\t" << *line << endl;
}
```

Pointer passing mode (constant or not) should be used for all custom types (classes and struct) 
when they can be null. If for example we want to pass an instance of a class that we are going to 
call Employee but it is possible that the instance is null then we must use a pass by pointer. This 
mode can also be very useful when you want to modify the pointer received to make it point to 
another object.

### Conclusion

Throughout the development of an application we may have to work with the 3 modes, so it is 
important to know the usefulness of each of these modes and when we should use them.
