# mongo-conf

```
* MongoDB 다운로드
MongoDB Download Center
Community Server
Previous Release 선택 후 다운로드
```

```
* 설치(PRIMARY, SECONDARY, ARBITER 서버)
MongoDB Release 서버에 업로드
data, log 폴더 생성

* 서비스 등록(PRIMARY, SECONDARY, ARBITER 서버)
vi .bashrc 입력 후 아래 내용 입력
PATH=$PATH:/home/partner/mongodb/bin
위 내용 입력 후 빠져 나오기
source .bashrc 로 등록
```

```
* key file 생성
$ openssl rand -base64 741 > ./mongodb-keyfile
$ chmod 600 mongodb-keyfile
키파일을 PRIMARY, SECONDARY, ARBITER 공유
```

```
* mongo PRIMARY
mongod.conf 설정(설정 파일 참고) 완료 후
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo // 확인

* mongo SECONDARY
mongod.conf 설정(설정 파일 참고) 완료 후
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo // 확인

* mongo ARBITER
mongod.conf 설정(설정 파일 참고) 중 journal enabled 를 false 로 변경 후 
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo // 확인

// PRIMARY, SECONDARY, ARBITER 서버에서 몽고를 정상적으로 실행 되었다면

* mongo PRIMARY replication 설정
$ mongo PRIMARY 에서 mongo 로 접속 후 아래 replication 설정 시작
$ mongo
> rs.initiate( { "_id": "grapAcsDb", "members": [ { _id: 0, host: "10.5.0.7:27017"} ] } ) // _id 는 mongod.conf replSetName
> rs.status() // 상태를 확인하면 아래와 같이 변경됨
grapAcsDb:PRIMARY> // PRIMARY 설정 성공
grapAcsDb:PRIMARY> use admin // admin database 이동
switched to db admin
grapAcsDb:PRIMARY> db.createUser({user: "ROOT_ID", pwd: "ROOT_PASSWORD", roles:["root"]})
Successfully added user: { "user" : "ROOT_ID", "roles" : [ "root" ] }
grapAcsDb:PRIMARY> exit
bye
$ mongo
grapAcsDb:PRIMARY> use admin // admin database 이동
grapAcsDb:PRIMARY> db.auth('ROOT_ID','ROOT_PASSWORD')
1 // root 계정 접근 성공

* mongo SECONDARY replication 설정
grapAcsDb:PRIMARY> rs.add("10.5.0.8:27017") // mongo PRIMARY 에서 실행
{ "ok" : 1 }
> rs.status() // SECONDARY 서버 에서 상태를 확인하면 아래와 같이 변경됨
grapAcsDb:SECONDARY> // SECONDARY 설정 성공
grapAcsDb:SECONDARY> use admin // admin database 이동
grapAcsDb:SECONDARY> db.auth('ROOT_ID','ROOT_PASSWORD')
1 // root 계정 접근 성공 및 replication 정상 작동 확인(데이터 복제 성공)
grapAcsDb:SECONDARY> rs.status() // PRIMARY, SECONDARY, ARBITER 상태 확인

* mongo ARBITER 설정
grapAcsDb:PRIMARY> rs.addArb("10.5.0.6:27017") // mongo PRIMARY 에서 실행
{ "ok" : 1 }
> rs.status() // ARBITER 서버 에서 상태를 확인하면 아래와 같이 변경됨
grapAcsDb:ARBITER> // ARBITER 설정 성공
grapAcsDb:ARBITER> rs.status() // PRIMARY, SECONDARY, ARBITER 상태 확인
```

```
* 복구
mongo SECONDARY 서버에서 
$ ps -ef | grep mongo // 확인 후
$ rm -rf data mongod.pid && mkdir data // SECONDARY data 모두 삭제
$ ls data // data 폴더 데이터 확인
$ mongod -f mongod.conf && mongo // 재 실행 및 mongo cli 접속
about to fork child process, waiting until server is ready for connections.
forked process: 8253
child process started successfully, parent exiting
MongoDB shell version v3.4.14
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.14
grapAcsDb:STARTUP2> // STARTUP 상태로 먼저 실행 하여 데이터 복구 진행
grapAcsDb:STARTUP2> rs.status() // 복구 완료가 되었을 경우 아래 SECONDARY 상태로 변경됨
grapAcsDb:SECONDARY>
```

```
* compact collection size 줄이기 SECONDARY 부터 테스트 실행
grapAcsDb:SECONDARY> rs.slaveOk() // SECONDARY 에서 작업을 할 때 필수 선행 명령어
grapAcsDb:SECONDARY> db.runCommand ( { compact: ''COLLECTION_NAME, force: false } ) // SECONDARY 서버는 force 를 false 로
grapAcsDb:PRIMARY> db.runCommand ( { compact: ''COLLECTION_NAME, force: true } ) // PRIMARY 서버는 force 를 true 로
```

```
* 명령어 모음
grapAcsDb:SECONDARY> rs.slaveOk() // SECONDARY 에서 작업을 할 때 필수 선행 명령어
grapAcsDb:SECONDARY> db.collection.getIndexes() // INDEX 확인
grapAcsDb:SECONDARY> db.collection.dropIndexes() // 전체 INDEX 삭제
grapAcsDb:SECONDARY> db.collection.dropIndex({'key': 1}) // 개별 INDEX 삭제
grapAcsDb:SECONDARY> db.collection.createIndex({'key': 1}, {expireAfterSeconds: seconds}) // TLL INDEX 생성
db.shutdownServer({force:true}) // shutdown
```

```
* 실행문 모음
db.collection.find({}).projection({id:1})
```

