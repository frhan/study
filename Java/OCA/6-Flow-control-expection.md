Flow control and Exceptions
-----

Flow Control
----
if statement
---
```java
if(boolean expression){  // must true or false
 //statement
}
```
- else block optional
- curly braces are optional
- else clause belongs to the innermost if statement to which it might possibly belong.
- the closest preceding if that doesn't have an else.

```java
int trueInt = 1;
if(trueInt) //illegal
boolean boo = false;
if(boo = true)
{
}
```
- the code compiles and runs fine and the if test succeeds because boo is set to true in the if argument.

```java
int x = 3;
if(x = 5) //won't compile; because x is not a boolean
{
}
```

Switch statement
---
- break statement are optional

```java
switch(expression){
    case constant1: //code block
    case constant2: //code block
    default : //code block
}
```
expression - `char,byte,short,int ,enum,String`
 - case constant must be a compile time constant
 - we can use only final variable that is immediately initialized with a literal value. 

```java
final int a = 1;
final int b;

b = 2;
int x = 0;
switch(x){
    case a:  //OK
    case b:  //compiler error
}

byte g = 2;
switch(g){
    case 23:
    case 128: //this code won't compile
}
```
- also illegal to have more than one case label using the same value
- legal to leverage the power of boxing in a switch expression.

```java
switch(new Integer(4)){
     case 4:  //legal
  }
```



