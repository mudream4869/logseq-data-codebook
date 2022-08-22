public:: true
title:: Code Snippets/ssh

- Disable root login
	- ```bash
	  sed -i 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config
	  /etc/init.d/ssh restart
	  ```
- Tips: http://www.study-area.org/tips/ssh_tips.htm