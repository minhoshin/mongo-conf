##### CHEAT SHEET

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

// ROOT AUTH CASE 1
$ mongo -u ROOT_ID -p ROOT_PASSWORD admin
// ROOT AUTH CASE 2
$ mongo
> db.auth('ROOT_ID', 'ROOT_PASSWORD')
> use admin
> show users

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

