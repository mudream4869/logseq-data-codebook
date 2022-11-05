title:: Linux/Ubuntu

- ## Upgrade App for Security Issue
  id:: 634e68bd-d960-4be4-a489-1babe761f48d
	- Basically,  Ubuntu's apt repo provide security patch version.
		- For example: Nginx
		- The apt wbepage: https://packages.ubuntu.com/focal/nginx
		- Although the nginx webpage said that there are security issue for `0.6.18-1.20.0`
			- ```
			  1-byte memory overwrite in resolver
			  Severity: medium
			  CVE-2021-23017
			  Not vulnerable: 1.21.0+, 1.20.1+
			  Vulnerable: 0.6.18-1.20.0
			  ```
		- Notice that extra version `1.20.0-0ubuntu1.2` which is diff from `1.20.0`, and [change log](https://changelogs.ubuntu.com/changelogs/pool/main/n/nginx/nginx_1.18.0-0ubuntu1.3/changelog) for Ubuntu's nginx apt repo.
			- ```
			  nginx (1.18.0-0ubuntu1.2) focal-security; urgency=medium
			  
			    * SECURITY UPDATE: DNS Resolver issues
			      - debian/patches/CVE-2021-23017-1.patch: fixed off-by-one write in
			        src/core/ngx_resolver.c.
			      - debian/patches/CVE-2021-23017-2.patch: fixed off-by-one read in
			        src/core/ngx_resolver.c.
			      - CVE-2021-23017
			  ```
		- Ubuntu apt repo patched it! 😍