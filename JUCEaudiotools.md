# Compiling supercolider and the sc3 plugins

## JUCE && Projucer
requirements:
1. freetype2
2. xinerama
3. webkit2gtk-4.0
4. gtk+-x11-3.0

1. Find the Projucer linux makefile
2. Make and manually install into the directory
3. Find the AppConfig.h file and change the JUCER_ENABLE_GPL_MODE to 1

## supercollider
1. git clone <supercollider>
2. follow the readme and install the packages. Problems with different versions of repo Qt and Qt webengine and Qt webkit seems to have been solved, so if you're using the latest version of ubuntu, you're not likely to need downloading of the qt source or finding it, and going through the hassle of configuring and installing qt, the repository version works
3. make

## sc3-plugins
Things to note:   
1. The sc3-plugins from github can be configured to install the plugins into the ./.local/lib/SuperCollider/plugins
2. The scide is hardcoded to look into ./.local/share/SuperCollider/plugins directory instead
3. Shift the shared object files into the plugins directory or softlink them

## pure data
And the same with puredata 0.49,
1. git clone <puredata>
2. Right, so the up to date distro, means you don't need to go hunting around for an appropriate tcl/tk installation and figuring out what to install, since the repo/distro contains a usable version 
2.1 If not, install tcl/tk seperately. There isn't as much versioning hurdles as Qt. 
3. ignored the install instruction for the makefile.gnu ( it's not there anyway )
4. run the autogen.sh to generate the configure script
5. ./configure .... had to think about portmidi ( is this needed? )
6. make

## configuring helm and dexed with JUCE

1. git clone <JUCE from the we are roli site>
2. find the projucer directory 
3. There's a juce build directory with the linux makefiles, do a make in that dir

Once you have a projucer, and copied it to a bin directory that is in $PATH,
for dexed;
1. open the juce project file
2. resolve all issues with the path settings. 
2.1 change the global path to point to the correct JUCE directory
2.2 ( for dexed ) ,  add the path to a working vst2 directory in the juce project
3. save and quit, the buildfiles will be generated per os target 
4. cd to the linux build directory and make

for helm: make and probably resolve the paths

## HELM

### installation
1. The global makefile will build the vst, lv2 and standalone version
2. If it complains about the vst aeffect.h that is missing, add the vst library path to the jucer file

## faust
Faust is a meta language transpiler with various audio language targets, such as pure data, supercollider, c, c++, webaudio

1. git clone <faust>
2. install from the repo libmicrohttpd{-dev} and llvm
3. cd build ; mkdir <builddir> ; cd <builddir>
Note: llvm used to have 2 flavours for compilation, so -fno-rtti and rtti. By default rtti is in place. But all linked/compiled programs needed to be one of the two. Past llvm-6 this is not an issue
4. cmake ../ -DCMAKE_INSTALL_PREFIX=/home/ubs/.local -DLLVM_CONFIG=llvm-config-8 -C ../backends/all.cmake

## Compiling surge the synthesizer

### prequisite files 
				build-essential
				libcairo-dev
				libxkbcommon-x11-dev
				libxkbcommon-dev
				libxcb-cursor-dev
				libxcb-keysyms1-dev
				libxcb-util-dev

1. git clone <surge github>
2. find the steinberg vst2 sdk. There are a few of those around, including the one that copies files from the vst2 sdk into the vst3sdk
3. in the surge directory, following their build instructions, you would have git cloned the vst3sdk
4. for some reason, even with the VST2SDK_DIR exported with correct vst3sdk path, the build program will still look for vst3sdk in the surge directory. So do the copy_from_vst2_to vst3 for the vst3sdk directory in surge. 
5. Carry on the build process

### install direcotries
/usr/lib/vst
/usr/lib/vst3
~/.vst
~/.local/share/Surge

### premake for surge
1. surge requires premake to confgure the files for making
2. repository premake does not work with the current surge, so you'll have to download the premake from github
3. in the premake directory
3.1 make -f BootStrap.mak linux # to create a premake binary
3.2 using that premake, generate a makefile 
3.3 using the newly made premake in the bin/release, premake embed to embed lua strings into the premake binary
3.4 premake embed
3.5 make config=release

## Shuriken beat slicer
1. git clone <aubio>
1.1 cd <aubio> ; make
1.2 For some reason aubio will build into the build directory and install into the build/dist/usr directory
1.3 so rsync or cp the usr directories and place the files in them at the appropriate places
1.4 change the variables in the aubio.pc pkgconfig file to point to the correct directory and place the aubio.pc into where you usually put all the pkgconfig files

2. git clone <rubberband>
2.1 fulfill the library requirements 
2.2 ./configure and make
2.3 install openjdk for the jni.h header file
2.4 make jni if needed
2.3 install into the appropriate directory ( either system level or your personal path )

3. git clone <shuriken beat slicer>
3.1 cd into <shuriken>
3.2 in the Shuriken.pro file, add INCLUDEPATH+=<path of where you put the other libraries, if they are not on the system paths> 
3.3 Also, if it is needed, add LIBS+= -L<path to the lib directory>
3.4 ./build 
3.5 shuriken is in <shuriken>/release

Shuriken is a beat slicer/time stretching tool

## IEM plugin suite
iem is the institute of electronic music and acoustics in graz.   
the plugin suite has ambisonic tools and a multiband compressor
It has ambisonic tools and a multiband compressor

1. git clone https://git.iem.at/audioplugins/IEMPluginSuite
2. cd into the directory
3. The jucer header files need to contain the path to the vst2 sdk
4. find ./ -iname \*jucer | while read a ; do grep -i vst3sdk "$a" ; if [ $? -eq 1 ] ; then echo " ---> $a" ; fi ; done | grep '\-\-\-' | awk '{print$2}' | while read a ; do sed -ie 's@headerPath="../../../resources/"@headerPath="../../../resources/;<path to the vst3sdk directory>"@' "$a" ; done
5. ./linux_buildAll.sh

## Tidalcycles
tidalcycles is a livecoding haskell environment that depends on supercollider
For ubuntu:

1. apt install haskell-platform
2. apt install emacs haskell-mode
3. cabal install Cabal cabal-install
3.1 looks like hoogle integration isn't working, and the haskell site doesn't have a way to install from scratch.
4. Find the cabal installed in the local user's .cabal/bin and put it somewhere on the path
5. Download the tidal and superdirt github repository
5.1 superdirt's startup.scd is used to initialise the supercollider server
5.2 take the tidal.el and place it into a your selected configuration directory. mine is .local/tidal
5.3 create the .emacs file according to http://pages.tidalcycles.org/getting_started.html#emacs . a note to emacs users - emacs --terminal `tty` <file> will give the traditional console experience otherwise the gui is used
6. Compile sc3-plugins. At the moment scide will be looking for the .so files but not in the .local/lib/SuperCollider/plugins where the sc3-plugins script installs them. Softlink them to the .local/share/SuperCollider/Extensions directory instead
7. load up the superdirt startup.scd file and evaluate

## OBX-d
obx-d is a software emulation of an oberheim synthesizer
https://github.com/reales/OB-Xd

1. Using Projucer, add the path to a patched version of vst3sdk that has vst2sdk files patched into it 

## Olive
Olive is a video editor 
### dependencies
1. apt-get install build-essential
2.  apt-get install qt5-default qtmultimedia5-dev libqt5multimedia5 libqt5multimedia5-plugins libqt5svg5-dev
3. apt-get install libavformat-dev libavcodec-dev libavfilter-dev libavutil-dev libswscale-dev libswresample-dev
4.  apt-get install frei0r-plugins-dev frei0r-plugins
5. apt-get install graphviz 
6. apt install doxygen libopencolorio-dev opencolorio-tools
7. apt install qttools5-dev-tools  qttools5-dev

### installation
1. mkdir build
2. cd build
3. cmake -DCMAKE_INSTALL_PREFIX=<path> ../
4. make -j 3 

