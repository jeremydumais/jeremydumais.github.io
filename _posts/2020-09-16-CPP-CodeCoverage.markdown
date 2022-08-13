---
layout: post
title:  "C++ : Code coverage with CMake, Gcov and Lcov"
date:   2020-09-16 19:40:00 -0400
categories: c++
---

Code coverage is a technique aimed at measuring the rate of source code that is covered by the 
various tests of a software.

If for example, I have a source file containing a function that has 10 lines of code and my unit 
tests will pass 8 of them then I will have 80% code coverage.

The implementation of a code coverage system allows the developer to know which parts of the code 
are covered by the tests and which are not.

The tools that will be used for the demo will be [Gcov](http://gcc.gnu.org/onlinedocs/gcc/Gcov.html), 
[Lcov](http://ltp.sourceforge.net/coverage/lcov.php) and [CMake](https://cmake.org/).
GCov is a utility which is part of the GNU Compiler Collection (GCC) suite and which allows to 
generate the exact number of times that the instructions of a program have been executed.
LCov is a tool that allows you to graphically render the results acquired via the GCov application.
CMake, that no longer needs presentation especially in the world of open source, will be the build 
system tool that we will use.

The project below will serve as an example. It's a very simple project that aims to display the 
first and last name of an employee on the screen.

We have our main application in the app folder, a shared library of our data models in the models 
folder, and our unit tests, in Google Test format, in the test folder.

![](/assets/posts/2020-09-code-coverage/projectstructure.png)

The main application (app) simply creates an employee then displays it on the screen.

<center><em>app/src/main.cpp</em></center>

```c++
#include "employee.h"
#include <iostream>

using namespace std;

int main(int argc, char *argv[])
{
    Employee myEmployee(1, "Joe", "Blow");
    cout << myEmployee << "\n";
    return 0;
}
```

Let's take a closer look at the contents of the shared library and more specifically the Employee 
class:

Three private fields: id, firstname and lastName.
1 constructor to initialize our three fields.
Three getter methods in order to be able to retrieve the values of our fields.
Overriding the << operator in order to easily display an employee (first and last name).

<center><em>models/include/employee.h</em></center>

```c++
#pragma once

#include <iostream>
#include <string>

class Employee
{
public:
    Employee(unsigned int id, 
             const std::string & firstName,
             const std::string &lastName);
    unsigned int getId() const;
    const std::string &getFirstName() const;
    const std::string &getLastName() const;
    friend std::ostream& operator<<(std::ostream &os, const Employee &emp);
private:
    unsigned int id;
    std::string firstName;
    std::string lastName;
};
```

<center><em>models/src/employee.cpp</em></center>

```c++
#include "employee.h"
#include <stdexcept>

using namespace std;

Employee::Employee(unsigned int id, 
             const std::string & firstName,
             const std::string &lastName) 
    : id {id},
      firstName {firstName},
      lastName {lastName}
{
    if (id == 0) {
        throw invalid_argument("id must be greater than zero.");
    }
}

unsigned int Employee::getId() const
{
    return id;
}

const std::string& Employee::getFirstName() const
{
    return firstName;
}

const std::string& Employee::getLastName() const
{
    return lastName;
}

std::ostream& operator<<(std::ostream &os, const Employee &emp)
{
    os << emp.firstName << " " << emp.lastName;
    return os;
}
```

If we compile and run the application, we get the following result:

```bash
jed@jed-MS-7593:~/Programming/CPP-CMake-CodeCoverage-Demo$ mkdir build
jed@jed-MS-7593:~/Programming/CPP-CMake-CodeCoverage-Demo$ cd build/
jed@jed-MS-7593:~/Programming/CPP-CMake-CodeCoverage-Demo/build$ conan install ..
Configuration:
[settings]
arch=x86_64
arch_build=x86_64
build_type=Release
compiler=gcc
compiler.libcxx=libstdc++11
compiler.version=7
os=Linux
os_build=Linux
[options]
[build_requires]
[env]

conanfile.txt: Installing package
Requirements
    gtest/1.10.0 from 'conan-center' - Cache
Packages
    gtest/1.10.0:a4062ec0208a59375ac653551e662b6cc469fe58 - Cache

Installing (downloading, building) binaries...
gtest/1.10.0: Already installed!
conanfile.txt: Generator cmake created conanbuildinfo.cmake
conanfile.txt: Generator txt created conanbuildinfo.txt
conanfile.txt: Generated conaninfo.txt
conanfile.txt: Generated graphinfo
jed@jed-MS-7593:~/Programming/test/CPP-CMake-CodeCoverage-Demo/build$ cmake -DCMAKE_BUILD_TYPE=Debug ..
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Conan: Adjusting output directories
-- Conan: Using cmake global configuration
-- Conan: Adjusting default RPATHs Conan policies
-- Conan: Adjusting language standard
-- Current conanbuildinfo.cmake directory: /home/jed/Programming/test/CPP-CMake-CodeCoverage-Demo/build
-- Conan: Compiler GCC>=5, checking major version 7
-- Conan: Checking correct version: 7
-- Appending code coverage compiler flags: -g -fprofile-arcs -ftest-coverage
-- Configuring done
-- Generating done
-- Build files have been written to: /home/jed/Programming/test/CPP-CMake-CodeCoverage-Demo/build
jed@jed-MS-7593:~/Programming/test/CPP-CMake-CodeCoverage-Demo/build$ cmake --build . && ./bin/app
Scanning dependencies of target models
[ 14%] Building CXX object models/CMakeFiles/models.dir/src/employee.cpp.o
[ 28%] Linking CXX shared library ../lib/libmodels.so
[ 28%] Built target models
Scanning dependencies of target app
[ 42%] Building CXX object app/CMakeFiles/app.dir/src/main.cpp.o
[ 57%] Linking CXX executable ../bin/app
[ 57%] Built target app
Scanning dependencies of target codecoverageexample_unittests
[ 71%] Building CXX object test/CMakeFiles/codecoverageexample_unittests.dir/main.cpp.o
[ 85%] Building CXX object test/CMakeFiles/codecoverageexample_unittests.dir/employee_unittest.cpp.o
[100%] Linking CXX executable ../bin/codecoverageexample_unittests
[100%] Built target codecoverageexample_unittests
Joe Blow
```

In order to support code coverage, we need to tell our compiler and linker about it by setting a 
few options. Lars Bilke has created a CMake module 
[CodeCoverage.cmake](https://github.com/bilke/cmake-modules/blob/master/CodeCoverage.cmake) and 
this is the one we will 
be using.

However, you can manually configure the options with -g, -fprofile-arcs and -ftest-coverage among 
others.

The CMakeLists.txt file in the root of the project contains the instructions that will enable code 
coverage options.

```cmake
cmake_minimum_required(VERSION 3.10)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake/modules)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

include(CodeCoverage)
append_coverage_compiler_flags()

enable_testing()

include(GoogleTest)

add_subdirectory("models")
add_subdirectory("app")
add_subdirectory("test")
```

Next, let's write our first unit test. This test creates a new employee which therefore calls the 
constructor of the class.

<center><em>test/employee_unittest.cpp</em></center>

```c++
#include "employee.h"
#include <gtest/gtest.h>

using namespace std;

TEST(Employee_Constructor, AllValidArgs_ReturnSuccess)
{
    Employee sample(1, "Joe", "Blow");
}
```

To launch the code coverage analysis, all you have to do is compile the application, run the unit 
tests and then generate the result, for example in HTML format.

```bash
jed@jed-MS-7593:~/Programming/CPP-CMake-CodeCoverage-Demo/build$ cmake --build . \
> && ctest --progress \
> && lcov -c -d . -o main_coverage.info \
> && genhtml main_coverage.info --output-directory out
```

If we open the resulting file index.html from the build\out folder, we see that our tests, or 
should I rather say our only test, covers 37.5% of our code.

![](/assets/posts/2020-09-code-coverage/coveragefirsttest.png)

Let's see the coverage in detail by clicking on the employee.cpp link. The blue lines indicate that 
they are covered by the tests and those in red indicate that they are not.

![](/assets/posts/2020-09-code-coverage/coverageemployee.png)

We can thus see that our tests do not cover the case of the constructor that would get an id of 
zero. If we add this test and restart everything (compilation/linking/testing/code coverage) we 
will see that this case is now covered and that our code coverage has gone from 37.5% to 43.8%.

```c++
TEST(Employee_Constructor, IdIsZero_ThrowInvalidArgument)
{
    try {
        Employee sample(0, "Joe", "Blow");
        FAIL();
    }
    catch(invalid_argument &err) {
        ASSERT_STREQ("id must be greater than zero.", err.what());
    }
}
```

![](/assets/posts/2020-09-code-coverage/coveragesecondtest.png)

We continue, and so on, until we reach satisfactory code coverage.

The demo presented above is available on my git: [https://github.com/jeremydumais/CPP-CMake-CodeCoverage-Demo](https://github.com/jeremydumais/CPP-CMake-CodeCoverage-Demo)
