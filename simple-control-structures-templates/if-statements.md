# If Statements

Note: These are templates not stand alone programs



### If Statement

```
    ADD R1, R1, #0  ; Set CC to R1
    BRnz ENDIF1     ; if R1 > 0
        
        ; ...
    
ENDIF1 NOP          ; endif
```

### If Else Statement

```
    ADD R1, R1, #0 ; Set CC to R1
    BRn ELSE1     ; if R1 >= 0

      ; ...

    BR ENDIF1

ELSE1 NOP          ; else

      ; ...

ENDIF1 NOP         ;endif

```

### If Elseif Else Statement

```
    ADD R1, R1, #0 ; Set CC to R1
    BRzp ELSEIF1   ; if R < 0

      ; ...

    BR ENDIF1


ELSEIF1 BRnp ELSE1  ; else if R == 0

      ; ...

    BR ENDIF1

ELSE1 NOP          ; else

    ; ...

ENDIF1 NOP         ; endif

```
