# Learn-C

## Basic C compilation sequence

- User program [source.c] 

==> C compiler (gcc -S)  [source.s] 

==> Assembler (gcc -c or as) [a.out] 

==> Hardware [machine code, .o file]
- See slide 1 (pg. 25, 26/69)
- `gcc -Wall -o executable_file file_name.c file_name_2.c`

## Pre-processor and directives
- #include <system_header files> (txtbook, pg. 684)
    - #include <stdio.h> // Standard input/output library; used for (printf, fprintf and scanf, fscanf) or (fopen and fclose).
    - #include <stdlib.h> // Standard library; used for (rand and srand), (malloc and free), (exit), (atoi, atol, and atof).
    - #include <math.h> // Math library; for (sin, cos, asin, acos, sqrt, log, log10, exp, floor, and ceil).
    - #include <string.h> // String library
- #include "local_header files" 
    - #include "PA1.h" // user created
- #define CONST_NAME [value]
    - #define PI 3.14
- header guards in .h files
    ` #ifndef PA1_H       /* Macro Guard */
    
      #define PA1_H `
      
    `#ifndef DEFAULT_SIZE
    
     #define DEFAULT_SIZE 2021
     
     #endif`
- note: extern.h contains 

## Data types:
- extern (outside func, might come from another file), global (outside func, inside current file, also static var, avalable to other files), static (could be inside/outside func, but only available to __this__ file) - see slide 1 pg. 45), local (inside func)
    - Ex: extern int x;     int x_global;   static int x_static;    int local_var;
    - Ex: extern int func_name(void);
    - Import list (from outside file to this file): those with "extern" keyword 
    - Export list (from this file to another file): those global vars/funcs
 
## Basic syntax
- if/else
    -  
- loop
- func

## Fundamental functions
- printf
- putchar


