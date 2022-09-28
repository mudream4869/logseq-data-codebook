title:: Database/PostgreSQL

- ## Get Active Connections' Addresses
	- ```sql
	  SELECT DISTINCT client_addr FROM pg_stat_activity;
	  ```
-