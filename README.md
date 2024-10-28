# Syntax

## Variables

```
let [mut] myVar: i32 = 0;
```

### Types

Integers:
	i8, i16, i32, i64, i128
Unsigned integers:
	u8, u16, u32, u64, u128
Floats:
	f32, f64, f128
Others
	bool, char, str

### Arrays / Maps

```
let arr: chr[12] = [1, 2, 3, ...];
let map: {chr: bool}[2] = {
	'a': false,
	'A': true
};
```

### Default values

```
let mut var; chr; // ''
let mut var: i32; // 0
let mut var: bool; // false
let mut var: chr[3]; // ['', '', '']
let mut var: {chr: bool}[2]; // Error: no value provided in impossiblity to use default value (duplicate keys)
```

### Casting

```
let mut var: bool = bool(5); // true
```

### Mutable Values

Non mutated -> Error

### Unused Values
-> Error

### Constant Expressions

To make a constant into a constant expression, use this syntax.

```
[cxpr] let myVar: i32 = 12;
```

## Conditions

### If / Else If / Else

```
use io;

let mut num: i32 = 10;

if(num < 10) {
	io::println("Value under 10");
} else if(num > 10) {
	io::println("Value over 10");
} else {
	io::println("Value is 10");
}
```

### Match

```
use io;

let arr: i8[2] = [1, 2];

match arr {
    [_, 2] -> {
        io::println("[n, 2]");
    }
    [3, val] -> {
        io::println(3, val);
    }
    val -> {
        io::println("Default value ", val);
    }
}
```

## Loops

```
use io;

for(let mut i: i8, i < 10; i++) {
	io::println("Index #", i);
}
let arr: i8[3] = [2, 4, 6];
for(let val: i8 iter arr) {
	io::println("Element: ", val);
}
```

## Pipes

```
use io;
use str;

"Hello World Test"
=> str.split(_, ' ') // Uses last value as "-"
=> _.remove(1) // Same as before
=> _[0] + " World";
=> io::println(); // Automatically adds "_" as argument for io::prinln()
```

## Templates

### Base

```
fn |type| add(a: type, b: type): type {}
```

### Templates Overload

```
fn |type| add(a: type, b: type): type {}

fn add(a: i8, b: i8): i8 {
	return a + b;
}
```

### Template Inference

```
fn |type| test(val: type) {}

test("abc"); // Equivalent to |str| test("abc");
```

## Errors

```
fn add(a: i32, b: i32): i32 | RangeError {
	if(a + b > I32_MAX) {
		trigger RangeError("Result value out of range for i32 type");
	}
	return a + b;
}
```

When getting an error, you can either catch it or bubble it up to the parent function.

```
import io;

fn catch_error(void): void {
	let result: i32 = catch add(12, 34) {
		err: RangeError -> {
			io::printerr(err);
		}
		err: OtherError -> {
			// Error: OtherError type cannot be produced by "add(i32, i32): i32 | RangeError"
		}
	}
}

fn bubble_error(void): void | RangeError { 
	// If not "| RangeError": Error: Missing error return type "RangeError"
	
	let result: i32 = bubble add(1, 2); // Bubbles it up to parent function
}

fn not_handled_error(void): void {
	let result: i32 = add(123, 456); // Error: Non-handled error type RangeError
}
```

If `bubble` from `main`, popup "The developer of this app is a skill issue"

## Parallel Code

```

fn sum(val: i8[4]): i8 {
	return val[0] + val[1] + val[2] + val[3];
}

fn main(void): void {

	let arr: i8[4][5] = [
		[1, 2, 3, 4],
		[5, 6, 7, 8],
		[9, 0, 1, 2],
		[3, 4, 5, 6],
		[7, 8, 9, 0]
	];

	let res: i8[5] = parallel arr -> element: i8[4] {
		yield sum(element);
	}.pack();
}
```