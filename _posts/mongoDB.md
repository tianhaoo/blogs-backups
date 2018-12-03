---
title: mongoDB
date: 2018-03-09 15:40:34
categories:
- 后端
tags:
    - mongoDB
---
## install
1. import the public key
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5
```
2. create a list file for MongoDB
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list
```
3. Reload local package database
```
sudo apt update
```
<!--more-->

4. Install the MOngoDB packages;
```
sudo apt install -y mongodb-org
```
5. Start MOngoDB
```
sudo service mogod start
```
6. Verify that MOngoDB has started successfully, by checking the contents of the log file at /var/log/mongodb/mongod.log for a line reading
```
[initandlisten] waiting for connections on port 27017
```
7. Stop MOngoDB
```
sudo service mongod stop
```
8. Restart MongoDB
```
sudo service mongod restart
```
9. Begin using MongoDB
```
mongo --host 127.0.0.1:27017
```
10. Uninstall MongoDB
```
sudo service mongod stop
sudo apt purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```



[more](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#import-the-public-key-used-by-the-package-management-system)

