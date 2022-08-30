title:: Code Snippets/Bash

- Find and Replace in file:
	- ```bash
	  sed -i 's/old-text/new-text/g' filename
	  ```
- Find files which size larger than 1M:
	- ```bash
	  find . -type f -size +1M
	  ```
- Find files:
	- ```bash
	  find -L . -name '*.png'
	  ```
- Unzip .tar.gz:
	- ```bash
	  tar zxvf {tar filename}
	  ```
- Zip .tar.gz:
	- ```bash
	  tar zcvf {tar filename} {dir}
	  ```
- Page Size
	- ```bash
	  getconf PAGESIZE
	  ```
- CPU Info
	- ```bash
	  lscpu
	  ```
- Get OOM Log
	- Source: [Finding which process was killed by Linux OOM killer](https://stackoverflow.com/questions/624857/finding-which-process-was-killed-by-linux-oom-killer)
	- ```bash
	  dmesg -T | egrep -i 'killed process'
	  ```
- Replace strings in a file
	- Source: [How to replace a string in multiple files in linux command line](https://stackoverflow.com/questions/11392478/how-to-replace-a-string-in-multiple-files-in-linux-command-line)
	- Mac
		- ```bash
		  sed -i '.bak' 's/foo/bar/g' *
		  ```
	- Linux
		- ```bash
		  sed -i 's/foo/bar/g' *
		  ```