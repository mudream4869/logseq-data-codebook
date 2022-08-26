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
- ## Get RSS
	- ```go
	  func getRss() (int64, error) {
	  	buf, err := ioutil.ReadFile("/proc/self/statm")
	  	if err != nil {
	  		return 0, err
	  	}
	  
	  	fields := strings.Split(string(buf), " ")
	  	if len(fields) < 2 {
	  		return 0, errors.New("Cannot parse statm")
	  	}
	  
	  	rss, err := strconv.ParseInt(fields[1], 10, 64)
	  	if err != nil {
	  		return 0, err
	  	}
	  
	  	return rss * int64(os.Getpagesize()), nil
	  }
	  ```
-