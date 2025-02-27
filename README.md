# eBin Bytecode
Function IDs (aka Dependency IDs) are prefixed by an `F`.
```java
F0 // Function ID = 0
```
Label IDs are prefixed by an `L`.
```java
L0 // Label ID = 0
```
Comparison IDs are prefixed by a `C`.
```java
C0 // Comparison ID = 0
```
Numbers are prefixed by a `N`.
```java
N0 // Number = 0
```
Strings are prefixed by a `S`, followed by the strings length, and the string encoded as a series of numbers
```java
S3 N102 N111 N111 // String = "foo"
```
Instructions are not prefixed by any character.
```java
1 // increment pointer value instruction
```
ePU ROM Positions are translated.
```nasm
CDP [10 12] [foo]
```
```cpp
10 N10 N12 F0
```

### Example of eBin program
Hades
```nasm
CDP ["somefile.hds"] [bar]
INCV
CLB [foo]
INCP
INT [foo == 1] [bar]
HLT
```
eBin
```cpp
10 S12 N... F0 0 15 L0 2 23 L0 C1 N1 F0 14
```

### Full Instruction List
```
0 : increment pointer value
1 : decrement pointer value
2 : increment pointer position
3 : decrement pointer position
4 : start loop
5 : end loop
6 : write value
7 : request next character
8 : push to stack
9 : pop from stack
10 : create function
11 : call function
12 : read value from tape
13 : syscall/write to terminal
14 : end program/return from function
15 : create label
16 : jump to label
17 : delete label
18 : read pointer position into pointer value
19 : set pointer value to following number
20 : subtract 255 from pointer position
21 : add 255 to pointer position
22 : set pointer position to following number
23 : conditional function call
24 : No operation
25 : write following number to tape
```

### ePU Storage Format
Non-program data is stored with `00` appended before the actual data
Program data is stored with `1` then the corresponding type indicator:
- `0` : Instructions
- `1` : `F`
- `2` : `L`
- `3` : `N`
- `4` : `C`
- `5` : `S`
