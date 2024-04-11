# Recursion Example

Below is a example of the fib sequence using recursion written in LC3 ASM.

```nasm
;int fib(int n){
;    int answer;
;    if (n <= 1)
;      answer = n;
;    else
;      answer = fib(n - 1) + fib(n - 2);
;    return answer;
;}


.orig x3000

    ; Get N from memory
    LD R0, n

    ; Push N as an argument
    ADD R6, R6, #-1
    STR R0, R6, #0
    
    ; fib(int n)
    JSR fib

    ; Get and Pop the Return Value from the address
    LDR R0, R6, #0 ;R0 = return value
    ADD R6, R6, #1
    ; Save return value at label m
    ST R0, RES
    ; Pop the arguments off the stack
    ADD R6, R6, #1

    HALT

fib

    BR STACKBUILDUP
STACKBUILDUPEND NOP
    
    ; Load a local varaible at R5 + #0
    LDR R0, R5, #0

    ; Load N into R0
    LDR R1, R5, #4

    ; R2 = -1
    AND R2, R2, #0
    ADD R2, R2, #-1

    ADD R2, R1, R2 ; n - 1
    BRp else

        AND R0, R0, #0
        ADD R0, R1, R0
        STR R0, R5, #0
   
    BR endif

else NOP
    
        ADD R3, R1, #-1
        ADD R6, R6, #-1
        STR R3, R6, #0
        JSR fib 

        ; Get and Pop the Return Value from the address
        LDR R0, R6, #0 ;R0 = return value
        ADD R6, R6, #1
        ; Pop the arguments off the stack
        ADD R6, R6, #1

        ADD R3, R1, #-2
        ADD R6, R6, #-1
        STR R3, R6, #0
        JSR fib

        ; Get and Pop the Return Value from the address
        LDR R4, R6, #0 ;R4 = return value
        ADD R6, R6, #1

        ; Pop the arguments off the stack
        ADD R6, R6, #1

        ADD R0, R0, R4
        STR R0, R5, #0
        
endif NOP

    LDR R0, R5, #0 
    STR R0, R5, #3

    BR STACKTEARDOWN
STACKTEARDOWNEND NOP
    

RET


n .fill 1
RES .BLKW 1

STACKBUILDUP

; Stack Build UP
; Pushes 4 words (16 bit * 4) onto the Stack Pointer
ADD R6, R6, -4 
; Fill only 3 Words and Return value is the last spot 
; R6 + 3 = Return Value intentionally left empty
; Save Return Address
STR R7, R6, 2 
; Save Old Frame Pointer
STR R5, R6, 1 
; R6 + 0 is kept empty as its the local variable
; Set Frame Pointer to Stack Pointer
ADD R5, R6, 0 

; Save Old Resgister Values 
; Push 5 words onto stack
ADD R6, R6, #-5 
; Save All registers onto stack ; USES THE NEW FRAME POINTER SO USES NEG NUMBERS
STR R0, R5, #-1 ; save SR1
STR R1, R5, #-2 ; save SR2
STR R2, R5, #-3 ; save SR3
STR R3, R5, #-4 ; save SR4
STR R4, R5, #-5 ; save SR5

BR STACKBUILDUPEND

STACKTEARDOWN
; Stack TearDOWN
; Grab the old register values
LDR R4, R5, #-5 ; restore R4
LDR R3, R5, #-4 ; restore R3
LDR R2, R5, #-3 ; restore R2
LDR R1, R5, #-2 ; restore R1
LDR R0, R5, #-1 ; restore R0

; Pop the Local variables and the saved register values by setting the stack pointer to the frame pointer
ADD R6, R5, #0 

; Load the return address 
LDR R7, R5, #2 

; Reload Old Frame Pointer
LDR R5, R5, #1 ; FP = Old FP
; Pop off the Old Saved Frame pointer and return address
ADD R6, R6, #3 ; pop 3 words

BR STACKTEARDOWNEND

.end
```
