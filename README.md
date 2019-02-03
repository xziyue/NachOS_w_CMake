# Some slight modifications
- Removed ```#include "synchlist.h"``` in ```~/threads/synchlist.cc```
- Changed the filename of ```~/userprog/errno.h``` into ```errno_nachos.h``` because it conflicts with the header in C standard library. Note that this file is not included in any project source files so far.

So far, they do not seem to cause a problem when compiling with the original makefiles.