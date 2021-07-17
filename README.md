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
    - `#include <ctype.h>` // isalpha(ch); islower(ch); isupper(ch); isspace(ch); isdigit(ch); toupper(ch); tolower(ch); // slide 2 - pg.58
- __#include "local_header files"__ 
    - `#include "PA1.h"` // user created
- __#define CONST_NAME [value]__
    - `#define PI 3.14`
- __header guards__ in .h files
    ` #ifndef PA1_H       /* Macro Guard */`
   
   `#define PA1_H `
  
    `#ifndef DEFAULT_SIZE`
    
    `#define DEFAULT_SIZE 2021`
    
     `#endif`
- note: extern.h contains all extern vars

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
- __Endianness__ (slide 2 - pg.5, 11)
    - Little endian (X86-64, 32-bit Raspberry Pi): MSB at high address, LSB starts at lowest address
    - Big endian: MSB starts at lowest address
    - Ex: 4-byte data *d4c3b2a1* (MSB = d4 - starts for big-endian, LSB = a1 - start for little endian)
     
- __Array__
    - `#define CONST_COUNT 5` then `type arr[CONST_COUNT];` (slide 2, p.29)
        - Allocates CONST_COUNT * sizeof(type) bytes of contiguous memory
        - arr[0] at low memory, a[CONST_COUNT - 1] at higher memory
        - arr is const type*; ex: when passing arr name into a function, func param can be `int *arr` (slide 2, pg.50)
        - arr <=> &arr[0], (arr+1) <=> &arr[1]
        - *(arr+i) <=> arr[i]
        - Should define a compile-time const for #of elems in arr     
    - Initialization: `type arr[CONST_COUNT] = {val_1, val_2,...,val_CONST_COUNT};` or `int arr[5] = {};` // works with const size arrays, an arr of 5 ints; or `int vals[] = {1,2,3};` // no need to specify size
        - Only be used at definition time
    - `printf("%d", sizeof(arr));` // size of arr = (num elems) * (4bytes each)
    - `size_t count;` // __size_t__ is [typedef unsigned int]
	   `count = sizeof(arr) / sizeof(arr[0]);` // counts number of elems
    - Array name (like const, cannot be changed) = addreess of the start of array
    - Each sizeof(pointer of an int) takes 4 bytes ==> `int *xptr = ptr + 2;` gives xptr an address of ptr pointing to + (2 * 4) = 8 bytes (see slide 2, p.33)
    - Note `*xptr == *(ptr+2) == arr[2]`
    - Note: if ptr is int*, then dereferenced ptr can take in the values of a char (1 byte)
        - `char y = 'a';`   `*ptr = y;`
    - Can also do (ptr - some_num)  
    - __Warning__: cannot add 2 pointers, but can subtract 2 pointers of same type
        - `(ptr-xptr)` gives the number of elems between them 
        - In C, (ptr-xptr) acts like `(ptr-xptr) / sizeof(*ptr)` gives the number of elems bw them
    - 2D arrays: 
        - `type arr[rows][cols] = {{val_00, val_01, val_02,...},...,{val_n0, val_n1,...}}`
            - contiguous block of memory (slide 2, pg.46) 
        - `int *pgrid[2] = { (int[]){1, 2, 3, 4, 5}, (int[]){2, 3, 4, 5, 6}};`
            - sizeof(pgrid) = 2 * 4 = 8
            - memory does not need to be contiguous (slide 2, pg.47)
    - Array DN know their own size; No bounds checking, outbound ==> maybe runtime error
    - When being returned from a function, (returned type = int*) ==> an array should be *static* (slide 2, pg.51)
    - `void passElem(int a[])` then `sizeof(a)` is 4 bytes (size of a pointer; not like ex. abt array above).
    - To copy array elements, use loop (b/c name_arr_1 = name_arr_2; // only copy the pointer

- __Characters__:
	- 1 byte each
	- `'0'` (ASCII encoding for 0); `'\0'` (null), `'\''` (single quote), `'\\'` (backflash) 
	- Ex: print from a to z (lowercase)
	`char ch;
	
	for (ch = 'a'; ch <= 'z'; ch++)
	
	printf("%c", ch);`
	- C represents each char as an int (ASCII value)
		- 'A' is 65; 'a' = 97 = 65 + 32;  
- __C String__ (slide 2, pg.50)
    - `char arr[] = "Hello World";` // each char is 1 byte; 'H' is at lower address than 'd'
    	- Should have enough space to hold '\0'
    		- if not ==> invalid string ==> cannot use functions from string library
    		- ExL `char mess[3] = 'dog'`; // is not a string (not have '\0' at the end)
    - `char *messg = "Hello World";` // LHS read only
    	- "Hello World" is string literal/const ==> cannot be changed ==> cannot do *messg = 'h';
    	-   
    - Funcs on Cstring
        - `strlen(myStr)`: O(N) - scan entire string ==> should save its value somewhere
        	- Decoration: `size_t strlen(const char *s);`
        	- Ex: strlen(arr); // 5 (not count '\0') (char arr[] = "hello";)
        - `strcpy(charPtr1, charPtr2)`
        	-  Decoration: `char *strcpy(char *s0, const char *s1);`
        	-  Ex: `strcpy(str, "abc");` // now str = abc, but len(str) >= 3 to copy abc
	-  
        - 

  
- __Pointers__
    - (Has their own address)
    - Hold others' address 
    - Are allocated the same amount of memory regardless of where they point at
        - Ex: `int *x = &y;` // for y is an int; x stores the address of y (MSB is at high memory location, see slide 1, pg. 59) 
        - Note: `int *ptr1, ptr2;` // means int *ptr1; int ptr2;
    - Dereference: int y = *x; // for x = pointer to an int
        - Dereferencing a NULL(0) pointer / non-addresss ==> segmentation fault (runtime)
    - __Aliasing__: >1 var can access the same memory address
    - __NULL__ = pointer to nothing
        - Ex:`int* ptr = NULL;`   `int* ptr = (int *)0;` // cast 0 to a pointer type `int *ptr = (void *)0`; // auto to correct type
        - Ex: `if(ptr != NULL)` // NULL is like false (Boolean)    
    - __Pointer arithmetic__: see Array part above
        - `const int *a` // cannot change to a++
    - __Precedence__: `*ptr++ = 0;` since post-incr has higher precedence than dereference ==> acts from left to right ==> `*ptr = 0;` then `ptr = ptr + 1;`
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
- __self-define funcs__:
    - should not return &address_of_something if that thing will disappear after func call (ex: local vars, passed params) ==> can use "static" to make things not disappear (see slide 2, pg.27)

## Numbers in computer system:
- Dec, Bin, Hexa, Oct
- 2's complement format
    - Decimal to 2's complement format
    - 2's complement format to dec
    - Addition / Subtraction
- IEEE 754 format:
    - Dec to IEEE 754
    - IEEE 754 to Dec 
