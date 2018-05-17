# mongo-conf

```
* MongoDB 다운로드
MongoDB Download Center
Community Server
Previous Release 선택 후 다운로드
```

```
* 설치
MongoDB Release 서버에 업로드
data, log 폴더 생성

* 서비스 등록
vi .bashrc 입력 후 아래 내용 입력
PATH=$PATH:/home/partner/mongodb/bin
위 내용 입력 후 빠져 나오기
source .bashrc 로 등록
```

```
* key file 생성
$ openssl rand -base64 741 > ./mongodb-keyfile
$ chmod 600 mongodb-keyfile
키파일을 마스터 슬레이브 공유
```

```
* mongo master
mongod.conf 설정(설정 파일 참고) 완료 후
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo 로 확인

* mongo slave
mongod.conf 설정(설정 파일 참고) 완료 후
$ mongod -f mongod.conf 로 실행
$ ps -ef | grep mongo 로 확인

* mongo replication 설정
$ mongo 로 접속 후 아래 replication 설정
> rs.initiate( { _id: "grapAcsDb", members: [ { _id: 0, host: "10.5.0.7:27017"}, { _id: 1, host: "10.5.0.8:27017"} ] } ) // _id 는 mongod.conf replSetName
설정이 완료되면 master slave mongo 접속 후 마스터 슬레이브 설정 상태 확인
grapAcsDb:PRIMARY>
grapAcsDb:SECONDARY>

추가로 rs.statsu()

* mongo arbiter
```

```
* 명령어
db.shutdownServer({force:true})
```