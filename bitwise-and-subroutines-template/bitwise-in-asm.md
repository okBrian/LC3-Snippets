# Bitwise in ASM

Just Templates !

LC3 ASM has built in NOT, AND

For NAND, NOR, and XNOR just take the compliment of the original

### OR

```nasm
; OR R1, R2, R3
; R1 = R2 | R3
; R2 | R3 == ~(~R2 & ~R3)

    NOT R2, R2       ; R2 = ~R2
    NOT R3, R3       ; R3 = ~R3
    AND R1, R2, R3   ; R1 = ~R2 & ~R3
    NOT R1, R1       ; R1 = ~R1       ; Remove this for NOR
```

### XOR

```nasm
; XOR R1, R2, R3
; R1 = R2 ^ R3
; R2 ^ R3 = ~(R2 & R3) & ~(~R2 & ~R3)

    AND R1, R2, R3  ; R1 = R2 & R3
    NOT R1, R1      ; R1 = ~(R2 & R3) 
    NOT R2, R2      ; R2 = ~R2
    NOT R3, R3      ; R3 = ~R3
    AND R3, R2, R3  ; R3 = ~R2 & ~R3
    NOT R3, R3      ; R3 = ~(~R2 & ~R3)
    AND R1, R1, R3  ; R1 = ~(R2 & R3) & ~(~R2 & ~R3)
```

### <<

```nasm
; R1 = 10  
; R1 = R1 << 1 ; R1 = 20  
; R1 = R1 << 1 ; R1 = 40

    AND R1, R1, #0  ; Set R1 to 0
    ADD R1, R1, #10 ; Add R1 by 10
    ADD R1, R1, R1  ; Same as << 1 or multiplying by 2 
    ADD R1, R1, R1  ; Same as << 2 or multiplying by 4
```

### Logical Right Shift is horrid trust me
