# Building with Make

[GNU make](www.gnu.org/software/make/) is a very popular and widely used 
system for building software. If you are already using Make to build your 
software, this example may be useful for incorporating tskit into your 
C/C++ project. 

For simplicity, we use ``git`` to obtain code from GitHub. However, this is 
probably not a good idea if end users are using the Makefile to build your software 
(they may not have git installed, etc). In this case it may be better to 
ensure that the tskit and kastore code is included in your distribution 
tarball.

This is a very simple example, and in no way represents best practices.
