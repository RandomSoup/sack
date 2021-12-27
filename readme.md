# The Sack programming language
> Spec v0.0.3 SemVer

Sack is a dynamically typed scripting language for beginner programmers that focusses on readability and ease of use over speed.

## Goals

- Sack is Simple. A program is executed from top to bottom with no async requests.
- Sack is small. The specification is minimal, and simple to implement in a language of choice.
- Sack is Extensible. While the specification is not indended to be open ended, It allows for changes where necessary.
- Sack is Easy. Keywords should be recognizable to both programmers and non programmers alike.

## Basic notes

- Sack does not have inbuilt networking or the ability to import such functionality from other files.
- Sack's default extension is `.sk` or `.sack`. All others are considered invalid by this spec version.
- Variable types can be one of the following:
	- String
	- Number (int)
	- Decimal (32 bit float)
	- Bool (`true` or `false`)
- Sack enforces a style guide for improve readability, It is left up to the implementation whether this is a default warn or error.

## Basic keywords

### Functions

Functions are declared using the `func` keyword, followed by a declaration of local variables,

As an example, here's a function that adds two numbers together:

```
func add_numbers ( a, b ) {

	let c = a + b;
	return c;

}
```

Functions can not be assigned to variables. The following is **invalid** syntax:
```
let add_numbers = function( a, b ) {
	
	let c = a + b;
	return c;
	
}
```
Functions can return a singular variable using the `return` keyword. Do note that any code after a return is unreachable and will be ignored by the implementation. As such, the following is **invalid** syntax:
```
func add_numbers ( a, b ) {

	let c = a + b;
	return c;
	print( "hi" );

}
```
The previous example is also valid in conditionals.

### Variables

Variables are dynamically determined by the implementation. Rules for this are as follows:

- Strings are variables surrounded by single or double quotes. Because of this, the following are valid strings.
```
let a = "hello";
let b = "world";
let c = "100";
```

- Numbers and Decimals are any variable with a digit 0-9 in them.
The following are valid Numbers:
```
let a = 1;
let b = 0;
let c = 100;
```

The following are valid Decimals:
```
let a = 1.0;
let b = 0.5;
let c = 100.25;
```

Trailing or leading decimals on numbers are invalid and should be detected as such by the compiler. Because of this, the following are **invalid**
```
let a = .1;
let b = 5.;
```

### Logical operators

The following are valid logical operators in sack:
```
# add 1 to variable
+=
# Addition
+
# Subtract 1 from variable
-=
# Subtraction
-
# Check equivalency 
==
# Check anti equivalency
!=
# Greater than
>
# Less than
<
# Modulo
%
# And
&&
# Or
||
```
By extension `<=` and `>=` are also valid since they are a combination of the geater than and less than operators with the equal operator.
Operators which check for a condition return a boolean value `true` or `false`.

### Comments
Comments are lines not executed by the parser that are used to enhance readability.

- All comments exist at the start of a new line and obey indentation (not compiler enforced)
- All comments begin with the `#` symbol and end with a newline
- A comment cannot be the last line of a program

The following are valid comments:
```
# hello
#hello

```

### If, else, else if
conditionals are compiler determined functions which check for a certain function.

The following are valid conditionals:
```
if ( 2 > 1 ) {

 print ( "hello" );

} 
else { 

 print ( "goodbye" );

}
```

Else if is valid because it's a combination of the else and the if operator, it is not sytatically unique.

### Print
Print is the only language defined i/o function. It takes in a single variable and returns the output to the terminal appending a newline

The following are valid print statements:
```
print ( "hello" );
print ( a );
```

### Loops
A loop will itterate between a range of numbers.
Example loops:
```
loop ( a in range, ( 1, 100 ) ) {

	print ( a );

}
```

### Quirks

- A string added to a string will append the string
- A string added to a number or a number added to a string will cast the number to a string and then append the string
- Any operation which contains a number and a decimal will result in a decimal.
- Semicolons are required to end a non commented line. Function declarations and conditionals do not need them as the closing `}` specifies the end.

## Sample program(s)

FizzBuzz:
```
funct checker ( num ) {
  if ( num % 15 ) {
    print ( "FizzBuzz" )
  }
  else if ( num % 3 ) {
    print ( "Fizz" );
  }
  else if ( num % 5 ) {
    print ( "Buzz" );
  }
  else {
    print ( num );
  }    
}

loop ( num in range, ( 1, 100 ) ) {
  checker ( num );
}
```
