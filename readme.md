# The Sack programming language
> Spec v1.0.0 SemVer

Sack is a dynamically typed scripting language for beginner programmers that focusses on readability and ease of use over speed.

## Goals

- Sack is Simple. A program is executed from top to bottom. This means asynchronous code is impossible.
- Sack is small. The specification is minimal, and easy to learn as well as implement.
- Sack is Extensible. While the specification is not indended to be open ended, It should allow for new features where required.
- Sack is Easy. Language reserved words should have their basis in common english, and code written in it should prioritize readability.

## Basic notes

- Sack's default extension is `.sk` or `.sack`. All others are considered invalid by this spec version.
- Variable types can be one of the following:
	- String
	- Number (int)
	- Decimal (32 bit float)
	- Bool (`true` or `false`)
	- None (`none`)
	- Identifier
	- List (Itterable Scope)
	- file (File object)
- Sack enforces a style guide to improve readability, it is left up to the implementation whether this is a default warn or error. For simplicity, the style guide will not be described in this spec, however the example code will follow it.

## Syntax

### Variables

Variables are case sensitive and dynamically determined by the implementation. Rules for this are as follows:

- Strings are variables surrounded by single or double quotes. Because of this, the following are valid strings.
```
let a = "hello";
let b = 'world';
let c = "100";
```

Strings may not have mismatching quotes.
As such, the following is  **invalid** syntax:
```
let a = "hi';
let b = 'hello";
```

- Numbers are any variable consisting of digits 0-9.
The following are valid Numbers:
```
let a = 1;
let b = 0;
let c = 100;
```

- Likewise, Decimals are 32 bit floating point numbers.
The following are valid Decimals:
```
let a = 1.0;
let b = 0.5;
let c = 100.25;
let d = 0.0;
```

The following are vaild negative Numbers:
```
let a = -1;
let b = -0.5;
let c = -0;
```

Trailing or leading decimals on numbers are invalid and should be detected as such by the compiler. Because of this, the following is **invalid**:
```
let a = .1;
let b = 5.;
```

Trailing negative signs are also invalid (unless used for subtraction). Because of this and the previous rule, the following is **invalid**:
```
let a = 1-;
let b = -.5;
let c = 1.-;
```

- Boolean values are returnable from functions and conditionals, they represent `true` or `false` values
The following are valid booleans:
```
let a = true;
let b = false;
```
Because booleans (along with all other variables) are case sensitive, the following is **invalid**:
```
let a = True;
let b = fAlSe;
```

- None is the default return value for functions, it represents no value.
The following is a function that returns none:
```
func a () {
	
	print( "hi" );
	
}
```

You can also return none from a failed conditional, however it is reccomended that failed conditionals return false. Here is an example of a conditional that returns `none`:
```
if a > b {

	return none;

}
```

### Lists
Lists are self contained scope blocks which are itterable. See the scope section for rules on scope.

A list is defined using square brackets like so:
```
let x = [ 1, 2, 3 ];
```

Lists can also contain named data, for instance:
```
let a = 2;
let x = [ 1, a, 3];
```

Finally you can assign names to data outright:
```
let x = [ 1, a: 2, 3 ];
```

Accessing data in lists can be accomplished in a few ways. You can itterate over a list:
```
let x = [ 1, 2, 3 ];

# Lists start at index 0, so this will print numbers 1 to 3
loop ( num in range ( 0, 2 ) ) {
	print ( x [ num ] );
}
```

Data can also be accessed outright using it's name or a variable's content:
```
let a = 2;
let x = [ 1, a: 2, 3 ];

# returns 3
print ( x[a] );

# returns 2
print ( x['a'] );
```

If you try to access data that is beyond the length of a list, it is a runtime error as opposed to returning `none`

Note that this makes the following code **Invalid**:
```
let a = 10;
let x = [ 1, a: 2, 3 ];

if ( x[a] == none ) {
    # because the 10th index of the list does not exist this will error on runtime
}
```

### Files and Binary

File io is a core feature of Sack, and as such it has it's own syntax.

Files are opened using the `open` keyword, and closed using the `close` keyword. There are multiple modes for opening files, and they are as follows:
```
let file1 = open ( "file1.txt", "r" );
let file2 = open ( "file2.txt", "w" );
let file3 = open ( "file3.txt", "a" );
```

The first argument is the file path, and the second is the mode. The mode can be one of the following:
- `r` - Read
- `w` - Write
- `a` - Append

Files can be read using the `read` keyword, and written to using the `write` keyword.

Here is an example of a file being opened, read, and closed:
```
let file = open ( "file.txt", "r" );
let data = read ( file );
close ( file );
```
In this example, the `data` variable will be a string containing the contents of the file.

Here is an example of a file being opened, written to, and closed:
```
let file = open ( "file.txt", "w" );
write ( file, "hello world" );
close ( file );
```

Flushing a file is done using the `flush` keyword. This is useful for writing to files that are opened in append mode.
```
let file = open ( "file.txt", "a" );
write ( file, "hello world" );
flush ( file );
close ( file );
```

### Functions

Functions are declared using either the `func` or `functi` keywords followed by the name of the function.

As an example, here's a function that adds two numbers together:

