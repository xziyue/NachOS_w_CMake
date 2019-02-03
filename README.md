## Test on different OS
- [ ] g++ Apple LLVM version 10.0.0 (clang-1000.10.44.4)

**(Failed)** Due to the fact that Apple no longer provides i386 support. However, it is non-trivial to adapt NachOS for 64-bit mode. If someone's Apple compiler still supports 32-bit mode, please inform me the compilation results.

## Some slight modifications
- Removed ```#include "synchlist.h"``` in ```~/threads/synchlist.cc```
- Changed the filename of ```~/userprog/errno.h``` into ```errno_nachos.h``` because it conflicts with the header in C standard library. Note that this file is not included in any project source files so far.

So far, they do not seem to cause a problem when compiling with the original makefiles.