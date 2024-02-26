# Subroutines

## Stack Build Up Template

```nasm
; Stack Build UP
; Pushes 4 words (16 bit * 4) onto the Stack Pointer
ADD R6, R6, -4 
; Fill only 3 Words and Return value is the last spot 
; R6 + 3 = Return Value intentionally left empty
; Save Return Address
STR R7, R6, 2 
; Save Old Frame Pointer
STR R5, R6, 1 ;
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
```

## Stack Tear Down Template

<pre class="language-nasm"><code class="lang-nasm">; Stack TearDOWN
; Grab the old register values
LDR R4, R5, -5 ; restore R4
LDR R3, R5, -4 ; restore R3
LDR R2, R5, -3 ; restore R2
LDR R1, R5, -2 ; restore R1
LDR R0, R5, -1 ; restore R0

<strong>; Pop the Local variables and the saved register values by setting the stack pointer to the frame pointer
</strong>ADD R6, R5, 0 

; Load the return address 
LDR R7, R5, 2 

; Reload Old Frame Pointer
LDR R5, R5, 1 ; FP = Old FP
; Pop off the Old Saved Frame pointer and return address
ADD R6, R6, 3 ; pop 3 words
</code></pre>

