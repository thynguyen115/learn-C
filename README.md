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
        - arr is const type*; 
        	- ex: when passing arr name into a function, func param can be `int *arr` (slide 2, pg.50)
        	- cannot change the array name ==> cannot do arr = arr + 1;
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
    - Funcs on Cstring
        - `strlen(myStr)`: O(N) - scan entire string ==> should save its value somewhere
        	- Decoration: `size_t strlen(const char *s);`
        	- Ex: strlen(arr); // 5 (not count '\0') (char arr[] = "hello";)
        	- Similar to (endPtr_to_null - startPtr - 1) // (slide 2, pg. 64)
        - `strcpy(charPtr1, charPtr2)`
        	-  Decoration: `char *strcpy(char *s0, const char *s1);`
        	-  Ex: `strcpy(str, "abc");` // now str = abc, but len(str) >= 3 to copy abc
        	-  If not enough space ==> overwrites other memory (next to it) ==> other memory is affected (i.e. changes its value) ==> __buffer overflow__ (slide 2, pg.68)
	-  `char *strncpy(char *dest, const char *src, size_t n);`
        	- Copy n chars from source.
        	- Ex: `strncpy(dest, src, 10);` // copy 10 chars
        	- If copy is terminated by length (n) being reach (but there is still other characters behind these) ==> no null char '\0' added to the end ==> can't tell where the end of string is (slide 2, pg. 69)
        -   `char *strcat(char *dest, const char *src);`
        	- Concatenate string pointed by src to the end of string pointed by dest
        	- Ex: `strcpy(dest, src);` // with char dest[10] = "world; char src[10] = "hello";
        		- gives: worldhello
	-    `char *strncat(char *dest, const char *src, size_t n);`
	-    `int strcmp(const char *s0, const char *s1);` // slide 2, pg. 70
		-  Compare 2 strings (by their ASCII values)
		-  Returns 0 (s0 == s1); >0 (s0 > s1); <0 (s0 < s1)
  
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
- __Struct__ // slide 3, pg. 12-15
	- Define a var type (not declare a var or allocate space)
	- `struct structName { // fields }; var1, var2;`
	- `struct structName variable_name;` // define a struct
	- Pointers to a struct: `struct rectangle *ptr = &r1;`
		- Access fields: `ptr->field_name = val;` or `(*ptr).field_name=val;`
		- Passing a pointer is cheaper (less space); If small struct ==> pass a struct copy to a function
	-  Assignment: `struct1 = struct2;` // copies all members
	-  `typedef struct struct_name {// fields; struct struct_name * next; } struct_name;`
		- Define a struct called "struct_name" ==> next time, only need to use `struct_name var1;` // no need to write `struct struct_name var1;`
		- slide 3, pg. 19   
- __Dynamic mem allocation__
    - Similarities
    - Differences
    - Dynamically allocated struct (slide 3, pg. 40)

## Basic syntax
- `int main(int argc, char * argv[]);` // slide 3, pg. 4
	- argc = number of elements in the array argv
	- argv = an array of (pointers to char) 
	- argv[0] (ex: ./pa1) // (slide 3, pg. 6)
	- argv[argc] = NULL
	- int getopt(int argc, char * const argv[], const char *optstring); 
- if/else
- loop
- func

## Fundamental functions
- printf
- putchar

## Memory
- Funcs:
	- `void * malloc(size_t size)` // slide 3, pg. 31
		- Allocates a continuous block of size bytes of uninitialized memory (heap)
		- Returns pointer to beginning of the block
		- Returns NULL if errno happens (allocation fails)
		- `p = (int *)malloc(n*sizeof(int))) == NULL`
		- `char *bytes = (char *)malloc(BLKSZ * sizeof(char));`
	- `void free(void *p)` // slide 3, pg. 32
		- p = address returned by malloc/calloc/realloc
		- free the block p pointed to 
		- do not free something that has already been erased
	- `void *calloc(size_t num_of_elems, size_t size_of_each_elem)` // slide 3, pg. 33
		- Similar to malloc, but initialize everything to 0 before returning the pointer
		- More expensive than malloc b/c of zero-ing
	- `void *realloc(void *p, size_t newBytes);` // slide 3, pg. 34
		- Reallocate the size of the heap memory where p pointed to ==> new size = newBytes
		- Contents (heap memory) unchanged (up to Min(old_size, new_size))
		- Return NULL (if having unchanged allocated buffer)
- Free: // slide 3, pg. p37
	- Should free once (even tho many pointers pointing to it)
	- Free the beginning of the address
	- If not free, memory leaks, no more memory ==> crash 	
- __self-define funcs__:
    - should not return &address_of_something if that thing will disappear after func call (ex: local vars, passed params) ==> can use "static" to make things not disappear (see slide 2, pg.27)

## 32-bit ARM:
- User Mode Memory Space (3MB)
	- slide 3, pg 47-50  

## Precedence:
- Slide 3, pg. 9, 10
- () > postfix 		> 		prefix > dereference

