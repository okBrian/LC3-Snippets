# Basic Java to LC3 ASM

This page is for when we learn C to ASM and ASM to C. But we haven't learned C yet.

The syntax between C and Java is somewhat similar so this is Java to LC3 ASM

### Variables in LC3 ASM

#### Java

```java
int foo;
int bar = 15;
int[] foobar = new int[10];
int barAlias = bar;
```

#### ASM&#x20;

```nasm
; int foo;
foo .BLKW 1     ; uninitialized
; or
foo .FILL 0     ; initialized to 0

; int bar = 15;
bar .FILL #15

; int[] foobar = new int[10];
foobar .BLKW 10 ; uninitialized
; or
foobar          ; initialized all to 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    .FILL 0
    
; int barAlias = bar;
barAlias .FILL bar
```

### Variable Assignment

#### Java

```java
int foo = 5;
int bar = foo + 1;
```

#### ASM &#x20;

```nasm
.orig x3000

    LD R0, foo      ; Load foo into temp register R0
    ADD R0, R0, #1  ; Add temp register by 1
    ST R0, bar      ; Store temp register R0 to bar
    HALT            
    
foo .FILL #5        ; Initialized with 5
bar .BLKW 1         ; Uinitialized

.end 
```

### Conditionals using Variables

#### Java

```java
int a = 5;
int b = 10;

if (a < b) {
    a++;
}
```

#### ASM

```nasm
.orig x3000

    LD R0, a        ; Load a into temp register R0
    LD R1, b        ; Load b into temp register R1
    
    NOT R1, R1      
    ADD R1, R1, #1  ; Find compliment of b and store in register R1
    ADD R0, R0, R1  ; Effectively doing if ((a - b) < 0)
    BRzp ENDIF
    
    ADD R0, R0, #1  ; Update the temp register R0
    ST R0, a        ; Store updated variable into a
        
ENDIF NOP           ; endif
    
    HALT
    
a .FILL 5           ; a = 5;
b .FILL 10          ; b = 10;

.end
```
