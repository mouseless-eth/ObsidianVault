# Module System
Modules help organise code and ecapsulates code modules.

**Packages store Crates**
**Crates contain Modules**
Modules control a chunk of code and allow you to define the privacy rules

###### 2 Types of Crates
- Binary Crate (code that you can execute)
- Library Crate (code to be used by other programs)

#### Creating a new package
```bash
cargo new my-project
```

`main.rs` is the crate root (source file that rust compiler starts at)

if `lib.rs` is defined, then the crate will be treated as a Library Crate

- Package must have atleast one crate
- Package can have either zero library crate or one library crate
- Package can have as many binary crates

Binary Crates live in `bin` directory

### Modules
We can create a Library Crate using the `--lib` flag
```bash
cargo new --lib restaurant
```

**Defining Modules**
```rust
mod front_of_house {
	mod hosting {
		fn add_to_waitlist() {}
		fn seat_at_table() {}
	}
	mod serving {
		fn take_order() {}
		fn server_order() {}
		fn take_payment() {}
	}
}
```
- By default parent modules cannot see child modules
- By default child modules can see parent modules

- If a struct if public, by default all of it's fields will be private
- To use a field outside of a module, both the struct and field needs to be made public

**Using Modules**
```rust
mod front_of_house {
	pub mod hosting {
		pub fn add_to_waitlist() {}
	}
}

pub fn eat_at_restraunt() {
	// Absolute path
	crate::front_of_house::hosting::add_to_waitlist();

	// Relative path
	front_of_house::hosting::add_to_waitlist();
}
```

**Relative Paths Using Super Keyword**
```rust
fn server_order() {}

mod back_of_house {
	fn fix_incorrect_order() {
		cook_order();
		super::server_order();
	}

	fn cook_order() {}
}
```

### Use Keyword
```rust
mod front_of_house {
	pub mod hosting {
		pub fn add_to_waitlist() {}
	}
}

// Absolute path
use crate::front_of_house::hosting;

pub fn eat_at_restraunt() {
	hosting::add_to_waitlist();
	hosting::add_to_waitlist();
	hosting::add_to_waitlist();
}

// Relative path
use self::front_of_house::hosting;

pub fn eat_at_restraunt() {
	hosting::add_to_waitlist();
	hosting::add_to_waitlist();
	hosting::add_to_waitlist();
}

// Adding add_to_waitlist to scope
use self::front_of_housing::add_to_waitlist;

pub fn eat_at_restraunt() {
	add_to_waitlist();
	add_to_waitlist();
	add_to_waitlist();
}

```

**Nested Paths**
```rust
use rand::{Rng, CryptoRng, ErrorKind::Transient};

use std::io::{self, Write};

// glob operator
use std::io::*;
```