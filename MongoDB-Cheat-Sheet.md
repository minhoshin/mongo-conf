### CHEAT SHEET

##### 관리자 추가
```
$ mongo
> use admin
// ROOT ACCOUNT MAKE CASE 1
> db.createUser({ user: "ROOT_ID", pwd: "USER_PASSWORD", roles: [{ role: "userAdminAnyDatabase", db: "admin"}] })
// ROOT ACCOUNT MAKE CASE 2
> db.createUser({ user: "ROOT_ID", pwd: "ROOT_PASSWORD", roles: ["root"] })
> exit
bye
$ sudo systemctl restart mongod
$ sudo systemctl status mongod
```

##### 관리자 접속
```
// ROOT AUTH CASE 1
$ mongo -u ROOT_ID -p ROOT_PASSWORD admin
// ROOT AUTH CASE 2
$ mongo
> db.auth('ROOT_ID', 'ROOT_PASSWORD')
1
> use admin
> show users
```

##### 데이터베이스 관리자  추가
```
> use DATABASE
> db.createUser({ user: "DB_USER_ID", pwd: "DB_USER_PASSWORD", roles: ["dbOwner"] })
> exit
bye
$ mongo
> use DATABASE
> db.auth('DB_USER_ID', 'DB_USER_PASSWORD')
1
> show dbs
DATABASE 0.000GB
> show users
```

##### 외부 접속 방법 추가
```
> vi /etc/mongod.conf
# network interfaces
net:
  port: 27017
#  bindIp: 127.0.0.1 // 주석처리
  bindIp: 0.0.0.0 // 모든 ip 접속 허용
#  bindIp: 192.168.1.1, 192.168.1.2 // 해당 ip 접속 허용

$ sudo systemctl restart mongod
$ sudo systemctl status mongod

$ mongo -u ID -p PASSWORD 192.168.12.345/db
```

