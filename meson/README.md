# Building with Meson

The [meson build system](https://mesonbuild.com) is a very powerful and 
convenient way to build C/C++ applications in a platform independent manner.
It is used by a [growing list](https://mesonbuild.com/Users.html) of large 
open-source projects: one of the key advantages of meson is very fast incremental
build times when using the default [ninja backend](https://ninja-build.org).

One of the features of Meson is the ability to specify dependencies via
[subprojects](https://mesonbuild.com/Subprojects.html), which make compiling
local versions of a particular library very easy. Tskit uses Meson as 
it's build system and supports being built as a Meson subproject.

This example documents one way in which this can be set up using 
[Git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
and soft-links.

## Trying out this example

If you want to try out the example in this repo without setting up your own,
you need to clone a copy to your local machine and run ``meson build`` in this 
directory.

To ensure that the git submodules are correctly initialised, you need to 
either run ``git clone git@github.com:tskit-dev/tskit-build-examples.git --recurse-submodules``
or ``git submodule update --init --recursive`` if you have already cloned the repo.

## Setting up the meson sub-projects

Because multi-language structure of the kastore and tskit repos we 
need to do a little trickery with soft-links to get things in the form
that meson requires for subprojects.

First we add submodules for the two repos and check out the 
latest releases:
```
$ git submodule add https://github.com/tskit-dev/kastore.git subprojects/git-submodules/kastore
$ git submodule add https://github.com/tskit-dev/tskit.git subprojects/git-submodules/tskit
$ git commit -a -m 
$ cd subprojects/git-submodules/kastore/
$ git checkout C_1.0.1
$ cd ..
$ git commit -a -m "Checked out kastore at release 1.0.1."
$ cd tskit
$ git checkout C_0.99.1
$ cd ..
$ git commit -a -m "Checked out tskit at release 0.99.1"

```

Returning to the project root (where the ``meson.build`` file is) we then see
the following:
```
$ ls subprojects/git-submodules/
kastore  tskit
```
Now we need to create some soft-links to directories within these git submodules
so that meson can find their build files:

```
$ cd subprojects
$ ln -s git-submodules/tskit/c tskit
$ ln -s git-submodules/kastore/c kastore
$ git add kastore tskit
$ git commit -a -m "Added soft links for submodule directories"
$ cd ..
$ ls -lh subprojects/
total 4.0K
drwxr-xr-x 4 jk jk 4.0K Jan 24 15:29 git-submodules
lrwxrwxrwx 1 jk jk   24 Jan 24 15:40 kastore -> git-submodules/kastore/c
lrwxrwxrwx 1 jk jk   22 Jan 24 15:39 tskit -> git-submodules/tskit/c
```

Now we've created the expected directory structure, we can set up the 
build:

```
$ meson build
The Meson build system
Version: 0.48.2
Source dir: /home/jk/work/github/tskit-build-examples/meson
Build dir: /home/jk/work/github/tskit-build-examples/meson/build
Build type: native build
Project name: example
Project version: undefined
Native C compiler: cc (gcc 7.3.0 "cc (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0")
Build machine cpu family: x86_64
Build machine cpu: x86_64

|
|Executing subproject kastore
|
|Project name: kastore
|Project version: undefined
|Native C compiler: cc (gcc 7.3.0 "cc (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0")
|Native C++ compiler: c++ (gcc 7.3.0 "c++ (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0")
|Build targets in project: 1
|
|Subproject kastore finished.

|
|Executing subproject tskit
|
|Project name: tskit
|Project version: undefined
|Native C compiler: cc (gcc 7.3.0 "cc (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0")
|Native C++ compiler: c++ (gcc 7.3.0 "c++ (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0")
|Library m found: YES
|Build targets in project: 2
|
|Subproject tskit finished.
Build targets in project: 3
Found ninja-1.8.2 at /usr/bin/ninja
```

This creates a build environment in the ``build`` directory. We can then compile this
using ninja:
```
$ ninja -C build
ninja: Entering directory `build'
[11/11] Linking target example.
```
Finally, we can then run the example program:
```
$ ./build/example
```
