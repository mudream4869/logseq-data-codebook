title:: Algorithm/Lock/CAS
todo:: true

- Lock Free
- CAS = **C**ompare **A**nd **S**wap
- # Pseudo Code
	- Since **CAS** operation need to be atomic, we need hardware support.
	- ```cpp
	  bool CAS(int* p, int oldValue, int newValue) {
	      if (*p != oldValue) {
	          // someone modified after getting value
	          return false;
	      }
	      // ok no one modified after getting value, go!
	      *p = newValue;
	      return true;
	  }
	  ```
- # Use Case
	- Swap new value, fail then retry.
	- ```cpp
	  int oldValue, newValue;
	  do {
	      oldValue = p;
	      // something not atomic
	      newValue = NEW_VALUE;
	  } while(!CAS(&p, oldValue, newValue)); // Fail? Retry!
	  ```