- # Leetcode Snippets
	- `String` to `Vec<u8>`
		- ```rust
		  s.as_bytes().to_vec()
		  ```
	- `Vec<u8>` to `String` by utf8
		- ```rust
		  String::from_utf8(v).unwrap()
		  ```
	- Enumerate
		- ```rust
		  for (idx, num) in nums.iter().enumerate() {
		  }
		  ```
- # Lifetime
	- To check if reference (borrow) is dangling or not
		- ```rust
		  fn main() {
		      let r;
		      {
		          let x = 5;
		          r = &x;
		      }
		      println!("r: {}", r);
		  }
		  ```
			- Compile error:
				- ```
				    |
				  5 |         r = &x;
				    |             ^^ borrowed value does not live long enough
				  6 |     }
				    |     - `x` dropped here while still borrowed
				  7 |     println!("r: {}", r);
				    |                       - borrow later used here
				  ```
				-
	- Example1: Last Referencing Point
		- ```rust
		  fn main() {
		     let mut s = 1;
		  
		      let r1 = &s;
		      println!("{}", r1);
		      // r1's lifetime end here (last referencing point)
		  
		      let r2 = &mut s;
		      println!("{}", r2);
		  } // r2's lifetime end here (last referencing point)
		  ```
			- Compile ok!
	- Example2: One writer nand multiple readers
		- ```rust
		  fn main() {
		      let mut s = 1;
		  
		      let r1 = &s;
		      let r2 = &mut s;
		      
		      println!("{} {}", r1, r2);
		  } // r1, r2's lifetime ends here
		  ```
			- Compile error msg:
				- ```
				    |
				  4 |     let r1 = &s;
				    |              -- immutable borrow occurs here
				  5 |     let r2 = &mut s;
				    |              ^^^^^^ mutable borrow occurs here
				  6 |     
				  7 |     println!("{} {}", r1, r2);
				    |                       -- immutable borrow later used here
				  ```
		- ```rust
		  fn main() {
		      let mut w = vec![1, 2, 3];
		      w[w.len() - 1] = 2;
		      print!("{:?}\n", w);
		  }
		  ```
			- ```
			  3 |     w[w.len() - 1] = 2;
			    |     --^^^^^^^-----
			    |     | |
			    |     | immutable borrow occurs here
			    |     mutable borrow occurs here
			    |     mutable borrow later used here
			  ```
		- ```rust
		  fn main() {
		      let mut w = vec![];
		      w.push(w.len());
		      print!("{:?}", w);
		  }
		  ```
			- rust: ok
			- TODO why diff
	- Example3: Longest string
		- ```rust
		  fn main() {
		      let string1 = String::from("abcd");
		      let string2 = "xyz";
		  
		      let result = longest(string1.as_str(), string2);
		      println!("The longest string is {}", result);
		  }
		  
		  fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
		      if x.len() > y.len() {
		          x
		      } else {
		          y
		      }
		  }
		  ```
			- The lifetime of `result` will be `string2`'s lifetime
			- It will return smallest lifetime ($$\cap L $$)
	- Static Lifetime
		- ```rust
		  let s: &'static str = "hello world";
		  ```
		- The memory occupy by string live as long as process.
		- The reference is only live in its range.