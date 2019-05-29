---
title: Odin Documentation
warning: true
---

## Introduction

This article is a basic tutorial for the programming language _Odin_. This tutorial assumes a basic knowledge of programming concepts such as variables, statements, and types. It is recommend to read the [Getting started with Odin](https://github.com/odin-lang/Odin/wiki#getting-started-with-odin) guide.

## Hellope!

To begin this tour, let us start with a modified version of the famous "hello world" program:
```odin
package main

import "core:fmt"

main :: proc() {
	fmt.println("Hellope!");
}
```
Save this code to the file "hellope.odin". Now compile and run it:
```
odin run hellope.odin
```
The `run` command compiles the `.odin` file to an executable and then runs that executable after compilation. If you do not wish to run the executable after compilation, the `build` command can be used.
```
odin build hellope.odin
```

## Lexical elements and literals
### Comments
Comments can be anywhere outside of a string or character literal. Single line comments begin with `//`:
```odin
// A comment

my_integer_variable: int; // A comment for documentation
```

Multi-line comments begin with `/*` and end with `*/`. Multi-line comments can be also be nested (unlike in C):
```odin
/*
	You can have any text or code here and
	have it be commented.
	/*
		NOTE: comments can be nested!
	*/
*/
```

Comments are parsed as tokens within the compiler. This is to allow for future work on automatic documentation tools.


### String and character literals
String literals are enclosed in double quotes and character literals in single quotes. Special characters are escaped with a backslash `\`.
```odin
"This is a string"
'A'
'\n' // newline character
"C:\\Windows\\notepad.exe"
```

Raw string literals are enclosed in single back ticks.

```odin
`C:\Windows\notepad.exe`
```

The length of a string can be found using the built-in `len` proc:
```odin
len("Foo")
len(some_string)
```

If the string passed to `len` is a compile-time constant, the value from `len` will be a compile-time constant.

#### Escape Characters
* `\a` - bell (BEL)
* `\b` - backspace (BS)
* `\e` - escape (ESC)
* `\f` - form feed (FF)
* `\n` - newline
* `\r` - carriage return
* `\t` - tab
* `\v` - vertical tab (VT)
* `\\` - backslash
* `\"` - double quote (if needed)
* `\'` - single quote (if needed)
* `\NN`- octal 4 bit character (2 digits)
* `\xNN` - hexadecimal 8 bit character (2 digits)
* `\uNNNN` - hexadecimal 16-bit Unicode character UTF-8 encoded (4 digits)
* `\UNNNNNN` - hexadecimal 24-bit Unicode character UTF-8 encoded (6 digits)

### Numbers
Numerical literals are written similar to most other programming languages. A useful feature in Odin is that underscores are allowed for better readability: `1_000_000_000` (one billion). A number that contains a dot is a floating point literal: `1.0e9` (one billion). If a number literal is suffixed with `i`, is an imaginary number literal: `2i` (2 multiply the square root of -1).

Binary literals are prefixed with `0b`, octal literals with `0o`, and hexadecimal literals `0x`. A leading zero does not produce an octal constant (unlike C).

In Odin, if a number constant is possible to be represented by a type without precision loss, it will automatically convert to that type.
```
x: int = 1.0; // A float literal but it can be represented by an integer without precision loss
```

Constant literals are "untyped" which means that they can implicitly convert to a type.
```
x: int; // `x` in typed of type `int`
x = 1; // `1` is an untyped integer literal which can implicitly convert to `int`
```

## Variable declarations
A variable declaration declares a new variable for that current scope.
```
x: int; // declares x to have type `int`
y, z: int; // declares y and z to have type `int`
```

Variables are initialized to zero by default unless specified otherwise.

## Assignment statements

The assignment statement assigns a new value to a variable/location:
```
x: int = 123; // declares a new variable `x` with type `int` and assigns a value to it
x = 637; // assigns a new value to `x`
```
`=` is the assignment operator.

You can assign multiple variables with it:
```
x, y := 1, "hello"; // declares `x` and `y` and infers the types from the assignments
y, x = "bye", 5;
```

**Note:** `:=` is two tokens, `:` and `=`. The following are all equivalent:
```
x: int = 123;
x:     = 123; // default type for an integer literal is `int`
x := 123;
```

## Constant declarations
Constants are entities (symbols) which have an assigned value. The constant's value cannot be changed. The constant's value must be able to be evaluated at compile time:
```
x :: "what"; // constant `x` has the untyped string value "what"
```
Constants can be explicitly typed like a variable declaration:
```
y : int : 123;
z :: y + 7; // constant computations are possible
```

## Packages
Every Odin program is made up of packages. Programs begin running in the package `main`.

### Import statement

The following program imports the the `fmt` and `os` packages from the `core` library collection.

```
package main

import "core:fmt"
import "core:os"

main :: proc() {
}
```

The `core:` prefix is to state where the import is meant to look; this is called a library collection. If no prefix is present, the import will look relative to current file.


**Note**: By convention, the package name is the same as the last element in the import path. `"core:fmt"` package comprises of files that begin with the statement `package fmt`. However, this is not enforced by the compiler, which means that default name for the import name will be determined by the last element in the import path if possible.

A different import name can be used over the default package name:
```
import "core:fmt"
import foo "core:fmt" // reference a package by different name
```

### Exported names
In Odin, a name is exported from a package if it does _not_ begin with an underscore `_`. For example, `foo` is an exported name, but `_bar` will not be exported.

Import names are not exported by the package as they are local to that file in the package and not the package itself.

The attribute `@(private)` can be applied to an entity to prevent it from being exported from a package.
```
@(private)
my_variable: int;
```


## Control flow statements

### For statement
Odin has only one loop statement, the `for` loop.

#### Basic for loop

A basic `for` loop has three components separated by semicolons:
* The initial statement: executed before the first iteration
* The condition expression: evaluated before every iteration
* The post statement: executed at the end of every iteration

The loop will stop executing when the condition is evaluates to `false`.

```odin
for i := 0; i < 10; i += 1 {
    fmt.println(i);
}
```

**Note:** Unlike other languages like C, there are no parentheses `( )` surrounding the three components. Braces `{ }` or a `do` are always required.
```odin
for i := 0; i < 10; i += 1 { }
for i := 0; i < 10; i += 1 do single_statement();
```

The initial and post statements are optional:
```odin
i := 0;
for ; i < 10; {
    i += 1;
}
```
These semicolons can be dropped. This `for` loop is equivalent to C's `while` loop:
```odin
i := 0;
for i < 10 {
    i += 1;
}
```

If the condition is omitted, this produces an infinite loop:
```odin
for {
}
```

#### Range-based for loop
The basic for loop
```
for i := 0; i < 10; i += 1 {
    fmt.println(i);
}
```
can also be written
```
for i in 0..<10 {
    fmt.println(i);
}
// or
for i in 0..9 {
    fmt.println(i);
}
```
where `a..b` denotes an open interval `[a,b]`, i.e. the upper limit is *inclusive*, and `a..<b` denotes a half-open interval `[a,b)`, i.e. the upper limit is *exclusive*.

Certain built-in types can be iterated over:
```
for character in some_string {
    fmt.println(character);
}
for value in some_array {
    fmt.println(value);
}
for value in some_slice {
    fmt.println(value);
}
for value in some_dynamic_array {
    fmt.println(value);
}
for value in some_map {
    fmt.println(value);
}
```

Alternatively a second index value can be added:
```
for character, index in some_string {
    fmt.println(index, character);
}
for value, index in some_array {
    fmt.println(index, value);
}
for value, index in some_slice {
    fmt.println(index, value);
}
for value, index in some_dynamic_array {
    fmt.println(index, value);
}
for key, value in some_map {
    fmt.println(key, value);
}
```
The iterated values are *copies* and cannot be written to. The following idiom is useful for iterating over a container in a by-reference manner:
```
for _, i in some_slice {
    some_slice[i] = something;
}
```

### If statement

Odin's `if` statements do not need to be surrounded by parentheses `( )` but braces `{ }` or `do` is required.

```
if x >= 0 {
    fmt.println("x is positive");
}
```
Like `for`, the `if` statement can start with an initial statement to execute before the condition. Variables declared by the initial statement are only in the scope of that `if` statement.

```
if x := foo(); x < 0 {
    fmt.println("x is negative");
}
```

Variables declared inside an `if` initial statement are also available to any of the `else` blocks:
```
if x := foo(); x < 0 {
    fmt.println("x is negative");
} else if x == 0 {
    fmt.println("x is zero");
} else {
    fmt.println("x is positive");
}
```

### Switch statement
A switch statement is another way to write a sequence of if-else statements. In Odin, the default case is denoted as a case without any expression.

```
package main

import "core:fmt"
import "core:os"

main :: proc() {
    switch arch := ODIN_ARCH; arch {
    case "386":
        fmt.println("32 bit");
    case "amd64":
        fmt.println("64 bit");
    case: // default
        fmt.println("Unsupported architecture");
    }
}
```
Odin's `switch` is like one in C or C++, except that Odin only runs the selected case. This means that a `break` statement is not needed at the end of each case. Another important difference is that the case values need not be integers nor constants.

To achieve a C-like fall through into the next case block, the keyword [`fallthrough`](#fallthrough-statement) can be used.


Switch cases are evaluated from top to bottom, stopping when a case succeeds. For example:
```
switch i {
case 0:
case foo():
}
```
`foo()` does not get called if `i==0`. If all the case values are constants, the compiler may optimize the switch statement into a jump table (like C).


A `switch` statement without a condition is the same as `switch true`. This can be used to write a clean and long if-else chain and have the ability to [`break`](#fallthrough-statement) if needed
```
switch {
case x < 0:
    fmt.println("x is negative");
case x == 0:
    fmt.println("x is zero");
case:
    fmt.println("x is positive");
}
```

A `switch` statement can also use ranges like a range-based loop:
```go
switch c {
case 'A'..'Z', 'a'..'z', '0'..'9':
	fmt.println("c is alphanumeric");
}

switch x {
case 0..<10:
	fmt.println("units");
case 10..<13:
	fmt.println("pre-teens");
case 13..<20:
	fmt.println("teens");
case 20..<30:
	fmt.println("twenties");
}
```

### Defer statement
A defer statement defers the execution of a statement until the end of the scope it is in.

The following will print `4` then `234`:
```
package main

import "core:fmt"

main :: proc() {
    x := 123;
    defer fmt.println(x);
    {
        defer x = 4;
        x = 2;
    }
    fmt.println(x);

    x = 234;
}
```

You can defer an entire block too:
```
{
    defer {
        foo();
        bar();
    }
    defer if cond {
        bar();
    }
}
```

Defer statements are executed in the reverse order that they were declared:
```
defer fmt.println("1");
defer fmt.println("2");
defer fmt.println("3");
```
Will print `3`, `2`, and then `1`.

A real world use case for `defer` may be something like the following:
```
f, err := os.open("my_file.txt");
if err != os.ERROR_NONE {
    // handle error
}
defer os.close(f);
// rest of code
```
In this case, it acts akin to am explicit C++ destructor however, the error handling is basic control flow.


### When statement
The `when` statement is almost identical to the `if` statement but with some differences:
* Each condition must be a constant expression as a `when` statement is evaluated at compile time.
* The statements within a branch do not create a new scope
* The compiler checks the semantics and code __only__ for statements that belong to the first condition that is `true`
* An initial statement is not allowed in a `when` statement
* `when` statements are allowed at file scope

Example:
```
when ODIN_ARCH == "386" {
	fmt.println("32 bit");
} else when ODIN_ARCH == "amd64" {
	fmt.println("64 bit");
} else {
	fmt.println("Unsupported architecture");
}
```

The `when` statement is very useful for writing platform specific code. This is akin to the `#if` construct in C's preprocessor however, in Odin, it is type checked.

### Branch statements
#### Break statement
A for loop or a switch statement can be left prematurely with a `break` statement. It leaves the innermost construct, unless a label of a construct is given:
```
for cond {
    switch {
    case:
        if cond {
            break; // break out of the `switch` statement
        }
    }

    break; // break out of the `for` statement
}

loop: for cond1 {
    for cond2 {
        break loop; // leaves both loops
    }
}
```

#### Continue statement
As in many programming languages, a `continue` statement starts the next iteration of a loop prematurely:
```
for cond {
    if get_foo() {
        continue;
    }
    fmt.println("Hellope");
}
```

#### Fallthrough statement
Odin's `switch` is like one in C or C++, except that Odin only runs the selected case. This means that a `break` statement is not needed at the end of each case. Another important difference is that the case values need not be integers nor constants.

`fallthrough` can be used to explicitly fall through into the next case block:
```
switch i {
case 0:
    foo();
    fallthrough;
case 1:
    bar();
}
```


## Procedures
In Odin, a procedure is something that can do work, which some languages call _functions_ or _methods_. A procedure literal in Odin is defined with the `proc` keyword:

```
fibonacci :: proc(n: int) -> int {
    switch {
    case n < 1:
        return 0;
    case n == 1:
        return 1;
    }
    return fibonacci(n-1) + fibonacci(n-2);
}

fmt.println(fibonacci(3));
```

### Parameters
Procedures can take zero or many parameters, the following example is basic procedure that multiplies two integers together:
```
multiply :: proc(x: int, y: int) -> int {
    return x * y;
}
fmt.println(multiply(137, 432));
```

When two or more consecutive parameters share a type, you can omit the other types from previous names, like with variable declarations. In this example: `x: int, y: int` can be shortened to `x, y: int`, for example:
```
multiply :: proc(x, y: int) -> int {
    return x * y;
}
fmt.println(multiply(137, 432));
```

Continuing the C family tradition, everything in Odin is passed by value. The procedure always gets a copy of the thing that has been passed, as if there was an assignment statement to the procedure parameter.

Passing a pointer value makes a copy of the pointer, not the data it points to it. Slices, dynamic arrays, and maps behave like pointers in this case (Internally they are structures that contain values, which include pointers and the "structure" is passed by value).

Parameters in a procedure body will be mutable but as they are copies, they will not affect the original values.

### Multiple results
A procedure in Odin can return any number of results. For example:
```
swap :: proc(x, y: int) -> (int, int) {
    return y, x;
}
a, b := swap(1, 2);
fmt.println(a, b); // 2 1
```

### Named results
Return values in Odin may be named. If so, they are treated as variables defined at the top of the procedure, like input parameters. A `return` statement without arguments returns the named return value. "Naked" return statements should only be used in short procedures as it reduces clarity when reading.

```
do_math :: proc(input: int) -> (x, y: int) {
    x = 2*input + 1;
    y = 3*input / 5;
    return x, y;
}
do_math_with_naked_return :: proc(input: int) -> (x, y: int) {
    x = 2*input + 1;
    y = 3*input / 5;
    return;
}
```

### Named arguments
When calling a procedures, it is not clear in which order parameters might appear. Therefore, the arguments can be named, like a struct literal, to make it clear which argument a parameter is for:
```
create_window :: proc(title: string, x, y: int, width, height: int, monitor: ^Monitor) -> (^Window, Window_Error) {...}

window, err := create_window(title="Hellope Title", monitor=nil, width=854, height=480, x=0, y=0);
```

**Note:** Currently, mixing named and non-named arguments is not allowed. This is subject to change if it is deemed necessary.


### Default values
The `create_window` procedure may be easier to use if default values are provided which will be used if they are not specified:
```
create_window :: proc(title: string, x := 0, y := 0, width := 854, height := 480, monitor: ^Monitor = nil) -> (^Window, Window_Error) {...}

window1, err1 := create_window("Title1");
window2, err2 := create_window(title="Title1", width=640, height=360);
```

**Note:** These default values must be constant or the `nil` value (if that type supports it).


### Explicit procedure overloading
Unlike other languages, Odin provides the ability to explicitly overload procedures:
```
bool_to_string :: proc(b: bool) -> string {...}
int_to_string  :: proc(i: int)  -> string {...}

to_string :: proc{bool_to_string, int_to_string};
```

#### Rationale behind explicit overloading

The design goals of Odin were explicitness and simplicity. Implicit procedure overloading complicates the scoping system. In C++, you cannot nest procedures within procedures so all procedure look-ups are done at the global scope. In Odin, procedures can be nested within procedures and as a result, determining which procedure should be used, in the case of implicit overloading, is complex.

Explicit overloading has many advantages:

* Explicitness of what is overloaded
* Able to refer to the specific procedure if needed
* Clear which scope the entity name belongs to

```
foo :: proc{
    foo_bar,
    foo_baz,
    foo_baz2,
    another_thing_entirely,
};
```


## Basic types
Odin's basic types are:
```
bool b8 b16 b32 b64

int  i8 i16 i32 i64 i128
uint u8 u16 u32 u64 u128 uintptr

// endian specific integers
i16le i32le i64le i128le u16le u32le u64le u128le // little endian
i16be i32be i64be i128be u16be u32be u64be u128be // big endian

f32 f64

complex64 complex128

rune // signed 32 bit integer
     // represents a Unicode code point
     // is a distinct type to `i32`

string cstring

rawptr

typeid
any
```

The `int`, `uint`, and `uintptr` types are pointer sized. When you need an integer value, you should default to using `int` unless you have a specific reason to use a sized or unsigned integer type

**Note:** The Odin `string` type stores the pointer to the data and the length of the string. `cstring` is used to interface with foreign libraries written in/for C that use zero-terminated strings.

### Zero values
Variables declared without an explicit initial value are given their _zero_ value.

The zero value is:

* `0` for numeric and rune types
* `false` for boolean types
* `""` (the empty string) for strings
* `nil` for pointer, typeid, and any types.


### Type conversion

The expression `T(v)` converts the value `v` to the type `T`.
```
i: int = 123;
f: f64 = f64(i);
u: u32 = u32(f);
```
or with type inference:
```
i := 123;
f := f64(i);
u := u32(f);
```

Unlike C, assignments between values of a different type require an explicit conversion.

#### Cast operator
The `cast` operator can also be used to do the same thing:
```
i := 123;
f := cast(f64)i;
u := cast(u32)f;
```
This is useful is some contexts but has the same semantic meaning.

#### Transmute operator
The `transmute` operator is a bit cast conversion between two types of the same size:
```
f := f32(123);
u := transmute(u32)f;
```
### Untyped types
In the Odin type system, certain expressions will have an "untyped" type. An untyped type can implicitly convert to a "typed" type. The following are the

### Auto cast operation
The `auto_cast` operator automatically casts an expression to the destination's type if possible:
```
x: f32 = 123;
y: int = auto_cast x;
```
**Note:** This operation is only recommended to be used for prototyping and quick tests. Please do not abuse it.

### Built-in constants and values
```
false // untyped boolean constant equivalent to the expression 0!=0
true  // untyped boolean constant equivalent to the expression 0==0
nil   // untyped nil value used for certain values
---   // untyped undefined value used to explicitly not initialize a variable
```

`---` is useful if you want to explicitly not initialize a variable with any default value:
```
x: int; // initialized with its zero value
y: int = ---; // uses uninitialized memory
```
This is the default behaviour in C.


### cstring type
The `cstring` type is a c-style string value, which is zero-terminated. It is equivalent to `char const *` in C. Its primary purpose is for easy interfacing with C. Please see the [foreign system](#foreign-system) for more information.

A `cstring` is easily convertable to an Odin `string` however, to convert a `string` to a `cstring` it requires allocations if the value is not constant.

```
str:  string  = "Hellope";
cstr: cstring = "Hellope"; // constant literal;
cstr2 := string(cstring);  // O(n) conversion as it requires search from the zero-terminator
nstr  := len(str);  // O(1)
ncstr := len(cstr); // O(n)
```

## Advanced types
### Type alias
You can alias a named type with another name:
```
My_Int :: int;
#assert(My_Int == int);
```

### Distinct types
A distinct type allows for the creation of a new type with the same underlying semantics.
```
My_Int :: distinct int;F
#assert(My_Int != int);
```

Aggregate types (struct, enum, union, bit_field) will always be distinct even when named.
```
Foo :: struct {};
#assert(Foo != struct{});
```


### Fixed arrays
An array is a simplified fixed length container. Each element in an array has the same type. An array's index can be any integer, character, or enumeration type.

An array can be constructed like the following:
```
x := [5]int{1, 2, 3, 4, 5};
for i in 0..4 {
    fmt.println(x[i]);
}
```
The notation `x[i]` is used to access the i-th element of `x`; and 0-index based (like C).

The built-in `len` proc returns the array's length.
```
x: [5]int;
#assert(len(x) == 5);
```

Array access is always bounds checked (at compile-time and at runtime). This can be disabled and enabled at a per block level with the `#no_bounds_check` and `#bounds_check` directives, respectively:

```
##no_bounds_check {
    x[n] = 123; // n could be in out of range of valid indices
}
```

`#no_bounds_check` can be used to improve performance when the bounds are known to not exceed.

#### Array programming
Odin's fixed length arrays support [array programming](https://en.wikipedia.org/wiki/Array_programming).

Example:
```
Vector3 :: [3]f32;
a := Vector3{1, 4, 9};
b := Vector3{2, 4, 8};
c := a + b;  // {3, 8, 17}
d := a * b;  // {2, 16, 72}
e := c != d; // true
```


### Slices

Slices look similar to arrays however, their length is not known at compile time. The type `[]T` is a slice with elements of type `T`. In practice, slices are much more common than arrays.

A slice is formed by specifying two indices, a low and high bound, separated by a colon:
```
a[low : high]
```

This selects a half-open range which includes the lower element, but excludes the higher element.

```
fibonaccis := [6]int{0, 1, 1, 2, 3, 5};
s: []int = fibonaccis[1:4]; // creates a slice which includes elements 1 through 3
fmt.println(s); // 1, 1, 2
```


Slices are like references to arrays; they do not store any data, rather describing a section, or slice, of an underyling data.

Internally, a slice stores a pointer to the data and an integer to store the length of the slice.

The built-in `len` proc returns the array's length.
```
x: []int = ...;
length_of_x := len(x);
```

#### Slice literals
A slice literal is like an array literal without the length.
This an array literal:
```
[3]int{1, 6, 3}
```
This is a slice literal which creates the same array as above, and then creates a slice that references it:
```
[]int{1, 6, 3}
```

#### Slice shorthand
For the array:
```
a: [6]int;
```
these slice expression are equivalent:
```
a[0:6]
a[:6]
a[0:]
a[:]
```

#### Nil slices
The zero value of a slice is `nil`. A nil slice has a length of 0 and has not underlying memory it points to. Slices can be compared again `nil` and nothing else.
```
s: []int;
if s == nil {
    fmt.println("s is nil!");
}
```



### Dynamic arrays
Dynamic arrays are similar to slices but their lengths may change during runtime. Dynamic arrays are resizeable and they are allocated using the current [context](#context-system)'s allocator.

```
x: [dynamic]int;
```

Along with the built-in proc `len`, dynamic arrays also have `cap` which can used to determine the dynamic arrays current underlying capacity.

#### Appending to a dynamic array

It is common to append new elements to a dynamic array; this can be done so with the built-in `append` proc.
```
x: [dynamic]int;
append(&x, 123);
append(&x, 4, 1, 74, 3); // append multiple values at once
```

#### Making and deleting slices and dynamic arrays
Slices and dynamic arrays can explicitly allocated with the built-in `make` proc.

```
a := make([]int, 6);           // len(a) == 6
b := make([dynamic]int, 6);    // len(b) == 6, cap(b) == 6
c := make([dynamic]int, 0, 6); // len(c) == 0, cap(c) == 6
```

Slices and dynamic arrays can be deleted with the built-in `delete` proc.

```
delete(a);
delete(b);
delete(c);
```

**Note:** Slices created with `make` must be deallocated with `delete`, whereas a slice literal does not need to be deleted since it is just a slice of an underlying array.

**Note:** There is not automatic memory management in Odin. Slices may not be allocated using an [allocator](#allocators).

### Enumerations
Enumeration types define a new type whose values consist of the ones specified. The values are ordered, for example:
```
Direction :: enum{North, East, South, West};
```
The following holds:
```
int(Direction.North) == 0
int(Direction.East)  == 1
int(Direction.South) == 2
int(Direction.West)  == 3
```
Enum fields can be assigned an explicit value:
```
Foo :: enum {
    A,
    B = 4, // Holes are valid
    C = 7,
    D = 1337,
}
```

If an enumeration requires a specific size, a backing integer type can be specified. By default, `int` is used as the backing type for an enumeration.
```
Foo :: enum u8 {A, B, C}; // Foo will only be 8 bits
```

Odin supports implicit selector expressions for enums:
```
Foo :: enum {A, B, C};

f: Foo;
f = .A;

BAR :: bit_set[Foo]{.B, .C};

switch f {
case .A:
    fmt.println("foo");
case .B:
    fmt.println("bar");
case .C:
    fmt.println("baz");
}
```

[`using`](#using-statement) can also be used with an enumeration to bring the fields into the current scope:
```
main :: proc() {
    Foo :: enum {A, B, C};
    using Foo;
    a := A;

    using Bar :: enum {X, Y, Z};
    x := X;
}
```

#### Implicit Selector Expression

An *implicit selector expression* is an abbreviated way to access a member of an enumeration, in a context where type inference can determine the implied type. It has the following form:

```
.member_name
```
For example:
```
Direction :: enum{North, East, South, West};
d: Direction;
d = Direction.East;
d = .East;
```

**Note:** This is preferred to [`using`](#using-statement) an enumeration as it does pollute the current scope.


### Bit sets
The `bit_set` type models the mathematical notion of a set. A bit_set's element type can be either an enumeration or a range:
```
Direction :: enum{North, East, South, West};

Direction_Set :: bit_set[Direction];

Char_Set :: bit_set['A'..'Z'];

Number_Set :: bit_set[0..<10]; // bit_set[0..9]
```

Bit sets are implemented as bit vectors internally for high performance. The zero value of a bit set is either `nil` or `{}`.

```
x: Char_Set;
x = {'A', 'B', 'Y'};
y: Direction_Set;
y = {.North, .West};
```

Bit sets support the following operations:
* `A | B` - union of two sets
* `A & B` - intersection of two sets
* `A &~ B` - difference of two sets (A without B's elements)
* `A ~ B` - symmetric difference (Elements that are in A and B but not both)
* `A == B` - set equality
* `A != B` - set inequality
* `e in A` - set membership (A contains element e)
* `e notin A` - A does not contain element e
* `incl(&A, elem)` - same as `A |= {elem};`
* `excl(&A, elem)` - same as `A &~= {elem};`


Bit sets are often used to denote flags. This is much cleaner than defining integer constants that need to be bitwise or-ed together.

If a bit set requires a specific size, the underlying integer type can be specified:
```
Char_Set :: bit_set['A'..'Z'; u64];
#assert(size_of(Char_Set) == size_of(u64));
```



### Pointers
Odin has pointers. A pointer is an memory address of a value. The type `^T` is a pointer to a `T` value. Its zero value is `nil`.
```
p: ^int;
```
The `&` operator takes the address to its operand (if possible):
```
i := 123;
p := &i;
```

The `^` operator dereferences the pointer's underlying value:
```
fmt.println(p^); // read  i through the pointer p
p^ = 1337;       // write i through the pointer p
```

**Note:** C programmers may be used to using `*` to denote pointers. In Odin, the `^` syntax is borrowed from Pascal. This is to keep the convention of the type on the left and its usage on the right:
```
p: ^int; // ^ on the left
x := p^; // ^ on the right
```

**Note:** Unlike C, Odin has no pointer arithmetic. If you need a form of pointer arithmetic, please use the `ptr_offset` and `ptr_sub` procedures in the `"core:mem"` package.

### Structs
A `struct` is a record type in Odin. It is a collection of fields. Struct fields are accessed by using a dot:
```
Vector2 :: struct {
    x: f32,
    y: f32,
}
v := Vector2{1, 2};
v.x = 4;
fmt.println(v.x);
```

Struct fields can be accessed through a struct pointer:
```
v := Vector2{1, 2};
p := &v;
p.x = 1335;
fmt.println(v);
```

We could write `p^.x`, however, it is to nice abstract the ability to not explicitly dereference the pointer. This is very useful when refactoring code to use a pointer rather than a value, and vice versa.

#### Struct literals
A struct literal can be denoted by providing the struct's type followed by `{}`. A struct literal must either provide all the arguments or none:
```
Vector3 :: struct {
    x, y, z: f32,
}
v: Vector3;
v = Vector3{}; // Zero value
v = Vector3{1, 4, 9};
```
You can list just a subset of the fields if you specify the field by name (the order of the named fields does not matter):
```
v := Vector3{z=1, y=2};
assert(v.x == 0);
assert(v.y == 2);
assert(v.z == 1);
```

#### Struct tags
Structs can tagged with different memory layout and alignment requirements:
```
struct #align 4 {...} // align to 4 bytes
struct #packed {...} // remove padding between fields
struct #raw_union {...} // all fields share the same offset (0). This is the same as C's union
```

### Unions
A `union` in Odin is a discriminated union, also known as a tagged union or sum type. The zero value of a union is `nil`.
```
Value :: union {
    bool,
    i32,
    f32,
    string,
}
v: Value;
v = "Hellope";

// type assert that `v` is a `string` and panic otherwise
s1 := v.(string);

// type assert but with an explicit boolean check. This will not panic
s2, ok := v.(string);
```

#### Type switch statement
A type switch is a construct that allows several type assertions in series. A type switch is like a regular switch statement, but the cases are types (not values). For a union, the only case types allowed are that of the union.
```
value: Value = ...;
switch v in value {
case string:
    #assert(type_of(v) == string);

case bool:
    #assert(type_of(v) == bool);

case i32, f32:
    // This case allows for multiple types, therefore we cannot know which type to use
    // `v` remains the original union value.
    #assert(type_of(v) == Value);
case:
    // Default case
    // In this case, it is `nil`
}
```

### Maps
A `map` maps keys to values. The zero value of a map is `nil`. A `nil` map has no keys. The built-in `make` proc returns an initialzed map using the current [context](#context-system), and `delete` can be used to delete a map.

```
m := make(map[string]int);
defer delete(m);
m["Bob"] = 2;
fmt.println(m["Bob"]);
```

To insert or update an element of a map:
```
m[key] = elem;
```
To retrieve an element:
```
elem = m[key];
```

To remove an element:
```
delete_key(&m, key);
```

If an element of a key does not exist, the zero value of the element will be returned. To check to see if an element exists can be done in two ways:
```
elem, ok := m[key]; // `ok` is true if the element for that key exists
```
or
```
ok := key in m; // `ok` is true if the element for that key exists
```

The first approach is called the "comma ok idiom".

### Procedure type
A procedure type is internally a pointer to a procedure in memory. `nil` is the zero value a procedure type.

Examples:
```
proc(x: int) -> bool
proc(c: proc(x: int) -> bool) - (i32, f32)
```

#### Calling conventions
Odin supports the following calling conventions:

* **odin** - default convention used for an Odin **proc**. It is the same **cdecl** but passes an implicit `context` pointer on each call. (**Note:** This is subject to change)
* **contextless** - This is the same as **odin** but without the implicit `context` pointer.
* **stdcall** or **std** -- This is the stdcall convention as specified by Microsoft.
* **cdecl** or **c** -- This is the default calling convention generated of a procedure in C.
* **fastcall** or **fast** - This is a compiler dependent calling convention.
* **none** - This is a compiler dependent calling convention which will do nothing to parameters

Most calling conventions exist only to interface with foreign windows code.

The default calling convention is **odin**, unless it is within a `foreign` block which it is then **cdecl**.

A procedure type with a different calling convention can be declared like the following:
```
proc "c" (n: i32, data: rawptr)
proc "contextless" (s: []int)
```

Procedure types are only compatible with the procedures that have the same calling convention and parameter types.

### Bit fields
A `bit_field` is a record type which allows you to define named integer fields with specific bit widths:
```
Foo :: bit_field {
    width_validated: 1,
    height_validated: 1,
    age: 3,
}
```
This data struct will be 1 byte wide even though it requires only 5 bits of information.
The memory layout of a `bit_field` will directly map to an integer:
```
width_validate == (x>>0) & 0b1
height_validated == (x>>1) & 0b1
age == (x>>2) & 0b111
```


### 'typeid' type
A `typeid` is a unique identifier for an Odin type. This construct is used by the `any` type to denote what the underlying data's type is.
```
a := typeid_of(bool);
i: int = 123;
b := typeid_of(type_of(i));
```

A `typeid` can be mapped to relevant type information which can be used in applications such as printing types and editing data:
```
import "core:runtime"

main :: proc() {
    u := u8(123);
    id := typeid_of(type_of(u));
    info: ^runtime.Type_Info;
    info = type_info_of(id);
}
```


### 'any' Type
An `any` type can reference any data type. Internally it contains a pointer to the underlying data and its relevant `typeid`. This is a very useful construct in order to have a runtime type safe printing procedure.

**Note:** The `any` value is only valid for as long as the underlying data is still valid. Passing a literal to an `any` will allocate the literal in the current stack frame.

**Note:** It is highly recommended that you do not use this unless you know what you are doing. Its primary use is for printing procedures.


## Using statement
`using` can used to bring entities declared in a scope/namespace into the current scope. This can be applied to import declarations, import names, struct fields, procedure fields, and struct values.
```
// imports all the exported entities from the `foo` package into this file scope
using import "foo"
```

```
import "foo"
bar :: proc() {
    // imports all the exported entities from the `foo` package into this scope
    using foo;
}
```

**Note:** It is not advised that to do `using import` as it pollutes the file's scope.

### Using statement with structs
Let's take a very simple entity struct:
```
Vector3 :: struct{x, y, z: f32};
Quaternion :: struct{x, y, z, w: f32};
Entity :: struct {
    position: Vector3,
    orientation: Quaternion,
}
```
It can used like this:
```
foo :: proc(entity: ^Entity) {
    fmt.println(entity.position.x, entity.position.y, entity.position.z);
}
```

The entity members can be brought into the procedure scope by `using` it:
```
foo :: proc(entity: ^Entity) {
    using entity;
    fmt.println(position.x, position.y, position.z);
}
```
The `using` can be applied to the parameter directly:
```
foo :: proc(using entity: ^Entity) {
    fmt.println(position.x, position.y, position.z);
}
```
It can also be applied to sub-fields:
```
foo :: proc(entity: ^Entity) {
    using entity.position;
    fmt.println(x, y, z);
}
```

We can also apply the `using` statement to the struct fields directly, making all the fields of `position` appear as if they on `Entity` itself:
```
Entity :: struct {
    using position: Vector3,
    orientation: Quaternion,
}
foo :: proc(entity: ^Entity) {
    fmt.println(entity.x, entity.y, entity.z);
}
```

### Subtype polymorphism
It is possible to get subtype polymorphism, similar to inheritance-like functionality in C++, but without the requirement of vtables or unknown struct layout:
```
Frog :: struct {
    ribbit_volume: f32,
    using entity: Entity,
    colour: Colour,
}

frog: Frog;
// Both work
foo(frog);
frog.x = 123;
```

**Note:** `using` can be applied to arbitrarily many things, which allows the ability to have multiple subtype polymorphism (but also its issues).

**Note:** `using`'d fields can still be referred by name.


## Implicit context system
TODO
### Allocators
TODO

## Foreign system
It is sometimes necessarily to interface with foreign code, such as a C library. In Odin, this is achieved through the `foreign` system. You can "import" a library into the code using the same semantics as a normal import declaration:
```
foreign import kernel32 "system:kernel32.lib"
```
This `foreign import` declaration will create a "foreign import name" which can then be used to associate entities within a foreign block.

```
foreign import kernel32 "system:kernel32.lib"
foreign kernel32 {
    ExitProcess :: proc "stdcall" (exit_code:  u32) ---;
}
```

Foreign procedure declarations have the **cdecl**/**c** calling convention by default unless specified otherwise. Due to foreign procedures do not have a body declared within this code, you need append the `---` symbol to the end to distinguish it as a procedure literal without a body and not a procedure type.

The attributes system can be used to change specific properties of entities declared within a block:
```
@(default_calling_convention = "std")
foreign kernel32 {
    @(link_name="GetLastError") get_last_error :: proc() -> i32 ---;
}
```

Available attributes for foreign blocks:
```
default_calling_convention=<string>
    default calling convention for procedures declared within this foreign block
link_prefix=<string>
    prefix that needs to be appended to the linkage names of the entities except where the link name has been explicitly overridden
```


## Parametric polymorphism
Parametric polymorphism, commonly referred to as "generics", allow the user to create a procedure or data that can be written _generically_ so it can handle values in the same manner.

Note: Within the Odin code base and documentation, the nickname "parapoly" is usually used.

### Explicit parametric polymorphism
Explicit parametric polymorphism means that the types of the parameters must be explicitly provided.
#### Procedures
To specify that a parameter is "constant", the parameters name must be prefixed with a dollar sign `$`. The following example takes two constant parameters to initialize an array of known length:
```
make_f32_array :: inline proc($N: int, $val: f32) -> (res: [N]f32) {
    for _, i in res {
        res[i] = val*val;
    }
    return;
}

array := make_f32_array(3, 2);
```

Types can also be explicitly passed with specifying that the `typeid` parameter is constant:
```
my_new :: proc($T: typeid) -> ^T {
    return (^T)(alloc(size_of(T), align_of(T)));
}

ptr := my_new(int);
```

#### Data types
Structures and union may have polymorphic parameters. The `$` prefix is optional for record data types as all parameters must be "constant".
Parapoly struct:
```
Table_Slot :: struct(Key, Value: typeid) {
    occupied: bool,
    hash:    u32,
    key:     Key,
    value:   Value,
}
slot: Table_Slot(string, int);
```
Parapoly union:
```
Error :: enum {Foo0, Foo1, Foo2};
Param_Union :: union(T: typeid) {T, Error};
r: Param_Union(int);
r = 123;
r = Error.Foo0;
```

### Implicit parametric polymorphism
Implicit implies that the type of parameter is inferred from its input. In this case, the dollar sign `$` can be placed on the type.

Note: Within the Odin code base and documentation, the name "polymorphic name" is usually used.

#### Procedures
```
foo :: proc($N: $I, $T: typeid) -> (res: [N]T) {
	// `N` is the constant value passed
	// `I` is the type of N
	// `T` is the type passed
	fmt.printf("Generating an array of type %v from the value %v of type %v\n",
	           typeid_of(type_of(res)), N, typeid_of(I));
	for i in 0..<N {
		res[i] = i*i;
	}
	return;
}

T :: int;
array := foo(4, T);
for v, i in array {
	assert(v == T(i*i));
}
```

#### Specialization
In some cases, you may want to specify that a type must be a specialization of a certain type.
```
// Only allow types that are specializations of a (polymorphic) slice
make_slice :: proc($T: typeid/[]$E, len: int) -> T {
	return make(T, len);
}
```
```
Table_Slot :: struct(Key, Value: typeid) {
	occupied: bool,
	hash:     u32,
	key:      Key,
	value:    Value,
}
Table :: struct(Key, Value: typeid) {
	count:     int,
	allocator: mem.Allocator,
	slots:     []Table_Slot(Key, Value),
}

// Only allow types that are specializations of `Table`
allocate :: proc(table: ^$T/Table, capacity: int) {
	...
}

// find :: proc(table: ^$T/Table, key: T.Key) -> (T.Value, bool) {
find :: proc(table: ^Table($Key, $Value), key: Key) -> (Value, bool) {
	...
}
```

## Useful idioms

### Basic idioms

#### If-statements with initialization
```
if str, ok := value.(string); ok {
    ...
} else {
   ...
}
```

#### Iterating through slices of structs by value or by reference
```
Foo :: struct {
    f: float,
    i: int,
}

foos := make([]Foo, num);

// By-value basic ranged-based loop, with implicit indexing
for v, j in foos {
    using v;
    fmt.println(i, v, f, i);
}

// Alternative range-based loop, with explicit indexing
for _, j in foos {
    using foo := foos[j]; // copy
    fmt.println(j, foo, f, i);
}

// By-reference range-based explicit indexing loop
for _, j in foos {
    using foo := &foos[j]; // "reference", changes to `f` or `i` are visible outside this scope
    fmt.println(j, foo, f, i);
}
```

#### 'defer if'
```
defer if cond {
}
```

### Advanced idioms

Subtype polymorphism with runtime type safe down casting:
```
Entity :: struct {
	id:   u64,
	name: string,

	derived: any,
}

Frog :: struct {
	using entity: Entity,
	volume: f32,
	jump_height: i32,
}


new_entity :: proc($T: typeid) -> ^T {
	e := new(T);
	e.derived = e^;
	return e;
}

entity: ^Entity = new_entity(Frog);
switch e in entity.derived {
case Frog:
	fmt.println("Ribbit:", e.volume);
}
```

## Extra information
More details can be found on the [Github wiki for Odin](https://github.com/odin-lang/Odin/wiki).
Some of this information includes:
- [Compiler Flags](https://github.com/odin-lang/Odin/wiki/Compiler-Flags)
- [Attributes and Tags](https://github.com/odin-lang/Odin/wiki/Attributes-and-Tags)
- [Built in Procedures](https://github.com/odin-lang/Odin/wiki/Built-in-Procedures)
- [Keywords and Operators](https://github.com/odin-lang/Odin/wiki/Keywords-and-Operators)
- ...and more
