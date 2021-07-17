# learn-C (Summer 1 — 2021)

## Basic C compilation sequence

- User program [source.c] 

==> C compiler (gcc -S)  [source.s] 

==> Assembler (gcc -c or as) [a.out] 

==> Hardware [machine code, .o file]
- See slide 1 (pg. 25, 26/69)
- gcc -Wall -o executable_file file_name.c file_name_2.c

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
- header guards in .h file
    - #ifndef foo.h
      #define "foo.h"
      #endif
## Basic syntax
- if/else
    -  
- loop
- func

