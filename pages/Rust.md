- # Leetcode Snippets
- # Lifetime
	- To check if reference (borrow) is dangling or not
	  collapsed:: true
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
	  collapsed:: true
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
	  collapsed:: true
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