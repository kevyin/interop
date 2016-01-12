# Installation                          {#install}

The Illumina Open Source InterOp Library provides several ways to obtain the library for your 
system including pre-compiled binaries for select platforms as well as a procedure to build the library 
from the source code.

## Download Binary

It is recommended to start from one of the binary distributions found under releases. The CentOS 5 build
should work on a wide variety of Linux distributions.

## Build from Source

This section describes the prerequisites for building the source, describes how to download the source and
how to build the source. Carefully consider the prerequisites before trying to build from the source.

### Prerequisites

The following are the minimum requirements to build the source:

- C/C++ Compiler (C++98)
    - Windows: [MinGW], [Cygwin], Microsoft Visual C++, [Express]
    - Linux: GCC, CLang
    - Mac OSX: CLang
- [CMake] Version 3.2
- [RapidXML] v1.3 - This is automatically downloaded if not found or specified

The following optional features have the corresponding requirements:

- Running unit tests
    - [Google Test] - This is automatically downloaded if not found or specified
- Wrapping the library for C#
    - [SWIG] 3.x or later
    - C# compiler: Visual Studio or [Mono]
- Running C# Unit Tests:
    - [NUnit] - This is automatically downloaded if not found or specified
- Building the documentation
    - [Doxygen]
- Clone the Source Code from the Version Control Repository
    - [Git]

Tips for Prerequisites
    
  - CMake and SWIG will likely need to be installed from the source on Linux
  - Building on a machine not connected to the Internet: download and provide necessary dependencies such as RapidXML

[NUnit]: http://www.nunit.org/
[Git]: https://git-scm.com/
[Doxygen]: http://www.stack.nl/~dimitri/doxygen/
[Mono]: http://www.monodevelop.com/
[SWIG]: http://www.swig.org/
[RapidXML]: http://rapidxml.sourceforge.net/
[CMake]: https://cmake.org/
[Express]: https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx
[Cygwin]: https://www.cygwin.com/
[MinGW]: http://sourceforge.net/projects/mingw-w64/
[Google Test]: https://code.google.com/p/googletest/

### Download the Source

If you must build from source, getting the [LatestStableRelease] is strongly recommended. This build is more
rigorously tested and will have less stringent build requirements.

If the latest stable version does not meet your needs, then you may consider using the [LatestVersion].

[LatestStableRelease]: https://github.com/IlluminaBioinformatics/interop/releases/latest "Latest Stable Release"
[LatestVersion]: https://github.com/IlluminaBioinformatics/interop/archive/master.zip "Bleeding-edge Release"

### Clone the Latest Source

The code may be "cloned" using the git version control program as follows:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
git clone git@github.com:IlluminaBioinformatics/interop.git
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After cloning the code, you may update your code for any changes made since you lasted cloned by 
running the following commands:

~~~~~~~~~~~~~{.sh}
cd interop
git pull
~~~~~~~~~~~~~

### Build the Source

The following code will work for most platforms. 

~~~~~~~~~~~~~~~{.sh}
mkdir Build
cd Build
cmake ../interop
cmake --build .
~~~~~~~~~~~~~~~

The user should create a directory outside of interop to build the binary, here we called it `Build`. After
entering the directory, the user can configure the build system with:

~~~~~~~~~~~~~{.sh}
cmake ../interop
~~~~~~~~~~~~~

Note, the user can obtain a list of the available generators using (for an alternative to the default):

~~~~~~~~~~~~~{.sh}
cmake --help
~~~~~~~~~~~~~

Then, specify the appropriate generator using the `-G` flag:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
cmake ../interop -G “Visual Studio 12 2013 Win64”
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Finally, the binary can be built in a platform independent manner with the following command:

~~~~~~~~~~~~~~~{.sh}
cmake --build .
~~~~~~~~~~~~~~~

### Configure the Source

There are several configuration options that you may wish to consider:

| Option Name                     | Description                                                                         |
| ------------------------------- | ------------------------------------------------------------------------------------|
| ENABLE_BACKWARDS_COMPATIBILITY  | Compile code for c++98 otherwise it compiles for c++11                              |
| ENABLE_DOCS                     | Build documentation with Doxygen                                                    |
| ENABLE_SWIG                     | Build third-party language bindings, e.g. C#                                        |
| ENABLE_TEST                     | Build unit tests (depends on Boost)                                                 |
| ENABLE_APPS                     | Build command line programs                                                         |
| ENABLE_STATIC                   | Build static libraries instead of dynamic                                           |
| FORCE_X86                       | Force 32-bit libraries instead of platform default (Does nothing for Visual Studio) |
| RAPIDXML_ROOT                   | Location of RapidXML headers. If not set, then it will auto download                |
| GTEST_ROOT                      | Optional. Location of GTest installation. If not set, then it will auto download    |

CMake also has several built-in options that you may use:

| Option Name                     | Description                                                                         |
| ------------------------------- | ------------------------------------------------------------------------------------|
| CMAKE_BUILD_TYPE                | Type of build: Debug, Release, RelWithDebInfo                                       |
| CMAKE_INSTALL_PREFIX            | Directory to install InterOp                                                        |

More options can be found at: https://cmake.org/cmake/help/v3.4/manual/cmake-variables.7.html

These options should set as follows when configuring CMake

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
cmake ../interop -DENABLE_BACKWARDS_COMPATIBILITY=ON
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you need to reconfigure the build, it is recommended that you delete everything in the build directory first.

The optional C# binding has the following options that may (or may not) need to be set. These need to be set if
the configuration fails to find the appropriate executable or you wish to change the version of the C# compiler.

| Option Name                     | Description                                                                         |
| ------------------------------- | ------------------------------------------------------------------------------------|
| CSHARP_MONO_VERSION             | Version of the mono compiler                                                        |
| CSHARP_DOTNET_VERSION           | Version of DOT NET to use   (v2.0.50727, v3.5 or v4.0.30319)                        |
| NUNIT_ROOT                      | Prefix of NUnit unit test library. Necessary to set only if auto download fails     |
| SWIG_EXECUTABLE                 | Location of SWIG executable                                                         |

When you build the C# DLL, you currently need to run the CMake configure and build steps twice. The first step
generates the C# *.cs files and the second builds the DLL from these files.

### Example configurations

Using pre-downloaded GTest and RapidXML packages assuming:
    
    - The packages have already been unarchived
    - The packages were placed at the same level as InterOp
    
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
    cmake ../interop -D GTEST_ROOT=../../gtest -D RAPIDXML_ROOT ../../rapidxml -DENABLE_TEST=ON
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Build C# binding using SWIG on Windows assuming:
    
    - Visual Studio is installed
    - Using latest .NET Version 4.x
    - SWIG could not be found automatically, but is installed in c:\Swig

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
    cmake ../interop -D SWIG_EXECUTABLE=c:\Swig\swig.exe -DCSHARP_DOTNET_VERSION=v4.0.30319 -DENABLE_SWIG=ON
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Build C# binding using SWIG on Windows assuming:
    
    - Visual Studio is installed
    - Using latest .NET Version 3.x
    - SWIG could not be found automatically, but is installed in c:\Swig

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
    cmake ../interop -D SWIG_EXECUTABLE=c:\Swig\swig.exe -DENABLE_SWIG=ON
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~