title:: Code Snippets/Python

- ## Unzip
	- [Unzipping file in python](https://stackoverflow.com/questions/3451111/unzipping-files-in-python)
	- ```python
	  import zipfile
	  with zipfile.ZipFile(path_to_zip_file, 'r') as zip_ref:
	      zip_ref.extractall(directory_to_extract_to)
	  ```
- ## Stdin
	- ```python
	  import sys
	  for line in sys.stdin:
	      print(line.strip())
	  ```
-