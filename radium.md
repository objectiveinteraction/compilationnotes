# Radium
radium is the latest in music tracker tech combined with faust, pure data 

## compiling

1. radium uses QT.
2. use QT_SELECT to set the correct qt version. use qt_chooser to list the target qts that can be chosen
3. radium requires llvm 4-6, but these require -fno-rtti or with rtti compiled into every dependency. recompile everything with the correct flags
4. llvm 6++ has no restrictions on rtti, but the faust that comes with radium is an older version 
5. The latest QT has changed calling interfaces on some functions. Update faustqt to QTUI.h 
6. moc is used to generate the qt c files. The qt make file needs some changes with references to qtui.h in qtcompile or the auto generated files need to be changed one at at time
7. faust is used to transpile the msd files into c files for use in radium
8. the file of one of the faust instruments has a division by zero error. Change the 0 into a float of small size
9. For some reason one of the faust instrument has a circular dependency on one of the variable, so comment out the one line til it passes compilation and reinstate
10. Change all function calls to qt that use the older qt.



