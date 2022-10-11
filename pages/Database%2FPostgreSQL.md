title:: Database/PostgreSQL

- How to read: post-gress-Q-L
- ## pg_ctl
	- [Ubuntu: Manual Page](https://manpages.ubuntu.com/manpages/focal/en/man1/pg_ctl.1.html)
	- `reload` only reload config file, such as `postgresql.conf` ,  `pg_hba.conf`
	- `restart` restart instance
- ## `log_cnt` in `id_seq`
	- When try to get the status of a id sequence in a standby pg server, we might get:
	  ```
	  # SELECT * FROM xxx_id_seq;
	   last_value | log_cnt | is_called 
	  ------------+---------+-----------
	        40082 |       0 | t
	  (1 row)
	  ```
	- But when we select it from main server:
	  ```
	   last_value | log_cnt | is_called 
	  ------------+---------+-----------
	        40050 |      32 | t
	  ```
	- Why ??
	- ```c
	  // src/backend/commands/sequence.c:
	  /*
	   * We don't want to log each fetching of a value from a sequence,
	   * so we pre-log a few fetches in advance. In the event of
	   * crash we can lose (skip over) as many values as we pre-logged.
	   */
	  #define SEQ_LOG_VALS    32
	  ```
	- https://stackoverflow.com/a/66458412/4411336
- ## Get Active Connections' Addresses
	- ```sql
	  SELECT DISTINCT client_addr FROM pg_stat_activity;
	  ```
- ## Check Replication State (v10)
	- ```sql
	  SELECT client_addr, state, sent_lsn, write_lsn, flush_lsn, replay_lsn
	  FROM pg_stat_replication;
	  ```
	- [pg_stat_replication sent_location, write_location, flush_location, replay_location的差别](https://billtian.github.io/digoal.blog/2016/01/13/01.html)
		- `sent_location` is the location that sent to standby.
		- `write_location` is the location where standby WAL at.
- ## Replication Config
	- `synchronous_commit`:
		- `off`:
			- Commit won't wait until write into WAL
			- May rollback.
		- `on`:
			- Commit wait until write into WAL
		- `local`:
			- Commit wait until write into **local** WAL
	- `synchronous_standby_names`:
		- At least `{num_sync}` in `{standby_names}` is synced
		  ```
		  ANY num_sync ( standby_name [, ...] )
		  ```
- ## Dump + Restore
	- Dump all table, data only
		- ```bash
		  pg_dump -h localhost -p 5432 -U {db_username} \
		  	-F c -v -f data.dump --data-only {db_name}
		  ```
		- `-F c`: compress as binary format
	- Restore
		- ```bash
		  pg_restore -h localhost -p 5432 -U {db_username} \
		  	-F c -d {db_name} --data-only -v data.dump
		  ```
		- Print:
		  ```
		  pg_restore: processing data for table xxx...
		  pg_restore: executing SEQUENCE SET xxx_seq...
		  ```