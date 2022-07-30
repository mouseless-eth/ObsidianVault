# Ownership 
### Three Rules About Ownership
1) Each value in Rust has a variable that's called its owner
2) There can only be one owner at a time
3) When the owner goes out of scope, the value will be dropped

## Ownership and memory allocation
```rust
fn main() {
	let x: i32 = 5;
	let y: i32 = x; // value of x is copied to y

	let s1: String = String::from("hello");
	let s2: String = s1; // move (not a shallow copy)

	println!("{}, world", s1); // Throws error because s1 was moved to s2


	let s3 = s2.clone(); // s2 is valid because it isn't moved
	println!("{}, world", s2); 
}
```

## Ownership and functions
*Passing variable into function moves it and invalidates it from being used as the function takes ownership over it's parameters*
```rust
fn main() {
	let s: String = String::from("hello");
	takes_ownership(s);
	println!("{}", s); // Throws an error because s is moved into function
}

fn takes_ownership(some_string: String) {
	println("{}", some_string);
}
```

*Integers are copied instead of moved so printing x won't throw an error*
```rust
fn main() {
	let x: i32 = 5;
	makes_copy(x);
	println!("{}", x); 
}

fn makes_copy(some_integer: i32) {
	println("{}", some_integer);
}
```

*Returning a string from a function moves ownership of string to whatever variable captures the return (s1 in this case)*
```rust
fn main() {
	let s1: String = gives_ownership;
	let s3: String = takes_and_gives_back(s2);
	println!("s1 = {}, s3 = {}", s1, s3); 
}

fn gives_ownership() -> String{
	let some_string: String = String::from("hello");
	some_string
}

fn takes_and_gives_back(a_string: String) -> String {
	a_string
}
```

## References and borrowing
```rust
fn main() {
	let s1: String = String::from("hello");
	let len = calculate_length(&s1); // passing reference to function
	println!("The length of '{}' is {}.", s1, len); 
}

// Function takes in a reference to a string (as seen by &)
fn calculate_length(s: &String) -> usize {
	let length: usize = s.len();
	length
}
```
- We can pass refences by using the '&' sign
- References are immutable by default (we cannot change it's value)

*Modifying a reference*
```rust
fn main() {
	let mut s1: String = String::from("hello");
	change(&mut s1);
}

// Function takes in a reference to a string (as seen by &)
fn calculate_length(some_string: &mut String) -> usize {
	some_string.push_str(", world");
}
```
- Make sure variable that you are passing is mutable
- Pass a mutable reference to the function
- Make sure function accepts a mutable reference

`&` = Immutable reference
`&mut` = mutable reference

### Mutable reference restrictions
- You can only have one mutable reference to a particular piece of data in a particular scope
```rust
fn main() {
	let mut s: String = String::from("hello");
	let r1: &mut String = &mut s;
	let r2: &mut String = &mut s; // This line breaks this rule
}

```
- You cannot have an immutable reference if an immutable reference already exists
	- This is because, immutable variables don't expect the variable to change
```rust
fn main() {
	let mut s: String = String::from("hello");

	let r1: &String = &s;
	let r2: &String = &s; 
	let r3: &mut String = &mut s; // This line breaks this rule
}
```
- You can have multiple immutable references
```rust
fn main() {
	let s: String = String::from("hello");

	// This is allowed
	let r1: &String = &s;
	let r2: &String = &s; 
	let r2: &String = &s; 
}
```

*This is allowed because the immutable variables have gone out of scope*
```rust
fn main() {
	let mut s: String = String::from("hello");

	let r1: &String = &s;
	let r2: &String = &s; 

	println!("{}, {}", r1, r2); // r1 and r2 go out of scope here

	let r3: &mut String = &mut s; 
	println!("{}", r3);
}
```

##### Dangling references
```rust
fn main() {
	// Throws an error because
	let reference_to_nothing: &String = dangle();
}

fn dangle() -> &String { // ERR is thrown here
	let s: String = String::from("hello");
	&s // s goes out of scope here so the return variable points to nothing
}
```

## Rules of references
1) At any given time, you can have either one mutable reference or any number of immutable references
2) References must always be valid