## C File IO
- stdout = buffer, if buffer is not full ==> not display
- stderr = not buffer, display immediately
- Some terms: EOF, NULL, BUFSIZ (slide 3, pg. 22)
- `#include <stdio.h>`, `#include <errno.h>`
-  Funcs: fopen, flose, fprintf, fscanf, fread, fwrite, perror, clearerr // slide 3, pg. 23-25

## Numbers in computer system:
- __Decimal: base 10__
	- 10 symbols [0,9] 
	-  Ex: 71 (base 10) = (7 * 10^1) + (1 * 10^0)
	-  From decimal to binary: takes the decimal number, / 2 until the result = 0, then read the remainders starting from the most recent remainder up to the first one.
		- Unsigned int ==> Unside Bin  
		- Ex: 71/2 = 35 (r = 1); 35/2=17 (r=1); 17/2=8 (r=1); 8/2=4 (r=0); 4/2=2 (r=0); 2/2=1 (r=0); 1/2=0 (r=1) ==> 71 (base 10) = 0100 0111 (base 2)  // slide 3, pg.5
		- Check: 0100 0111 = 2^6 + 2^2 + 2^1+ 1 = 71

- __Binary: base 2__
	-  2 symbols: 0, 1
	-  Prefix: "0b"
	-  Ex: 10 (base 2) = 0b10 = (1 * 2 ^ 1)  + (0 * 2^0) = 2 (base 10)
	-				MSB  	      LSB
	- From bin to hexa: 
		- 0000 = 0, 0001 = 1, 0010 = 2, 0011 = 3, 0100 = 4
		- 0101 = 5, 0110 = 6, 0111 = 7, 1000 = 8, 10001 = 9,
		- 1010 = A (10), 1011 = B (11), 1100 = C (12), 
		- 1101 = D (13), 1110 = E (14), 1111 = F (15)
		- Ex: 0110 1010 0011 1111 = 0x6A3F  
- __Hexadecimal: base 16__
	- 16 symbols: [0,9] and [A,F] w/ A=10, B=11, C=12, D=13, E=14, F=15	 
	- Prefix: "0x"
	- Ex: 10 (base 16) = 0x10 = 16 (base 10)
	- From int to hexademical: takes the decimal number, /16 until the result = 0, then read the remainders starting from the most recent remainder up to the first one.
		- Ex: 249 (base10) => 249/16=15 (r=9), 15/16=0 (r=15=F) ==> 0xF9 (base 16) 
		- Ez: 19FE (base 16) = 1 * (16^3) + 9 * (16^2) + F * (16^1) + E * (16^0) = 6654 (base 10)

- __Octal: base 8__ (slide 4, pg. 17)
	- Symbols: 0-7
	-  Prefix: 0 (--> bin=0) or 1 (--> bin = 1)
	-  From Bin to Oct (groups of 3 bits per digit from right to left)
		- 0 110 101 000 111 111 = 0 65077
		- 01 75123 = 1  
	- Bin to Oct: 000 = 0, 001 = 1, 010 = 2, 011 = 3, 100 = 4, 101 = 5, 110 = 6, 111 = 7 (slide 3, pg. 16)
- __2's complement format__:
    - Decimal to 2's complement format:
    	- Negative decimal --> binary: ignore the sign, num/2 until 0 (just like above), __flip bits, +1__
    	-  -102 (base 10) ==> 102/2 = 51 (r=0) .... until 1/2=0 r = 1 ==> slide 4, pg.34
    - 2's complement format to dec
    	- If negative binary: Flip bits, +1, take magnitude, adjust the sign (slide 4, pg.36) 
    	- If positive binary: work as normal.
- __Addition__ of 2 binary numbers:
	- __1 + 1 = 0 (carries 1)__; 1 + 0 = 0 + 1 = 1; 0 + 0 = 0 // slide 4, pg. 20
- __Subtraction__ of 2 binary numbers:
	-  __0 - 1 = 1__ (borrow 10, and affect the higher bits) // slide 4, pg. 21
	-  x - y = x + (-y) // use 2's complement
-  __Singed__:
	- 0 (pos), 1 (neg) // side 4, pg. 25
- __Overflow__:
	- Unsigned numbers: when Cout != 0 (after addition, still carry a 1) // slide 4, pg. 39
	- 2's complement overflow: : result is correct ONLY when the carry into the sign bit 
position (MSB) equals the carry out of the sign bit position (MSB) // slide 4, pg. 40
	For signed: overflow occurs if operands have same sign and result’s sign is different // slide 4, pg. 41
- __Expand bit__:
	- Unsigned: add leading 0s
	- Signed: repeat the sign of the value // slide 4, pg. 44
- __Truncate bit__:
	-   From 8 bits --> 4 bits ==> remove 4 leading digits, the result could be overflowed and not the same. (slide 4, pg. 44, 47-48)
- IEEE 754 format:
    - Dec to IEEE 754
    - IEEE 754 to Dec 
