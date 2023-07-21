# GAME ENGINE

## Static vs Dynamic Libraries

1. Can have source code and header files included in the project - for example with single header libs one file gives you the source and the definitions so we don't need a binary.
2. Can only have header files that define the implementation - can build but not run the code until we actually link to the implementation.
    Which is not a source code file but a precompiled binary of the library that is compiled for our specific platform. Or a compiled object file that we want to link to.
    In this case we have header file + library binaries that we need to link to (by giving path of library to linker)
3. Advantage of dynamic libraries - don't have to compile the whole code again - instead linker can extract the implementation and insert it into our code during the linking stage.
4. Libraries can either be static - generally .lib/.a  files
    or dynamic - dll files

### Static Libraries
Static because they are only used by one program
Eg: Have a string library that we link to statically, by two programs. Each program uses its own version of the library

Program A -> string lib ( copy 1)
Program B -> string lib (copy 2)

### Dynamic Libraries
DLL files take the idea of libraries one step further. It seems wasteful to have multiple copies of the library, each one taking up space in each of the programs. DLLs are dynamically linked, meaning that we can share one copy of the function with several programs. Multiple programs running at the same time (that use the same function) can share one copy of that function, saving memory. These dynamically linked libraries usually have the extension .dll on Windows or .so on UNIX (shared object).


## Game Loop

