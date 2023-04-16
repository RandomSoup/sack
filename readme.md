# The Sack programming language
> Spec v0.0.5 SemVer

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
	- None (`none`)
- Sack enforces a style guide for improve readability. Compilers should by default warn for violations of the style guide.

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
This is also **invalid** syntax:
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

### Variables

Variables are dynamically determined by the implementation. Rules for this are as follows:

- Strings are variables surrounded by single or double quotes. Because of this, the following are valid strings.
```
let a = "hello";
let b = 'world';
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
let d = 0.0;
```

The following are vaild negative Numbers:
```
let a = -1;
let b = -0;
```

The following are valid negative Decimals:
```
let a = -10.43;
let b = -0.5;
```

Trailing or leading decimals on numbers are invalid and should be detected as such by the compiler. Because of this, the following are **invalid**
```
let a = .1;
let b = 5.;
```

Putting the minus sign in the back is invalid (unless used for subtraction). Because of this and the previous rule, the following are **invalid**
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
Because booleans (along with all other variables) are case sensitive, the following are **invalid**
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
You can also return none from a failed conditional, like so:
```
if a > b {

	return none;

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
# Not (Converts numbers to a negative form)
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
Print is the only language defined i/o function. It takes in a single variable and returns the output to the terminal appending a newline

The following are valid print statements:
```
print ( "hello" );
print ( 42 );
print ( a );  # Only valid if the variable "a" is defined, else it returns an error and the program stops.
```

### Loops
A loop will itterate between a range of numbers starting at the first number and ending at the last for example `range( 1, 5 )` is equal to `[1, 2, 3, 4, 5]`

Example loops:
```
loop ( a in range( 1, 100 ) ) {

	print ( a );

}
```
### Type casting
Most data types can be converted into any other data type.
```
# Convert to int.
# Can be done with all data types.
int(x)

# Convert to float.
# Can be done with all data types.
float(x)

# Convert to string.
# Can be done with all data types.
string(x)

# Convert to bool.
# Can be done with all data types.
bool(x)
```

### Importing functions
By using the `import` keyword you can use functions from other sack programs.

Example:

in exampleModule.sk:
```
func helloworld() {
    print ( "Helloworld" );
}
```

and in exampleProgram.sk:
```
import ( "exampleModule" );
helloworld();
```

You can import modules that are in the current directory without specifying the path to the file, else you will need to specify the path.
By sack first tries to import modlues ending with ".sk", if that fails it will look for files ending with ".sack", if that also fails it will not to compile and just produce errors.


### Quirks

- A string added to a string will append the string.
- A string added to a number or a number added to a string will cast the number to a string and then append the string.
- A string multiplied by a integer will result in more of the string.
- Any operation which contains a number and a decimal will result in a decimal.
- Semicolons are required to end a non commented line. Function declarations and conditionals do not need them as the closing `}` specifies the end.
- Naming
	- Identifiers may be given any alphanumeric name that does not start with a number. Underscores are allowed anywhere in the variable name

## Sample program(s)

FizzBuzz:
```
func checker ( num ) {
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

loop ( num in range( 1, 100 ) ) {
  checker ( num );
}
```
