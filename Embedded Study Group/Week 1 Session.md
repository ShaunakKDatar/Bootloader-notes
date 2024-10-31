
# Working of C Compiler

- Preprocessor - Preprocess code for compilation, Handle `#define` and `#include` lines
- Compiling - Converts preprocessor code to a IR
- Assembly - Converts Intermediate representation to hardware specific assembly code
- Linking:
	- Static Linker - Copies all the code from the included files to the executable, makes executable larger but self contained
	- Dynamic Linker - Stores the reference to the functions in the include file, instead of copying the code.

# Build Systems

## GNU Make
## Ninja
To use ninja we will have to make a Cmake.txt file.
We use ninja for speed. Ninja has a relatively higher speed 
CMake is the build system generator. It is the backend of ninja
## CMake
It is a build system generator. So with little input it gives us a entire build system, including a ninja build.

# Memory allocation in C

## 1. Static Allocation
Space is allocated for the variables when the program starts and remains allocated for the entire duration
## 2. Automatic Allocation
Space is allocated when a block of code is entered and freed when the block is exited.
## Dynamic Allocation
Space is allocated during program execution based on need. You request memory using functions and manage it manually.

We use functions like - `malloc` - allocates memory, `calloc` - allocates memory and initializes all with `1`, and `free`- used to free the allocated memory.


# Registers in Microcontrollers

Everything in a microcontroller works on the basis of registers. Registers are basically memory elements which stores stuff using flip-flop. For example the register for the UART communication protocol is called `Serial Buffer`. 


# Memory allocation using segments

Text Segment: 
- Stores the Compiles part of the program and is read-only
- Contains global variables, constants and uninitialized variables.
- Executable code resides here, making up the core instructions of the program

Stack Segment:
- Has a last in first out malloc
- Stores local variables and function call information
- Managed automatically, but has limited size and can cause stack overflow if overused.

Heap Segment:
- Used for Dynamic memory allocation
- Memory is allocated and freed using functions like malloc() and free()
- Flexible with size but requires careful management to avoid memory leaks

# 



