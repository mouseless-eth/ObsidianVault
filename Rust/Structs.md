# Chapter 5 : Enums and Structs
*Different ways to declare structs*
```rust
struct User {
	username: String,
	email: String,
	sign_in_count: u64,
	active: bool,
};

fn main() {
	let mut user1 = User {
		email: String::from("throwaway@gmail.com"),
		username: String::from("foo"),
		sign_in_count: 1,
		active: true,
	};

	let name = user1.username;
	user1.username = String::from("bar");

	let user2 = build_user(
		String::from("foobar@gmail"), 
		String::from("foobar")
	);

	let user3 = User {
		email: String::from("foobar@gmail"),
		username: String::from("foobar"),
		..user2
	};
}

fn build_user(email: String, username: String) -> User{
	User {
		email,
		username,
		active: true,
		sign_in_count: 1,
	}
}
```

*Tuple Structs*
```rust
fn main() {
	struct Color(i32, i32, i32);
	struct Point(i32, i32, i32);
}
```