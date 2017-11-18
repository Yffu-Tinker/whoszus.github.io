```shell
# mysql master docker startup shell code 
docker run --name tinker-mysql-master -p 3306:3306 -v /home/tinker/docker/mysql/my-master.cnf:/etc/my.cnf -e MYSQL_ROOT_PASSWORD=gosuncn -e MYSQL_ROOT_PASSWORD=gosuncn -e MYSQL_USER=tinker -e MYSQL_PASSWORD=tinker -d mysql/mysql-server:5.7.20

# mysql slave docker startup shell code  
docker run --name tinker-mysql-slave -p 13306:3306 -v /home/tinker/docker/mysql/my-slave.cnf:/etc/my.cnf -e MYSQL_ROOT_PASSWORD=gosuncn -e MYSQL_USER=tinker -e MYSQL_PASSWORD=tinker -d mysql/mysql-server:5.7.20

# enter mysql-docker cli 
docker exec -it tinker-mysql-master bash
# init
mysql -uroot -pgosuncn --socket=/var/run/mysqld/mysqld.sock -e "grant all privileges on *.* to 'root'@'%' identified by 'gosuncn';"



# Q&A 
	- Fatal error: Please read "Security" section of the manual to find out how to run mysqld as root!
	!# 

```

