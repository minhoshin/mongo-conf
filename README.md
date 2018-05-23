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
$ ps -ef | grep mongo 로 확인

* mongo SECONDARY
mongod.conf 설정(설정 파일 참고) 완료 후
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo 로 확인

* mongo ARBITER
mongod.conf 설정(설정 파일 참고) 완료 후
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo 로 확인

// PRIMARY, SECONDARY, ARBITER 서버에서 몽고를 정상적으로 실행 하였다면

* mongo replication 설정
$ mongo PRIMARY 에서 mongo 로 접속 후 아래 replication 설정 시작
> mongo
> rs.initiate( { "_id": "grapAcsDb", "members": [ { _id: 0, host: "10.5.0.7:27017"} ] } ) // _id 는 mongod.conf replSetName
> rs.status() // 상태를 확인하면 아래와 같이 변경됨
grapAcsDb:PRIMARY> // PRIMARY 설정 성공
grapAcsDb:PRIMARY> use admin // admin database 이동
switched to db admin
grapAcsDb:PRIMARY> db.createUser({user: "ROOT_ID", pwd: "ROOT_PASSWO", roles:["root"]})
```

```
* mongo arbiter
```

```
* 명령어
db.shutdownServer({force:true})
```