# C++ Notes

Resource:
https://www.learncpp.com/

- only include those files in headers that are needed by it. Do not include things needed by the .cpp implementation by the headers


Classes
 class ClassName {
    int ssn; // private by default can also give a private access specifier
    private:
        int secret;
    public:
        string name;

 }

 - Recommend using the struct keyword for data-only structures, and the class keyword for defining objects that require both data and functions to be bundled together.

- class members are private by default

- Some programmers prefer to list private members first, because the public members typically use the private ones, some people list public first to show interface

Fraction frac {}; // Value initialization using empty set of braces - default constructor gets called but when using value-initialization, the compiler may zero-initialize the class members before calling the default constructor in certain cases

We can also initialize class objects using default-initialization:
Fraction frac; // Default-initialization, calls default constructor

- defining a function with default parameters, all default parameters must follow any non-default parameters, i.e. there cannot be non-defaulted parameters after a defaulted parameter.


// Function with defaults params
void fn(int a = 0, double b = .34, string c = "abc")

- class has no constructors, C++ will automatically generate a public default constructor for you. This is sometimes called an implicit constructor (or implicitly generated constructor).

// Tell the compiler to create a default constructor, even if
    // there are other user-provided constructors.
    ClassName() = default; 


- A class may contain other class objects as member variables. By default, when the outer class is constructed, the member variables will have their default constructors called. This happens before the body of the constructor executes.

Eg:

### Game.h
 - Defining the interface

class Game {
    public:
        Game() // Constructor
        ~Game() // This is a destructor
        void Initialize();
        void Run()
}

### Game.cpp
#include "Game.h"

// Use namespace resolution to define the implementation of the constructor
Game::Game() {
    definition here
}

- fancy C++ people like to use .hpp as the extension for their header files
- static_cast - cast with compile type checking
- constexpr is also a way to declare a constant but it's a compile time constant whereas const may be compile or run time const


## Creating object on the stack vs heap

void fn() {
    // Created on the stack
    // constructor called
    Game game;

    game.Run()
    // Automatically popped from the stack at the end of this functions scope
    // Like all stack objects the lifetime of this object is tied to the scope of this function
    // destructor called when going out of scope
}

void fn() {
    // Created on the heap using the new keyword
    // new = allocation + initialization (calling the constructor of the object)
    // new = malloc + call to constructor
    Game game = new Game()

    // Have to explicitly free memory by calling delete
    // delete also runs the destructor and frees the memory
    // delete = free + call to destructor
    delete game

    // if scope ends the object is still alive but no reference to it is so memory leak??
}

## Operator Overloading


## Generics and Templates
c++ template functions: - implement in header file

Templates: just a placeholder for an actual function or class that the compiler will generate for us
depending on the types we use.

if we have a template class it is just a placeholder for actual classes that our compiler will generate.


## IN CPP creating an object with new creates it on the heap and returns a reference to it

## SMART POINTERS
Different kind of smart pointers

unique_ptr
shared_ptr
weak_ptr
...

### UNIQUE PTR
If we know we will only have one reference(pointer) to an object 
basically if we don't change ownership
#include <memory>

Eg:
// old way
void myFunc(void) {
    Enemy* e = new Enemy();


    // oops forgot to free. Memory leak
}

// with a unique smart pointer
#include <memory>

void myFunc(void) {
    std::unique_ptr<Enemy> e = std::make_unique<Enemy>()
    enemy2 = e // Not allowed
    e->Jump;

// This pointer goes out of scope when this function goes of of scope and it destroyed automatically
// Really it gets destroyed when the "owner" of this pointer goes out of scope
}


### SHARED_PTR

void myFunc(void) {
    std::shared_ptr<Enemy> e = std::make_shared<Enemy>();
    e2 = e // Allowed

    e->Jump
}
 Can share ownership
 Multiple shared_ptr references can reference the same resource
 As soon as the last smart pointer owning the resource goes out of scope, the object is destroyed
 uses reference counts