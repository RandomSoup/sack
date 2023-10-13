# The Sack programming language
> Spec v1.0.0 SemVer

Sack is a dynamically typed scripting language for beginner programmers that focuses on readability and ease of use over speed.

## Goals

- Sack is Simple. A program is executed from top to bottom. This means asynchronous code is impossible.
- Sack is small. The specification is minimal, and easy to learn as well as implement.
- Sack is Extensible. While the specification is not intended to be open ended, It should allow for new features where required.
- Sack is Easy. Language reserved words should have their basis in common english, and code written in it should prioritize readability.

## Basic notes

- Sack's default extension is `.sk` or `.sack`. All others are considered invalid by this spec version.
- Variable types can be one of the following:
	- String
	- Number (signed 32 bit int)
	- Decimal (32 bit float)
	- Bool (`true` or `false`)
	- None (`none`)
	- List (Iterable Scope)
	- File (File object)
	- Byte (8 bit unsigned int)
	- Function
- Sack enforces a style guide to improve readability. Compilers should by default warn for violations of the [style guide](style-guide.md).

## Syntax

### Variables

Variables are case sensitive and dynamically determined by the implementation. Rules for this are as follows:

- Strings are variables surrounded by single or double quotes. Because of this, the following are valid strings.
```
let a = "hello";
let b = 'world';
let c = "100";
```

Chaining declarations is also valid:
```
let a = "hello", b = 'world', c = "100";
```

Strings may not have mismatching quotes.
As such, the following is  **invalid** syntax:
```
let a = "hi';
let b = 'hello";
```

Strings can be zero indexed, which returns a single character long string. For example:
```
let a = "Hello";
# Prints H
print( a[0] );
# Prints true
print( a[2] == "l" );
# Throws an error (47 is more than 4)
print( a[47] );
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

The following are valid negative Numbers:
```
let a = -1;
let b = -0;
```

The following are valid negative Decimals:
```
let a = -10.43;
let b = -0.5;
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
Because booleans (along with all other identifiers) are case sensitive, the following is **invalid**:
```
let a = True;
let b = fAlSe;
```

- None is the default return value for functions, it represents no value.
The following is a function that returns none:
```
functi a () {
	
	print( "hi" );
	
}
```

You can also return none from a failed conditional, however it is recommended that failed conditionals return false. Here is an example of a conditional that returns `none`:
```
if a > b {

	return none;

}
```

- Byte is an 8 bit integer, and is used for binary data.
The following are valid bytes:
```
let a = 0b00000000;
let b = 0b11111111;
```

### Lists
Lists are self contained iterable scope blocks.

A list is defined using square brackets like so:
```
let x = [ 1, 2, 3 ];
```

Lists can also contain named data, for instance:
```
let a = 2;
# [ 1, a: 2, 3 ]
let x = [ 1, a, 3];
```

Finally you can assign names to data outright:
```
let x = [ 1, a: 2, 3 ];
```

You can put a variable's content in a list (without giving it a name in the list) like so:
```
let a = 1;
# [ 0, 1, 2]
let x = [ 0, :a, 2]

# This is also possible, although not very useful
# [ 3 ]
let y = [ :3 ]
```

Accessing data in lists can be accomplished in a few ways. You can iterate over a list:
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

Lists can be appended to:
```
let x = [ 1, 2, 3 ];
x += 4;

let y = []
y += 1;

# x is now [ 1, 2, 3, 4 ]
# y is now [ 1 ]
```

Lists can also be concatenated:
```
let x = [ 1, 2, 3 ];
x += [ 4, 5, 6 ];

# x is now [ 1, 2, 3, 4, 5, 6 ]
```

To append a list into a list, surround the list with brackets:
```
let x = [ 1, 2, 3 ];
x += [[ 4, 5, 6 ]];

# x is now [ 1, 2, 3, [ 4, 5, 6 ] ]
```

Lists can be multiplied:
```
let x = [ 1 ] * 3;
x += [ 2, 3 ] * 2;

# x is now [ 1, 1, 1, 2, 3, 2, 3 ]
```

### Files and Binary

File IO is a core feature of Sack, and as such it has it's own syntax.

Files are opened using the `open` keyword, and closed using the `close` keyword. There are multiple modes for opening files, and they are as follows:
```
let file1 = open ( "filename", "mode" );
```

