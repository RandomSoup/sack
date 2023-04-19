# Sack Style Guide

## Naming convention

Functions and variables should use `snake_case`, unless they are supposed to be used as constants in which case they should use `SCREAMING_SNAKE_CASE`.

Never use `l` (lower case L), `I` (uppercase i), or `O` (uppercase o), for variable names as they look too much like numbers.

If you don't want people to use your function/variable unless they are sure they know exactly what they are doing, prefix it with a single underscore.
Variables and functions specific to a certain implementation should have two underscores, the implementations name, an underscore, and then name of the variable/function.

## Comments

Comments should be directly before the line they describe, one space should be put after the `#`.

Comments should start with an uppercase letter but should not end with a period, however an exclamation or question mark is allowed. For example:

```
# Calculates foo
func calc_foo () {
    # Returns 4
    return 4;
}

# Stores foo
let foo = calc_foo();
```

## Spacing

Every binary operator, parenthesis, and brace should have 1 space around it, with the exception of function calls, list indexes, and semicolons. Unary operators should not have space. For example:

```
if a[0] == -b {
    print( "Hello!" );
}
```

### Indentation

After every left brace should be an indent of 4 spaces, which should end before the closing right brace. For example:
```
loop (while foo > 2) {
    print( "Foo is greater than 2" );
    foo -= 1;
}
```

Trailing expressions follow the same rules but with with `[]` and `()`. For example:
```
takes_long_args(
    this_is_a_really_really_long_argument_name,
    these_long_names_should_change
);
```

Trailing expressions may be on the same line as other trailing expressions. For example:
```
let my_list = [
    1, 2, 3,
    4, 5, 6
    7, 8, 9,
    0
];
```

Trailing binary operators should have the operator on the second line, unless the binop changes the a value. for example:
```
let sum =
    a_really_long_var_name
    + some_other_long_var;
```

## Brace style

All left braces should be inline with the rest of the line, all right braces should be on there own line (with the exception of `else` and `else if`), for example:

```
func foo ( bar ) {
    if bar {
        # Some code here...
    } else {
        # More code here...
    }
}
```

## Code location

Imports should always be at the top of files, and sorted alphabetically, with an extra newline after the final import.

All globals variables should be after imports, with an extra newline after the final variable.

Following after global variables should be functions. All functions should have a newline after the final `}`.

All other code should be at the bottom of the file.

## Returning

When returning from all branches of an `if-(else-if)*-else`, it is better to remove the else. For example:
```
# Wrong
func is_even ( a ) {
    if ( type(a) != "Number" ) {
        return false;
    } else {
        return a % 2;
    }
}

# Correct
func is_even ( a ) {
    if ( type(a) != "Number" ) {
        return false;
    }
    return a % 2;
}
```

## Unary vs binary increment/deincrement operators

Unary increment and deincrement operators (`++` and `--`) should only be used when the value is not needed, they should not be used when `+=` or `-=` would work.
