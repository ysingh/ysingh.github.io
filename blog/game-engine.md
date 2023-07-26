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
Basic Loop

Process Input
Update Game simulation
Render

# OOP Design

### Game.cpp
Initialize()
Run() -> Has the game loop
Destroy() -> Do we really need to destroy, shouldn't the OS take care of this?

https://gafferongames.com/post/fix_your_timestep/
https://archive.org/details/GDC2015Fiedler
https://www.youtube.com/watch?v=fdAOPHgW7qM
https://www.youtube.com/watch?v=pctGOMDW-HQ
https://www.youtube.com/watch?v=jTzIDmjkLQo


- dont tie physics simulation to frame rate
- simulate physics and keep speed of render separate
- delta time seconds elapsed since last frame
- Can have either constant delta time eg: cap the frame rate at 60 fps or monitor refresh rate and set that as the simulation time step to a constant eg: 16 ms since thats the time every frame takes.
- variable delta time: game runs independent of fps but not deterministic.

- When we are running our physics at any timestep we are basically sampling the actual behavior.
For example if we run at 60 fps our physics will run every 16 ms. So we will have a set of values like this
[a, b, c, d, e]
where a will be the physics approximation on the first sample (frame 0)
b will be the approximation at 16ms (frame 1)
c will be the approximation at 32ms (frame 2)

Problem with this is if the frame rate is low our physics samples we be more spread out and more inexact.
Eg: if we have a bullet going in a straight line and a thin wall, if our physics samples are too far apart the bullet will go through the wall, since we won't see the position where the bullet collided with the wall. This is called "Tunneling"

