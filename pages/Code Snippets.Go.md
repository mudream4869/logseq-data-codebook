public:: true
title:: Code Snippets/Go

- ## Stdin
	- ```cpp
	  scanner := bufio.NewScanner(os.Stdin)
	  for {
	  	scanner.Scan()
	  	text := scanner.Text()
	  	if len(text) == 0 {
	  		break
	  	}
	  	// text ...
	  }
	  ```
-