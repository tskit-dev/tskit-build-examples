# Building with Meson

The [meson build system](https://mesonbuild.com) is a very powerful and
convenient way to build C/C++ applications in a platform independent manner.
It is used by a [growing list](https://mesonbuild.com/Users.html) of large
open-source projects: one of the key advantages of meson is very fast incremental
build times when using the default [ninja backend](https://ninja-build.org).

One of the features of Meson is the ability to specify dependencies via
[subprojects](https://mesonbuild.com/Subprojects.html), which make compiling
local versions of a particular library very easy. Tskit uses Meson as
its build system and supports being built as a Meson subproject.

## Trying out this example

If you want to try out the example in this repo without setting up your own,
you need to clone a copy to your local machine and run ``meson build`` in this
directory.

```
The Meson build system
Version: 0.62.0
Source dir: /home/jk/work/github/tskit-build-examples/meson
Build dir: /home/jk/work/github/tskit-build-examples/meson/build
Build type: native build
Project name: example
Project version: undefined
C compiler for the host machine: cc (gcc 9.4.0 "cc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0")
C linker for the host machine: cc ld.bfd 2.34
Host machine cpu family: x86_64
Host machine cpu: x86_64

Executing subproject tskit

tskit| Project name: tskit
tskit| Project version: 1.0.0
tskit| C compiler for the host machine: cc (gcc 9.4.0 "cc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0")
tskit| C linker for the host machine: cc ld.bfd 2.34
tskit| C++ compiler for the host machine: c++ (gcc 9.4.0 "c++ (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0")
tskit| C++ linker for the host machine: c++ ld.bfd 2.34

Executing subproject tskit:kastore

kastore| Project name: kastore
kastore| Project version: 2.1.1
kastore| C compiler for the host machine: cc (gcc 9.4.0 "cc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0")
kastore| C linker for the host machine: cc ld.bfd 2.34
kastore| C++ compiler for the host machine: c++ (gcc 9.4.0 "c++ (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0")
kastore| C++ linker for the host machine: c++ ld.bfd 2.34
kastore| Build targets in project: 1
kastore| Subproject kastore finished.

tskit| Library m found: YES
tskit| Build targets in project: 2
tskit| Subproject tskit finished.

Build targets in project: 3

example undefined

  Subprojects
    kastore: YES
    tskit  : YES

Found ninja-1.10.2.git.kitware.jobserver-1 at /home/jk/.local/bin/ninja

```

This creates a build environment in the ``build`` directory. We can then compile this
using ninja:
```
$ ninja -C build
ninja: Entering directory `build'
[12/12] Linking target example.
```
Finally, we can then run the example program:
```
$ ./build/example
```
