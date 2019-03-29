# CICM Hoa ambisonics
## Compilation notes for compiling the cicm higher order ambisonic library

The HOA library is an open source toolkit for generating higher order ambisonic and binaural output for pure data

The hoa library requires cream, pure data, and cicmwrapper

### HOA Library PD
1. git clone <hoalibrary-pd> 
2. git submodule update --init --recursive
3. configure it with the location of the pure data source
4. In ThirdParty/CicmWrapper/Sources/ebox.c
4.1  comment out the line that tries to toupper and reassign the uppercase name to itself. It's a const char* so hopefully the library doesn't depend on the string

### pure data 
1. In the pure ddata source, v0.49.1, export a getter method to provide the sys_staticpath and the sys_searchpath.
2. in main.c:   
```
 t_namelist* get_sys_staticpath(){return STUFF->st_staticpath;}   
 t_namelist* get_sys_searchpath(){return STUFF->st_searchpath;}
```
3. Extern the functions in the header

### CREAM library and CICMWrapper
1. CICM libraries depend on the cicmwrapper to provide the interface to the internals
2. You can git submodule update --init --recursive to update the cream library thirdparty sources, but in this case, the cicmwrapper needs to be replaced with the modified version
3. In the cicmwrpaper, replace variables that expect sys_staticpath, sys_searchpath to use the newer pure data functions
4. write a new function binbuf_get_attribute_int to eventually do an atom_getint instead. Check with pure data developers on: Are A_FLOATS the only valid representation of numbers in pd?

### Alter help files with library loads
1. In help files to test if it works
1.1 add [ declare -stdlib <path to lib> ] to load up the cream and hoa library 
1.2 Restart the help file to see if the library works
