title:: Code Snippets/C++

- ## [[Memory Usage]]
	- https://stackoverflow.com/questions/669438/how-to-get-memory-usage-at-runtime-using-c
	- ```cpp
	  #include <unistd.h>
	  
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
- ## Read File Content
	- ```cpp
	  std::string getFileString(const std::string filename) {
	      std::ifstream ifs(filename);
	      return std::string((std::istreambuf_iterator<char>(ifs)),
	                         (std::istreambuf_iterator<char>()));
	  }
	  ```
- ## Timing
	- ```cpp
	  auto t1 = std::chrono::high_resolution_clock::now();
	  // func();
	  auto t2 = std::chrono::high_resolution_clock::now();
	  double totalSeconds =
	          std::chrono::duration_cast<std::chrono::duration<double>>(t2 - t1)
	              .count();
	  ```
- ## Write Values
	- ```cpp
	  void writeValues(const std::string& filename,
	                   const std::vector<uint64_t>& values) {
	      std::ofstream fp;
	      fp.open(filename);
	      if (!fp) {
	          throw std::runtime_error(filename + " open error");
	      }
	  
	      for (const auto& v : values) {
	          fp << v << std::endl;
	      }
	  }
	  ```