# While Loop

Note: These are templates not stand alone programs



### Basic While Loop Example

<pre><code><strong>WHILE1 ADD R1, R1, #0 ; Set CC to R1
</strong>    BRnz ENDW1        ; while R1 > 0

        ; ...

    BR WHILE1
ENDW1 NOP             ; endwhile
</code></pre>

### Break Example

```
WHILE1 ADD R1, R1, #0 ; Set CC to R1
    BRnz ENDW1        ; while R1 > 0

    ADD R2, R2, #0    ; Set CC to R2
    BRnp ENDW1        ; break if R2 != 0

    BR WHILE1
ENDW1 NOP             ; endwhile
```

### Continue Example

```
WHILE1 ADD R1, R1, #0 ; Set CC to R1
    BRnz ENDW1        ; while R1 > 0

    ADD R2, R2, #0    ; Set CC to R2
    BRzp WHILE1       ; continue if R2 >= 0

    BR WHILE1
ENDW1 NOP             ; endwhile
```
