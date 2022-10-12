- ## Make a boot USB
  collapsed:: true
	- platform:: mac
	- Step1: Insert USB
	- Step2: Find index of your USB
	  ```bash
	  diskutil list
	  ```
	- Step3:
	  ```bash
	  diskutil unmountDisk /dev/diskN
	  ```
	- Step4:
	  ```bash
	  sudo dd if=YOURISOFILE.ISO of=/dev/rdiskN bs=1m
	  ```
	- Step5:
	  ```bash
	  diskutil eject /dev/diskN
	  ```
- ## Ansible: `Please pip install hvac to use the hashi_vault lookup module.`
  collapsed:: true
	- platform:: mac
	- Error msg:
	  ```
	  An unhandled exception occurred while running the lookup plugin 'hashi_vault'.
	  Error was a <class 'ansible.errors.AnsibleError'>,
	  original message:
	  Please pip install hvac to use the hashi_vault lookup module.
	  ```
	- Please check Ansible's python environment:
	  ```bash
	  ansible -m debug -a 'var=ansible_playbook_python' localhost
	  ```
	- The terminal will show the path of Ansible's env:
	  ```
	  localhost | SUCCESS => {
	      "ansible_playbook_python": "/usr/local/Cellar/ansible/2.6.0/libexec/bin/python2.7"
	  }
	  ```
	- Pip install!
	  ```bash
	  source /usr/local/Celler/ansible/2.6.0/libexec/bin/activate
	  pip install hvac
	  ```
	- Ref: https://github.com/ansible/ansible/issues/28050#issuecomment-402220672
	- 2022 Update:
		- python2 is retired. Just
		  ```bash
		  pip install hvac
		  ```
		  is ok.
- ## Mac use NTFS
  collapsed:: true
	- 2022 Solution:
		- Install FUSE + ntfs-3g ([Can't install NTFS-3G on macOS BigSur](https://apple.stackexchange.com/questions/422521/cant-install-ntfs-3g-on-macos-bigsur))
		  ```bash
		  brew tap gromgit/homebrew-fuse
		  brew install --cask macfuse
		  brew install ntfs-3g-mac 
		  ```
		- Unmount readonly volume
		- Mount ([在 macOS 上掛載可寫的NTFS](https://blog.roy4801.tw/2019/03/31/%E5%9C%A8-macOS-%E4%B8%8A%E6%8E%9B%E8%BC%89%E5%8F%AF%E5%AF%AB%E7%9A%84NTFS/))
			- Choose disk id
			  ```bash
			  diskutil list
			  ```
			- Prepare mount place
			  ```bash
			  sudo mkdir /Volumes/<disk name>
			  ```
			- Mount command
			  ```bash
			  sudo /usr/local/bin/ntfs-3g /dev/disk2s1 /Volumes/<disk name> -olocal -oallow_other
			  ```