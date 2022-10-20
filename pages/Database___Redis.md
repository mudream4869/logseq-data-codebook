title:: Database/Redis

- ## Cluster
	- ### Troubleshooting: Stuck on Creating Cluster
		- Sometimes stuck on here:
		  ```bash
		  redis-cli --cluster create ... --cluster-replicas 1 --cluster-yes
		  ```
		- Should check
			- Docker's network: Redis server should broadcast ip with docker's ip or use host mode
			- Nodes' name maybe duplicate: Check the data in `/var/lib/redis/nodes.conf`
				- Ref: https://linux.m2osw.com/redis-infamous-waiting-cluster-join-message
	- ### Server Version
		- ```
		  $ redis-cli
		  > INFO SERVER
		  ```
	- ### Nodes
		- ```
		  $ redis-cli -c # -c to use cluster mode
		  127.0.0.1:6379> CLUSTER NODES
		  ```
	- ### Redis Cluster: (error) MOVED
		- Should connect cluster node with `-c` option
		- ```
		  $ redis-cli -c
		  ```