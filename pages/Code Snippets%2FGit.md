title:: Code Snippets/Git

- ## Remove submodule
	- ```bash
	  mv a/submodule a/submodule_tmp
	  
	  git submodule deinit -f -- a/submodule    
	  rm -rf .git/modules/a/submodule
	  git rm -f a/submodule
	  ```
- ## Config username and email
	- ```bash
	  git config --global user.name "Name"
	  git config --global user.email "email@example.com"
	  ```
- ## Stop replacing LF to CRLF!
	- ```bash
	  git config --local core.autocrlf false
	  ```
- ## Undo git add
	- ```bash
	  git reset <FILENAME>
	  ```