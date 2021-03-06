# Real World Project: Database Shard
This project is done on Ubuntu 20.10.

## The prerequisites:
 Install Docker, Docker Compose and Mariadb.
 
## To install Docker:
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install docker-ce

```
#### This command is to check the Docker status 
```
sudo systemctl status docker
```
## To install Docker Compose:
```
sudo apt install docker-compose
```
## To install Mariadb:
```
sudo apt install mariadb-client
```
### Start clonning this [maxscale-docker repository](https://github.com/luma24/maxscale-docker) by running this command on the terminal.
```
git clone https://github.com/luma24/maxscale-docker
cd maxscale-docker/maxscale/
```
### To bring the containers up:
```
docker-compose up -d
```
### Now you should have 3 containers maxscale_master2_1, maxscale_master_1 and maxscale_maxscale_1 as shown below
```
maxscale_master2_1    docker-entrypoint.sh mysql ...   Up      0.0.0.0:4003->3306/tcp                                  
maxscale_master_1     docker-entrypoint.sh mysql ...   Up      0.0.0.0:4001->3306/tcp                                  
maxscale_maxscale_1   /usr/bin/tini -- docker-en ...   Up      3306/tcp, 0.0.0.0:4000->4000/tcp, 0.0.0.0:8989->8989/tcp

```
### Run this command to check that the servers up and running
```
docker-compose exec maxscale maxctrl list servers

┌────────────────┬─────────┬──────┬─────────────┬─────────────────┬───────────┐
│ Server         │ Address │ Port │ Connections │ State           │ GTID      │
├────────────────┼─────────┼──────┼─────────────┼─────────────────┼───────────┤
│ zip_master_one │ master  │ 3306 │ 0           │ Master, Running │ 0-3000-32 │
├────────────────┼─────────┼──────┼─────────────┼─────────────────┼───────────┤
│ zip_master_two │ master2 │ 3306 │ 0           │ Running         │ 0-3000-31 │
└────────────────┴─────────┴──────┴─────────────┴─────────────────┴───────────┘
```
### Run this command to check that the zipcodes_one and zipcodes_two databases there.
```
mariadb -umaxuser -pmaxpwd -h 127.0.0.1 -P 4000
```
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 1
Server version: 10.5.8-MariaDB-1:10.5.8+maria~focal-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| zipcodes_one       |
| zipcodes_two       |
+--------------------+

```

Once complete, to remove the cluster and maxscale containers:

```
docker-compose down -v
```
