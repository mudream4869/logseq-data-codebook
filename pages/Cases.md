- ## Make a boot USB
  collapsed:: true
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
	  ```
	  ansible -m debug -a 'var=ansible_playbook_python' localhost
	  ```
	- The terminal will show the path of Ansible's env:
	  ```
	  localhost | SUCCESS => {
	      "ansible_playbook_python": "/usr/local/Cellar/ansible/2.6.0/libexec/bin/python2.7"
	  }
	  ```
	- Pip install!
	  ```
	  source /usr/local/Celler/ansible/2.6.0/libexec/bin/activate
	  pip install hvac
	  ```
	- Ref: https://github.com/ansible/ansible/issues/28050#issuecomment-402220672