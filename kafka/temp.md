
# 주키퍼 정보 확인

  

# zookeeper 접속

$ telnet localhost 2181

  

# zookeeper 서버 정보 확인

$ srvr

  
  

# 브로커 실행

cd /opt/homebrew/Cellar/kafka/3.5.1/bin

sh kafka-server-start -daemon ../../3.5.1/.bottle/etc/kafka/server.properties

  
  

# 토픽 생성

sh kafka-topics --bootstrap-server localhost:9092 --create --replication-factor 1 --partitions 1 --topic test Created topic "test".

  

# 토픽에 메세지 작성

kafka-console-producer --bootstrap-server localhost:9092 --topic test

> create test message

> create test meaaage 2

  

# 토픽 메세지 읽기
```
kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
```

# 출력

create test message

create test meaaage 2