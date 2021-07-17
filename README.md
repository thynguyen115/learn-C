# Learn-C

## Basic C compilation sequence

- User program [source.c] 

==> C compiler (gcc -S)  [source.s] 

==> Assembler (gcc -c or as) [a.out] 

==> Hardware [machine code, .o file]
- See slide 1 (pg. 25, 26/69)
- `gcc -Wall -o executable_file file_name.c file_name_2.c`

## Pre-processor and directives
- __#include <system_header files>__ (txtbook, pg. 684)
    - `#include <stdio.h>` // Standard input/output library; used for (printf, fprintf and scanf, fscanf) or (fopen and fclose).
    - `#include <stdlib.h>` // Standard library; used for (rand and srand), (malloc and free), (exit), (atoi, atol, and atof).
    - `#include <math.h>` // Math library; for (sin, cos, asin, acos, sqrt, log, log10, exp, floor, and ceil).
    - `#include <string.h>` // String library
- __#include "local_header files"__ 
    - `#include "PA1.h"` // user created
- __#define CONST_NAME [value]__
    - `#define PI 3.14`
- header guards in .h files
    ` #ifndef PA1_H       /* Macro Guard */`
    `#define PA1_H `
  
    `#ifndef DEFAULT_SIZE`
    `#define DEFAULT_SIZE 2021`
     `#endif`
- note: extern.h contains 

## Data types:
- __extern__ (outside func, might come from another file), __global__ (outside func, inside current file, also static var, avalable to other files), __static__ (could be inside/outside func, but only available to __this__ file) - see slide 1 pg. 45), __local__ (inside func)
    - Ex:  `extern int x;     int x_global;   static int x_static;    int local_var;`
    - Ex: `extern int func_name(void);`
    - __Import list__ (from outside file to this file): those with "extern" keyword 
    - __Export list__ (from this file to another file): those global vars/funcs
- Size [32-bit ARM (PI)] (slide 1, pg. 51): char (1), short (2), int = long = float = pointer * (4), double = long long = long double = 8
    - `sizeof()` // returns number of bytes to store a var / var type
    - Ex: `size_t shrt_byte = sizeof(shrt_byte);` // size_t = (often) unsigned long
    - Ex:  `printf("size=%zu", sizeof(x_ptr));` 
- Edianness
    - Little endian
    - Big endian
- Array
    - 1D
    - 2D
- C String
    - Funcs on Cstring
        - strlen
        - strcpy
        -     
- Pointers
    - (Has their own address)
    - Hold others' address 
    - Are allocated the same amount of memory regardless of where they point at
        - Ex: `int *x = &y;` // for y is an int; x stores the address of y (MSB is at high memory location, see slide 1, pg. 59) 
        - Note: `int *ptr1, ptr2;` // means int *ptr1; int ptr2;
    - Dereference: int y = *x; // for x = pointer to an int
        - Dereferencing a NULL(0) pointer / non-addresss ==> segmentation fault (runtime)
    - Aliasing: > 1 var can access the same memory address
    - Pointer arithmetic
- Struct
- Dynamic mem allocation
    - Similarities
    - Differences
    
## Basic syntax
- if/else
    -  
- loop
- func

## Fundamental functions
- printf
- putchar

## Numbers in computer system:
- Dec, Bin, Hexa, Oct
- 2's complement format
    - Decimal to 2's complement format
    - 2's complement format to dec
    - Addition / Subtraction
- IEEE 754 format:
    - Dec to IEEE 754
    - IEEE 754 to Dec 
