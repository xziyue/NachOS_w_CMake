## Remember, always check your compilation results with the original Makefiles!
## USE AT YOUR OWN RISK!
----

## What is this repo?
It is essentially the original NachOS files (with some small modifications, which are listed below) provided on Blackboard, plus a ```CMakeLists.txt``` - a CMake configuration for NachOS.

## Why CMake?

Because CMake is a cross-platform building tool. More importantly, a very powerful IDE - JetBrains CLion, works perfectly with CMake. It is completely for free for college students (one only needs to register with their university email), and it is available on almost all platforms: Windows, OS X, Linux... you name it. With the help of IDE, we can write code and debug them much more efficiently.

- To import this project into CLion: open the code folder with CLion. (Although one can also open the root folder, he/she may need to manually open ```CMakeLists.txt``` and load CMake project in CLion. It will take a longer time for CLion to figure out what the project structure is.)

- To run NachOS without any parameter: simply press the play button at the top right corner of CLion

![cion_run](./image/clion_build_bar.png)

- In order to run with parameters, we can setup a run config in CLion

![clion_config](./image/clion_run_config.png)

- We can set a breakpoint and debug just like in other IDEs (e.g. MSVC)!

![clion_debug](./image/clion_debug.png)

## Test on different OS'
- [x] g++ (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0
- [ ] g++ Apple LLVM version 10.0.0 (clang-1000.10.44.4)

**(Failed)** Due to the fact that Apple no longer provides i386 support, I cannot compile NachOS on my Mac. It is nontrivial to adapt NachOS for 64-bit mode. I am afraid that I might make some mistakes, so I am not doing it for you guys.

If someone's Apple compiler still supports 32-bit mode, please inform me of the compilation results.

## Some slight modifications
- Removed ```#include "synchlist.h"``` in ```~/threads/synchlist.cc```
- Changed the filename of ```~/userprog/errno.h``` into ```errno_nachos.h``` because it conflicts with the header in C standard library. Note that this file is not included in any project source files so far.

So far, they do not seem to cause a problem when compiling with the original Makefiles.

## If you like my work, please star the repo. Thank you very much!