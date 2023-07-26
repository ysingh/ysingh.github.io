# C++ Notes

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