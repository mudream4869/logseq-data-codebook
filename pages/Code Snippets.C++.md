public:: true
title:: Code Snippets/C++

- ## Memory Usage
	- https://stackoverflow.com/questions/669438/how-to-get-memory-usage-at-runtime-using-c
	- ```cpp
	  struct MemoryInfo {
	      // kb
	      double rss;
	      double sharedMemory;
	      double privateMemory;
	  };
	  
	  MemoryInfo getMemoryInfo() {
	      int tSize = 0, resident = 0, share = 0;
	      std::ifstream buffer("/proc/self/statm");
	      buffer >> tSize >> resident >> share;
	      buffer.close();
	  
	      long pageSizeKB = sysconf(_SC_PAGE_SIZE) / 1024;
	      double rss = resident * pageSizeKB;
	      double sharedMem = share * pageSizeKB;
	      return MemoryInfo{
	          .rss = rss,
	          .sharedMemory = sharedMem,
	          .privateMemory = rss - sharedMem,
	      };
	  }
	  ```
- ## Disk Usage
	- ```cpp
	  // just run exec("du -s")
	  uint64_t getDiskUsage(const std::string& path) {
	      uint64_t usedKB;
	      std::ifstream buffer("du -s " + path);
	      buffer >> usedKB;
	      buffer.close();
	      return usedKB;
	  }
	  ```
- ## Stdin
	- ```cpp
	  std::string line;
	  while (std::getline(std::cin, line)) {
	      // line
	  }
	  ```
-