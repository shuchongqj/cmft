[cmft](https://github.com/dariomanesku/cmft) - cubemap filtering tool
=====================================================================

What is it?
-----------

Cross-platform open-source command-line cubemap filtering tool.<br \>
Utilizes both host CPU and OpenCL GPU at the same time for fast processing! (check [perfomance charts](https://github.com/dariomanesku/cmft#performance)) <br \>

![cmft-cover](https://github.com/dariomanesku/cmft/raw/master/res/cmft_cover.jpg)

- Supported input/output formats: \*.dds, \*.ktx, \*.hdr, \*.tga.
- Supported input/output types: cubemap, cube cross, latlong, face list, horizontal strip.


See it in action - [here](https://github.com/dariomanesku/cmftViewer)
----------------
Shading with a prefiltered cubemap:
![cmftViewer8](https://github.com/dariomanesku/cmftViewer/raw/master/screenshots/cmftViewer8.jpg)


Download
--------

Download binaries here: <br \>
 * [cmft - Windows 32bit](https://github.com/dariomanesku/cmft-bin/raw/master/cmft_win32.zip)<br />
 * [cmft - Windows 64bit](https://github.com/dariomanesku/cmft-bin/raw/master/cmft_win64.zip)<br />
 * [cmft - Linux 32bit](https://github.com/dariomanesku/cmft-bin/raw/master/cmft_lin32.zip)<br />
 * [cmft - Linux 64bit](https://github.com/dariomanesku/cmft-bin/raw/master/cmft_lin64.zip)<br />
 * [cmft - OSX 32bit](https://github.com/dariomanesku/cmft-bin/raw/master/cmft_osx32.zip)<br />
 * [cmft - OSX 64bit](https://github.com/dariomanesku/cmft-bin/raw/master/cmft_osx64.zip)<br />

Notice: Linux and OSX binaries are outdated. For now, on those systems, compile from source.

Building
--------

	git clone git://github.com/dariomanesku/cmft.git
	cd cmft
	make

- After calling `make`, *\_projects* folder will be created with all suppored project files. Deleting *\_projects* folder is safe at any time.<br \>
- All compiler generated files will be in *\_build* folder. Again, deleting *\_build* folder is safe at any time.<br \>

### Windows

- Visual Studio solution can be found in *\_projects/vs2012/*.<br \>

### Linux

- Makefile can be found in *\_projects/gmake-linux-gcc/*.<br \>
- Project can be build from the root directory by running `make linux-gcc-release64` (or similar).<br \>

### OS X

- XCode
  - XCode solution can be found in *\_projects/xcode4/*.<br \>
  - XCode project generated by premake contains only one scheme with 4 build configurations (debug/release 32/64bit). Select desired build configuration manually and/or setup schemes manually as desired.<br \>
  - Also you will probably have to manually specify runtime directory (it is not picking it from premake for some reason). You can do this by going to "*Product -> Scheme -> Edit Scheme... -> Run cmftDebug -> Options -> Working Directory (Use custom working directory)*" and specifying *runtime/* directory from cmft root folder.<br \>
- Makefile
  - Makefile can be found in *\_projects/gmake-osx-gcc/*.<br \>
  - Project can be build from the root directory by running `make osx-gcc-release64` (or similar).<br \>
  <br \>
- Remember to edit *runtime/cmft_osx.sh* accordingly in regard to the build system you are using.<br \>

### Other
- Also other compilation options may be available, have a look inside *\_projects* directory.<br \>
- File *config.mk* is used for setting environment variables for different compilers.<br \>
- Aditional build configurations will be available in the future. If one is there and not described here in this document, it is probably not yet set up properly and may not work out-of-the-box as expected without some care.<br \>

Using
-----
- Use *runtime/cmft\_win.bat* on Windows, *runtime/cmft\_lin.sh* on Linux and *runtime/cmft\_osx.sh* on OSX as a starting point.<br \>
- First compile the project, edit *runtime/cmft\_*\* file as needed and then run it.<br \>
- Notice: your screen will freeze during OpenCL execution on the GPU (if the screen is connected to that same GPU). This is because cmft is using one kernel per cubemap face that usually takes a lot of time to execute and durring that time GPU is 100% dedicated to the task and unresponsive to the OS. In the future, cmft will be changed to use smaller kernels to avoid this problem.<br \>

Project status
--------------

- There are still issues to be fixed. Before using cmft in production, wait until [cmftViewer](https://github.com/dariomanesku/cmftViewer) gets released. Throughout [cmftViewer](https://github.com/dariomanesku/cmftViewer) development, all cmft major problems should be discovered and fixed. [cmftViewer](https://github.com/dariomanesku/cmftViewer) will also be a good showcase of what exactly you can get and expect from cmft, so stay tuned.

### Known issues
- Linux GCC build works but processing on CPU is noticeably slower comparing to Windows build (haven't yet figured out why). OpenCL runs fine.
- PVRTexTool is not properly opening mipmapped \*.ktx files from cmft. This appears to be the problem with the current version of PVRTexTool. Has to be further investigated.

Performance
-----------

cmft was compared with the popular CubeMapGen tool for processing performance.<br \>
Test machine: Intel i5-3570 @ 3.8ghz, Nvidia GTX 560 Ti 448.

Filter settings:
- Gloss scale: 8
- Gloss bias: 1
- Mip count: 8
- Exclude base: false

Test case #1:
- Src face size: 256
- Dst face size: 256
- Lighting model: phongbrdf

Test case #2:
- Src face size: 256
- Dst face size: 256
- Lighting model: blinnbrdf

Test case #3:
- Src face size: 512
- Dst face size: 256
- Lighting model: phongbrdf

Test case #4:
- Src face size: 512
- Dst face size: 256
- Lighting model: blinnbrdf

<br />

|Test case| CubeMapGen   | cmft Cpu only | cmft Gpu only | cmft  |
|:--------|:-------------|:--------------|:--------------|:------|
|#1       |01:27.7       |00:08.6        |00:18.7        |00:06.0|
|#2       |04:39.5       |00:29.7        |00:19.6        |00:11.2|
|#3       |05:38.1       |00:33.4        |01:03.7        |00:21.6|
|#4       |18:34.1       |01:58.2        |01:07.7        |00:35.5|

![cmft-performance-chart](https://github.com/dariomanesku/cmft/raw/master/res/cmft_performance_chart.png)

Recommended tools
-----------------

- [PVRTexTool](http://community.imgtec.com/developers/powervr/) - for opening \*.dds and \*.ktx files.<br />
- [GIMP](http://www.gimp.org) - for opening \*.tga files.<br />
- [Luminance HDR](http://qtpfsgui.sourceforge.net/) - for opening \*.hdr files.<br />

Environment maps
----------------

For testing purposes, you can use environment maps from [here](https://github.com/dariomanesku/environment-maps).<br />
Also, have a look at [hdrlabs.com archive](http://www.hdrlabs.com/sibl/archive.html).

Similar projects
----------------

- [CubeMapGen](http://developer.amd.com/tools-and-sdks/archive/legacy-cpu-gpu-tools/cubemapgen/) - A well known for cubemap filtering from AMD.<br \>
- [Marmoset Skyshop](http://www.marmoset.co/skyshop) - Commercial plugin for Unity3D Game engine.

Useful links
------------

1. [Sebastien Lagarde Blog - AMD Cubemapgen for physically based rendering](http://seblagarde.wordpress.com/2012/06/10/amd-cubemapgen-for-physically-based-rendering/) by [Sébastien Lagarde](https://twitter.com/SebLagarde)
2. [The Witness Blog - Seamless Cube Map Filtering](http://the-witness.net/news/2012/02/seamless-cube-map-filtering/) by [Ignacio Castaño](https://twitter.com/castano)
3. ...more to come

Contributors
------------

* Cover photo design by Marko Radak. Check out his [personal website](http://markoradak.com/).

More to come
------------

- [cmftViewer](https://github.com/dariomanesku/cmftViewer) for viewing filtered cubemaps in action. [cmftViewer](https://github.com/dariomanesku/cmftViewer) will be as well cross-platform and open-source implemented using [bgfx rendering library](https://github.com/bkaradzic/bgfx/).
- Tutorial and details on theory and implementation.

Get involved
------------

In case you are using cmft for your game/project, please let me know. Tell me your use case, what is working well and what is not. I will be happy to help you using cmft and also to fix possible bugs and extend cmft to match your use case.

Other than that, everyone is welcome to contribute to cmft by submitting bug reports, feature requests, testing on different platforms, profiling and optimizing, etc.

When contributing to the cmft project you must agree to the BSD 2-clause licensing terms.

Keep in touch
-------------

Add me and drop me a line on twitter: [@dariomanesku](https://twitter.com/dariomanesku).


[License (BSD 2-clause)](https://github.com/dariomanesku/cmft/blob/master/LICENSE)
-------------------------------------------------------------------------------

    Copyright 2014 Dario Manesku. All rights reserved.

    https://github.com/dariomanesku/cmft

    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions are met:

       1. Redistributions of source code must retain the above copyright notice,
          this list of conditions and the following disclaimer.

       2. Redistributions in binary form must reproduce the above copyright notice,
          this list of conditions and the following disclaimer in the documentation
          and/or other materials provided with the distribution.

    THIS SOFTWARE IS PROVIDED BY COPYRIGHT HOLDER ``AS IS'' AND ANY EXPRESS OR
    IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
    MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
    EVENT SHALL COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT,
    INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
    BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
    DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
    LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE
    OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
    ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
