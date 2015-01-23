
# Compose Conference Z3 tutorial

## Intro 
Intro to Z3 via F#.

## What you need to have ready

I'll assume you already have a fully-fledged dev machine 
(Windows + Visual Studio 2013, OS X with XCode dev tools, Linux with g++, etc). 

We will be using Z3 from F#'s repl (fsi on Windows, fsharpi on Mac OS X and Linux). 
You now need to have both F# and Z3, with it's .NET bindings, installed on your machine: 


You can use the Z3 dlls in /platform/{windows,osx-mono}. Or get/build from the source:


### Using Windows

1. Visual Studio 2013 will already give you F# (fsi, fsc).

2. You can get Z3 in several ways:
  1. If you trust me, just pick up the dlls from platform/windows.
  2. If you trust the Z3 guys, download the Windows binaries from http://z3.codeplex.com. 
  3. If you want to build yourself, download the source from http://z3.codeplex.com and build it yourself. 
	
	
### Using Mono (for Linux and MacOS)

1. Install software needed for the build process:

	* g++
	* python
	* mono for .NET 4.0
	* xbuild
	* fsharp

    On a Debian (>> squeezy) or Ubuntu (>= 14.04 LTS) system, this suffices:
```
$ sudo apt-get install build-essential python mono-complete mono-xbuild fsharp
```

On OS X, install the Mono MDK for Mac OS from
       http://www.mono-project.com/download/
and install the XCode development tools (e.g., by executing "gcc" in
a Terminal -- if it's not there yet, OS X will offer to install XCode).

2. Get Z3. 
	1. If you trust me, you can just use the dlls in platform/osx-mono;
	2. Or else you can build z3 youself 
		
		For this, get z3 sources for z3/unstable (4.3.2 is known to work) from
		http://z3.codeplex.com/. 

```
		export Z3DIR=/path/to/z3/,  and follow these steps:
```
    On Linux, this suffices:
```
      $ pushd "$Z3DIR"
      $ ./configure
      $ pushd "$Z3DIR/build"
      $ make
      $ popd && popd
```

    On OS X, you need to enforce a 32bit build (for compatibility with Mono):
```
      $ pushd "$Z3DIR"
      $ ./configure
      $ pushd "$Z3DIR/build"
      $ perl -i -pe 's/-D_AMD64_/-arch i386/; s/LINK_EXTRA_FLAGS=/$&-arch i386 /' config.mk
      $ make
      $ popd && popd
```

	Now build the .NET bindings for z3:
	```
      $ pushd "$Z3DIR/src/api/dotnet/"
      $ echo -e '<configuration>\n <dllmap dll="libz3.dll" target="libz3.dylib" os="osx"/>\n <dllmap dll="libz3.so" target="libz3.dylib" os="linux"/>\n</configuration>\n' > Microsoft.Z3.config
      $ xbuild Microsoft.Z3.csproj
SI: 1 Warning(s)
      $ popd
	```


Now we're ready to rock n'roll: '

Here, we're starting fsharpi on OS X and assuming the Z3 dlls are in ../platform/osx-mono. Your setup might be different.

```
$ fsharpi -I:../platform/osx-mono -r:Microsoft.Z3
> open Microsoft.Z3 ;;
> let ctx = new Context ();;

val ctx : Context

> 
```