```
func add_numbers ( a, b ) {

	let c = a + b;
	return c;

}
```

Functions can not be retroactively assigned to variables. As such, the following is **invalid** syntax:
```
let add_numbers = function( a, b ) {
	
	let c = a + b;
	return c;
	
}
```

So is this:
```
func add_numbers ( a, b ) {
	
	let c = a + b;
	return c;
	
}
let add_numbers_copy = add_numbers;
```

Functions can return a singular variable using the `return` keyword. Do note that any code after a return is unreachable and will be ignored by the implementation. As such, the following will result in unreachable code:
```
func add_numbers ( a, b ) {

	let c = a + b;
	return c;
	print( "hi" );

}
```

### Logical operators

The following are valid logical operators in sack:
```
# add to identifier
+=
# Addition
+
# Subtract from identifier
-=
# Subtraction
-
# Multiply
*
# Multiply from identifier
*=
# Divide
/
# Divide from identifier
/=
# Check equivalency 
==
# Check anti equivalency
!=
# Greater than
>
# Greater than or equal to
>=
# Less than
<
# Less than or equal to
<=
# Plus one
++
# Minus one
--
# Modulo
%
# And
&&
# Or (Not exclusive)
||
# Xor
^^
# Not (inverts a boolean value)
! 
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
Conditionals are compiler determined functions which check for a certain function.

The following are valid conditionals (assuming `a` is defined):
```
if a > 1 {

 print ( "Good morning." );

} else if 1 > a {

 print( "Good afternoon." );

} else { 

 print ( "Good night." );

}
```

Else if is valid because it's a combination of the else and the if operator, it is not sytatically unique.

### Print
Print is a language defined output function. It takes in a single variable and returns the output to the terminal appending a newline

The following are valid print statements:
```
print ( "hello" );
print ( 42 );
print ( a );  # Only valid if the variable "a" is defined, else it returns an error and the program stops.
```

### Input
Similar to print, Input is a language defined input function. It allows for a prompt, and will read text entered into the program.

The following is a valid input statement:
```
let a = input ( "Please enter your name: " )
```

### Loops
A loop will itterate between a range of numbers starting at the first number and ending at the last for example `range( 1, 5 )` is equal to `[1, 2, 3, 4, 5]`

The following is an example of valid loop in range syntax:
```
loop ( a in range( 1, 100 ) ) {

	print ( a );

}
```

While loops are also possible:
```
let a = 1
loop ( while a < 100 ) {
	print ( a );
	a = a + 1;
}
```

### Language Defined Functions
Sack defines a few functions for convenience. These are typically related to typecasting, and are reserved keywords.


Most data types can be converted into any other data type.
```
# Convert to int.
# Will round a Decimal to the nearest integer
int ( x );

# Convert to float.
# Will convert a Number to a Decimal
# For example, float(3) is equal to 3.0
float ( x );

# Convert to string.
# Can be done with all data types.
string ( x );

```

You can also quickly get the type of a variable using the `type()` function:
```
x = 3

# prints `Number`
print ( type ( x ) );

string ( x );

# prints `String`
print ( type ( x ) );
```

You can also get the length of a list with `len()`:
```
let x = [ 1, 2, 3 ];

# returns `2`
len( x );
```

### Importing functions
By using the `import` keyword you can use functions from other sack programs.

Example:
```
# exampleModule.sk
func helloworld() {
    print ( "Helloworld" );
}
```

```
# exampleProgram.sk
import ( "exampleModule" );

# prints "Helloworld" to the console
helloworld();
```

You can import modules that are in the current directory without specifying the path to the file, else you will need to specify the path.
By default sack first tries to import modlues ending with ".sk", if that fails it will look for files ending with ".sack", if that also fails it will cause an error on runtime.

### Scope
The following rules govern scope in sack.

1. You can only access from the global scope or the current scope.
2. The current scope has access to the scope above it, which has access to the scope above it, etc...
3. Calling a function makes a new scope with nothing but the global scope above it.
4. Functions can only access arguments or global vars that are defined before them.

Following these rules, the following should print four:
```
let e = 3;
functi print_e() { 
	print ( e ); 
}
e = 4;
print_e();
```

Likewise, the following should print `[e: 3, x: 5]`
```
let e = 3;
let list = [ e, x = 5 ];
e = 4;
print ( list );
```

### Quirks

- A string added to another string will concatenate the strings.
- A string added to a number or a number added to a string will cast the number to a string and then append the string.
- A string multiplied by a integer will result in more of the string.
- Any operation which contains a number and a decimal will result in a decimal.
- Semicolons are required to end a non commented line. Function declarations and conditionals do not need them as the closing `}` terminates the line.
- Naming
	- Identifiers may be given any alphanumeric name that does not start with a number. Underscores are allowed anywhere in the variable name

## Sample program(s)

FizzBuzz:
```
func checker ( num ) {
  let a = num % 15;
  let b = num % 5;
  let c = num % 3;
  if a == 0 {
    print ( "FizzBuzz" );
  }
  else if b == 0 {
    print ( "Fizz" );
  }
  else if c == 0 {
    print ( "Buzz" );
  }
  else {
    print( num );
  }    
}

loop ( num in range( 1, 100 ) ) {

	checker ( num );

}
```
