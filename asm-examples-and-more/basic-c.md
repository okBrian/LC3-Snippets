# Basic C

This is supplemental material fro when we learn C.&#x20;

Some of these features aren't in Java, which are hidden under the garbage collector.&#x20;

So here is C to ASM, I made all the C programs runnable too. Just use a C online compiler or the provided docker file from class

## Basic C Program Structure&#x20;

```c
#include <stdio.h> // C Preproccesor that just includes the standard IO library
                   // Similar to doing 
                   // import java.util.ArrayList
                   // Can do alot more but lecture should cover this in detail

int main() { // Entry point of program same as .orig x3000
    // Syntax is pretty similar to Java
    int a = 0; 
    for (int i = 0; i < 5; i++) {
        a++;
        printf("%d\n", a);
    }
    
    return 0; // similar to HALT ig, 0 means the program ran w/o issues 
    // Output:
    // 1
    // 2
    // 3
    // 4
    // 5
}
```

## Stack Vs. Heap

Both are areas where data can be stored in memory.

Stack and Heap  can get kind of complicated, but the basics you need to know isn't that bad

Stack

* Auto deletes at end of program like the stack of LC3
* Usually shouldn't store anything big in stack as its somewhat limited
* In C int a = 5; is in the stack and will auto delete at the end of the program
* Stored in a specific area of memory and is stored in a stack like data structure so it is easily deleted after the program finishes.

Heap&#x20;

* Memory allocated needs to be freed at end of a program
* Can store bigger data like Objects and ArrayLists in Java, or Arrays in C.
* In Java this is handled by the garbage collector, so we didn't need to worry about it.&#x20;
* Stored anywhere open memory is found so if you lose the pointer, you can't free the memory causing a memory leak

If the data isn't freed it can cause a data leak where memory is allocated but never freed. As the program continues to run it will take more and more memory till their is no memory left, and the computer will probably blue screen. In 2110 we use Valgrind to find potential memory leaks

## Pointers & Heap Memory Allocation&#x20;

Pointers are just variables that point to another memory address. This is basically the LDI opcode.

```c
#include <stido.h> // For printf

int main() {
    int a = 5;  // A integer stored on the stack 
    int* b = &a; // Points to the memory address of a
    
    // In C inorder to access the value of a pointer we must dereference it
    printf("%d\n", *b);
    // *b the * before b dereferences the varaible giving us the value to which
    // b points to which is 5 
    
    // Output
    // 5
    
    return 0;
}
```

#### Malloc

"Memory Allocate" - Allocates a given amount of bytes and returns a void\*

void\* -> can be kind of confusing but basically C doesn't know how you are interpreting the memory allocated so you need to cast it to like a int\* for example.

In other words, In the memory's point of view all it sees is bits, and it is up to the C program to interpret the data we allocate.

Malloc takes in 1 parameter which is the amount of bytes to allocate in memory

&#x20;**Important Note: malloc doesn't initialize the memory allocated so they will store garbage values to begin with**&#x20;

```c
#include <stdio.h>  // For printf
#include <stdlib.h> // For malloc

int main() {
    // Integers are 4 bytes in most modern machines, so we are allocating 
    // 4 bytes of memory
    // The cast to (int*) at the front tells the program how we are interpreting the 
    // memory
    int* a = (int*)malloc(sizeof(int));
    // Dereference the varaible and give it a value
    *a = 5;
    
    printf("%d\n", *a);
    
    // Output
    // 5
    
    free(a); // free the allocated data
    return 0;
}
```

#### Calloc

"Contiguous Allocate" - Allocates a given amount of memory but initalizes it to the default value, 0.

The main difference is that it sets the memory allocated to the default value, and it takes 2 parameters

The first parameter is how many blocks of memory to allocate, and the second parameter is the size of each block.

```c
#include <stdio.h>  // For printf
#include <stdlib.h> // For calloc

int main() {
    // First Parameter is 1 block of memory
    // Second Paramter is the size of the block which is 4
    int* a = (int*)calloc(1, sizeof(int));
    // Dereference the varaible and give it a value
    *a = 5;
    
    printf("%d\n", *a);
    
    free(a); // free the allocated data
    
    // Output
    // 5
    
    return 0;
}
```

#### Realloc

"Re-allocate" - Re-allocates more space to a given amount of memory, keeps the current values stored and new memory allocated is uninitialized so it has the default garbage value.

If there is open memory behind realloc it will just stay in-place and expand, but if there is not it will be moved and copied.

Realloc takes 2 parameters the pointer to the memory that is being reallocated and the byte-size of the new memory being allocated.

```c
#include <stdio.h>  // For printf
#include <stdlib.h> // For calloc & realloc

int main() {
    // Original Memory Allocation
    int* a = (int*)calloc(1, sizeof(int));
    // Dereference the varaible and give it a value
    *a = 5;
    
    printf("%d\n", *a);
    
    // Reallocate for 2 * sizeof(int) ie. 8 bytes from 4 bytes
    // This is basically an array
    a = (int*)realloc(a, 2 * sizeof(int));
    // a[1] auto dereferences so you can set it to 10 w/o *
    a[1] = 10;
    
    // *a or a[0] are both valid here
    printf("%d\n", *a);
    printf("%d\n", a[1]);
    
    
    // Output
    // 5
    // 5
    // 10
    
    free(a); // free the allocated data
    return 0;
}
```

**Note** if you make a different pointer variable with realloc you should free either one but not both

```c
#include <stdio.h>  // For printf
#include <stdlib.h> // For calloc & realloc

int main() {
    int* a = (int*)calloc(1, sizeof(int));
    *a = 5;
  
    // New pointer here
    int* b = (int*)realloc(a, 2 * sizeof(int));
    b[1] = 10;
    
    printf("%d\n", *b);
    printf("%d\n", b[1]);
    
    
    
    // free(a);
    // or
    // free(b);
    // but not both it will cause an error because you are freeing the same memory 
    // twice, use valgrind to check
    
    free(a);
    return 0;
}
```

## Arrays

Arrays in C can be created in 2 ways.&#x20;

* Stack&#x20;
  * These should only be used when the size is known and will not change.
* Heap
  * These should be used when the size is unknown, the array will grow, or the array is very large.

#### Stack&#x20;

```c
#include <stdlib.h>

int main() {
    // No Size varaible to keep track of size
    const int SIZE = 5;
    int a[SIZE];
    for (int i = 0; i < SIZE; i++) {
        a[i] = i;
    }
    
    for (int i = 0; i < SIZE; i++) {
        printf("%d\n", a[i]);
    }
    
    // No need to free as its on the stack
    return 0;
}
```

#### Heap

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    const int SIZE = 5;
    int* arr = (int*)malloc(SIZE * sizeof(int));
    // Note the pointer arr is stored on the stack
    // The data malloc is stored on the heap
    // So if the pointer is deleted you lose access to the heap

    for (int i = 0; i < SIZE; i++) {
        arr[i] = i; 
        // arr[i] in a heap array is implicitly doing *(arr + i)
        // its taking the memory address of arr adding it by i and dereferencing it
    }
    
    for (int i = 0; i < SIZE; i++) {
    printf("%d", *(arr + i));
    // same as doing arr[i]
    }
    free(arr);
    return 0;
}
```

