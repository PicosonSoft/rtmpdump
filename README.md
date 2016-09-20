# rtmpdump
A fork of ffmpeg's rtmpdump utility.

Building with Visual Studio requires a compatible build of openssl,
so either get a binary somewhere or build your own from source,
the following guide works for openssl-1.0.2e:

http://developer.covenanteyes.com/building-openssl-for-visual-studio/

Newer openssl versions may work as well, but that was the latest version at the time
this fork was created.

Once the library has been built, either move/copy/install the directories lib, bin and include to a folder named "runtime"
at the root of this repository OR set the CMake RUNTIME_PATH or CMAKE_PREFIX_PATH variable to wherever those openssl directories are.

For the original project documentation please see the README file.
