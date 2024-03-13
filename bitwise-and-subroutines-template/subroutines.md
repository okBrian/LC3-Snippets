# Subroutines

## Stack Build Up Template

```nasm
; Stack Build UP
ADD R6, R6, #-4 
; R6 + 3 = Return Value intentionally left empty
STR R7, R6, #2 
STR R5, R6, #1 
; Set Frame Pointer to Stack Pointer
ADD R5, R6, #0 

; R5 @ 0 is a local variable

; Save Old Resgister Values 
ADD R6, R6, #-5 
STR R0, R5, #-1 ; save SR1
STR R1, R5, #-2 ; save SR2
STR R2, R5, #-3 ; save SR3
STR R3, R5, #-4 ; save SR4
STR R4, R5, #-5 ; save SR5
```

## Stack Tear Down Template

```nasm
; Stack TearDOWN
; Grab the old register values
LDR R4, R5, #-5 
LDR R3, R5, #-4 
LDR R2, R5, #-3 
LDR R1, R5, #-2
LDR R0, R5, #-1
; Pop off all registers and all but one local variable
ADD R6, R5, #0 

; Load the return address 
LDR R7, R5, #2 

; Reload Old Frame Pointer
LDR R5, R5, #1 ; FP = Old FP
; Pop off the Old Saved Frame pointer and return address
ADD R6, R6, #3 ; pop 3 words
```

## Stack Buildup from Caller

```nasm
; For 1 Argument
ADD R6, R6, #-1
STR R0, R6, #0 ; Arg 1
```

```nasm
; For 2 Arguments
ADD R6, R6, #-2;
STR R0, R6, #0 ; Arg 1
STR R1, R6, #1 ; Arg 2
```

```nasm
; For 3 Arguments
ADD R6, R6, #-3
STR R0, R6, #0 ; Arg 1
STR R1, R6, #1 ; Arg 2
STR R2, R6, #2 ; Arg 3
```

## Stack Teardown from Caller

```nasm
JSR ...
LDR R0, R6, #0
ADD R6, R6, #2 ; 2 for 1 arg, 3 for 2 arg, etc.
```
