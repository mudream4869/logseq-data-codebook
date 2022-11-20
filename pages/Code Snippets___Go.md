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
- ## [[Memory Usage]]
	- ```go
	  type MemoryInfo struct {
	  	RSS     float64
	  	Shared  float64
	  	Private float64
	  }
	  
	  func GetMomoryInfo() (*MemoryInfo, error) {
	  	buf, err := ioutil.ReadFile("/proc/self/statm")
	  	if err != nil {
	  		return nil, fmt.Errorf("read statm: %w", err)
	  	}
	  
	  	fields := strings.Split(string(buf), " ")
	  	if len(fields) < 3 {
	  		return nil, errors.New("cannot parse statm")
	  	}
	  
	  	rss, err := strconv.ParseInt(fields[1], 10, 64)
	  	if err != nil {
	  		return nil, fmt.Errorf("parse rss: %w", err)
	  	}
	  
	  	shared, err := strconv.ParseInt(fields[2], 10, 64)
	  	if err != nil {
	  		return nil, fmt.Errorf("parse shared: %w", err)
	  	}
	  
	  	pageSizeKB := float64(os.Getpagesize()) / 1024
	  
	  	return &MemoryInfo{
	  		RSS:     float64(rss) * pageSizeKB,
	  		Shared:  float64(shared) * pageSizeKB,
	  		Private: float64(rss-shared) * pageSizeKB,
	  	}, nil
	  }
	  ```
-