The first argument is the file path, and the second is the mode. The mode can be one of the following:
- `r` - Read
- `w` - Write
- `a` - Append
- `rb` - Read Binary
- `wb` - Write Binary

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

Performing an invalid operation, such as writing to a read only file or vice versa, will result in an error. For example, the following code will error:
```
let file = open ( "file.txt", "r" );
write ( file, "hello world" );
close ( file );
```

To write to a specific location in a file, you can use the `seek` keyword. This will move the file pointer to a specific location in the file. The following code will write to the 5th byte in the file:
```
let file = open ( "file.txt", "w" );
seek ( file, 5 );
write ( file, "hello world" );
close ( file );
```
Overseeking will result in an error. For example, the following code will error:
```
let file = open ( "file.txt", "w" );

# file.txt contains the string "hello world"
# The error will occur when attempting to seek, not when writing
seek ( file, 100 );

# This will not be reached
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

Files can also be read and written to using binary data. This is done using the `read` and `write` keywords with the file opened in binary mode. Reading and writing binary data is done using the `byte` type. In this case the file object will be a list of bytes.
```
let file = open ( "file.txt", "rb" );
let data = read ( file );

# data is now a list of bytes
print ( data );

close ( file );
```

File data can also be printed to the console. The following code will print the contents of a file to the console:
```
# file.txt contains the text "hello world"
let file = open ( "file.txt", "r" );
# prints "hello world"
let data = read ( file );
print ( data );
close ( file );
```

Files can be iterated over using the `loop` keyword. The following code will print the contents of a file to the console:
```
# file.txt contains the text "hello world"
let file = open ( "file.txt", "rb" );
let data = read ( file );
loop ( byte in data ) {
	print ( byte );
}
```
alternatively:
```
# file.txt contains the text "hello world"
let file = open ( "file.txt", "r" );
let data = read ( file );
loop ( line in data ) {
	print ( line );
}
```

### Functions

Functions are declared using either the `func` or `functi` keywords followed by the name of the function.

As an example, here's a function that adds two numbers together:

```
functi add_numbers ( a, b ) {

	let ret = a + b;
	return ret;

}
```

Functions aren't allowed to be anonymous. As such, the following is **invalid** syntax:
```
functi( a, b ) {
	
	let ret = a + b;
	return ret;
	
}
```

Functions can be assigned to variables however:
```
functi add_numbers ( a, b ) {
	
	let ret = a + b;
	return ret;
	
}
let add_numbers_copy = add_numbers;
```

Functions can also be overloaded, this is done based on the number of arguments:
```
functi add_numbers ( a, b ) {
    
    let ret = a + b;
    return ret;
    
}

functi add_numbers ( a, b, c ) {
    
    let ret = a + b + c;
    return ret;

}
```

The overloaded function to use is chosen when it is called, this may seem obvious but consider the following (which is valid):
```
functi add_numbers ( a, b ) {
    
    let ret = a + b;
    return ret;
    
}

functi add_numbers ( a, b, c ) {
    
    let ret = a + b + c;
    return ret;

}

# Is this add_numbers with 2 or 3 args? It's both!
let adder = add_numbers;

