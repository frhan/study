Assignments
---

Stack and Heap
---
- instance variable and objects live on the heap
- local variables live on the stack

Literals,assignments and variables
---
- Java 7,numeric literals can be declared using underscore character(_) 

```java
 int pre7 = 10000; //legal
 int with7 = 1_00_000; //legal
 int i1 = _1_00_000; //illegal can't begin with an " _"
 int i2 = 1_0000_000; 
```       
Decimal literals
---
`int light = 343;`
Binary literals
---
`int b1 = 0B10101010;`
Octal Literals
---
Int seven = 07
Hexadecimal Literals
---
`int x = 0x0001`

- Accept lowercase/uppercase letters for extra digit
- Allowed upto 16 digit in a hexadecimal number

int z = 0xDeadCafe
int z = 0xDEADCAFE

Floating point literals
---
`double d = 11130202.0123`

double(64bits)  - defautt
float(32bits) - must attach suffix f or F to the number

`float f = 23.405050500 // compiler error`

Boolean literals
---

```java
char a  = ‘a’;
char b = ‘@’;
char letterN = ‘\u004E’ //letter N
char c = (char) 7000;//legal
char d = (char) -98; //legal
char e = -29 //illegal
char f = 7000 ; //illegal
```
Characters are 16 bit unsigned integer under the hood.

Assignment Operators
---
```Button b = new Button();```
- A variable refering to an object is just that  - a reference variable
- A reference variable bit holder contain bit representing a way to get to the object

Primitive Assignment
---
```byte b = 27; //legal```
- Compiler automatically narrows the literal value to a byte
Integer or smaller expression always resulting in an int

```java
byte a = 3;
byte b = 3;
byte c = a+b; //won’t compile
byte c = (byte)(a+b);//legal
Int j,k=1,l,m = k+3; // legal; k is initiated before m uses it
Int j,k=m+3,l,m = 1 // illegal; m is not initiated before k uses it
```
Primitive Casting
---
- Cast can be implicit or explicit
- Implicit cast happens when we’re doing widening conversion
 - 



