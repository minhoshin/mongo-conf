$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

$ echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

$ sudo apt-get update

$ sudo apt-get install mongodb-org

$ sudo systemctl start mongod

$ ps -ef | grep mongo

$ sudo systemctl status mongod

$ sudo systemctl enable mongod

$ mongo
```
> use admin
> db.createUser({user: "UserID", pwd: "UserPassword", roles: [{ role: "userAdminAnyDatabase", db: "admin"}] })
ctrl + c // exit
```

$  sudo vi /etc/mongod.conf
```
security:
  authorization: "enabled"
```

$ sudo systemctl restart mongod

$ sudo systemctl status mongod

$ mongo -u UserID -p UserPassword admin


$ sudo ufw status

$ sudo ufw enable

$ sudo ufw allow OpenSSH

$ sudo ufw allow from client_ip_address to any port 27017

$ sudo ufw status

$  sudo vi /etc/mongod.conf
```
net:
  port: 27017
  bindIp: 127.0.0.1,IP_of_MongoHost
```

$ sudo systemctl restart mongod

$ sudo systemctl status mongod

$ mongo -u AdminSammy -p --authenticationDatabase admin --host IP_address_of_MongoHost
