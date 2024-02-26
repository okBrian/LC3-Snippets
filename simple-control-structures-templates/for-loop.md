# For Loop

Note: These are templates not stand alone programs



### Basic For Loop Example

```
    AND R1, R1, #0   ; Set R1 to 0
FOR1 ADD R1, R1, #0  ; Set CC to R1
    BRnz ENDF1       ; R1 > 0
    
        ; ...
    
    ADD R1, R1, #1   ; R1 = R1 + 1
    BR FOR1          ; Loop Back Around
ENDF1 NOP            ; endfor
```