# Calls the 2 argument version
adder ( 47, 17 );
# Calls the 3 argument version
adder ( 21, 5, 1940 );
```

Functions can return a singular variable using the `return` keyword. Do note that any code after a return is unreachable and will be ignored by the implementation. As such, the following will result in unreachable code:
```
functi add_numbers ( a, b ) {

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
By extension `<=` and `>=` are also valid since they are a combination of the greater than and less than operators with the equal operator.
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

Else if is valid because it's a combination of the else and the if operator, it is not syntactically unique.

### Print
Print is a language defined output function. It takes in a single variable and returns the output to the terminal appending a newline

The following are valid print statements:
```
print ( "hello" );
print ( 42 );
print ( a );  # Only valid if the variable "a" is defined, else it returns an error and the program stops.
```
Print can accept the following types:
- String
- Number
- Decimal
- Byte
- List
- Bool

### Input
Similar to print, Input is a language defined input function. It allows for a prompt, and will read text entered into the program.

The following is a valid input statement:
```
let a = input ( "Please enter your name: " );
```

### The `in` keyword
The `in` keyword is used to check if a value is in a list or string. It returns a boolean value.

The following are valid `in` statements:
```
let a = "hello";
let b = "h";
# prints true
print ( b in a );

let c = "z";
# prints false
print ( c in a );

let d = [ 1, 2, 3, 4, 5 ];
let e = 1;
# prints true
print ( e in d );

let f = 6;
# prints false
print ( f in d );
```

It follows that conditional statements can also use the `in` keyword:
```
let a = "hello";
let b = "h";
if ( b in a ) {
	print ( "hello contains h" );
}
```

Invalid types will result in an error:
```
let a = "forty-seven";
let b = 47;

# This will result in an error
print ( b in a );
```

### Loops
A loop will iterate between a range of numbers starting at the first number and ending at the last for example `range( 1, 5 )` is equal to `[1, 2, 3, 4, 5]`

The following is an example of valid loop in range syntax:
```
loop ( a in range( 1, 100 ) ) {

	print ( a );

}
```

While loops are also possible:
```
let a = 1;
loop ( while a < 100 ) {
	print ( a );
	a += 1;
}
```

There is a quicker way to write a infinite loop:
```
loop {
    print("This will print forever!");
}
```

The keywords `break` and `continue` will exit the loop, or continue to the next iteration respectively.

```
# Looping from 1 to 10, but breaking out at 5
let a = 1;
loop ( while a <= 10 ) {
	if a == 5 {
		break;
	}
}

# Looping from 1 to 10 but only the odd numbers
a = 1;
loop ( while a <= 10 ) {
	if a % 2 {
		continue;
	}
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

# Convert a byte to a string.
string ( x );

# Convert a string to a list of bytes.
# if the string is a single character, it will return a single byte.
byte ( x );
```
When converting between types, invalid conversions will return `none`.

You can quickly get the type of a variable using the `type()` function:
```
let x = 3;

# prints `Number`
print ( type ( x ) );

x = string ( x );

# prints `String`
print ( type ( x ) );

# prints `Functi`
print ( type ( print ) );
```

You can get the length of a list or string with `len()`:
```
let x = [ 1, 2, 3 ];

# prints `2`
print ( len( x ) );

let s = "Test"; 

# prints `3`
print ( len( s ) );

# prints `none`
print ( len( [] ) );
```

You can get the arguments passed to a function with `args()`:
```
functi test ( a, b, c ) {
	print ( args() );
}
```
Args will also return a list of arguments passed to the program if called outside of a function. with the first argument being the name/path of the program.
```
# prints `["example.sk"]`
print ( args() );
```
If the function that is called has no arguments, `args()` will return `none`, however if args is called in the global scope it's length will always be greater than 0.

### Importing functions
By using the `import` keyword you can use functions from other sack programs.

Example:
```
# exampleModule.sk
functi helloworld() {
    print ( "Helloworld" );
}
```

```
# exampleProgram.sk
import "exampleModule";

# prints "Helloworld" to the console
helloworld();
```

You can import modules that are in the current directory without specifying the path to the file, else you will need to specify the path.
By default sack first tries to import modules ending with ".sk", if that fails it will look for files ending with ".sack", if that also fails it will cause an error on runtime.

Because there are multiple valid extensions for a Sack file, you can also specify the extension directly. Be careful though, as a mismatch will error:
```
import "exampleModule.sk";

# The following will error, as the file extension is incorrect
import "exampleModule.sack";
```

You may also import a package, by simply importing a folder:
```
# examplePackage/main.sk
functi helloworld() {
    print ( "Helloworld" );
}

# exampleProgram.sk
import "examplePackage";

# prints "Helloworld" to the console
helloworld();
```

### Scope
The following rules govern scope in sack.

1. You can only access from the global scope or the current scope.
2. The current scope has access to the scope above it, which has access to the scope above it, etc...
3. Calling a function makes a new scope with nothing but the global scope above it.
4. Functions can only access arguments or global vars that are defined before them.

There are also some rules for defining functions and variables:
1. Functions and variables can not be defined with the name of a builtin function.
2. Functions can not be defined if an existing variable has that name, or if a function with the same number of args exists.
3. Variables can not be defined if a function with the same name exists.

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
let list = [ e, x: 5 ];
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
functi checker ( num ) {
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
