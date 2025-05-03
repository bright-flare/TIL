
# 특정 offset의 레코드 읽어오기

poll() 메서드를 호출하여 가장 최근 offset부터 읽는게 아닌 특정 offset에서 읽어오는 방법도 존재한다. 


## seekToBeginning

- 파티션의 맨 앞(가장 오래된)에서부터 모든 메세지를 읽어온다.
```java
consumer.seekToBeginning(partitions);  
```


## seekToEnd
- 파티션에 새로 들어온 메세지부터 읽기 시작.
```java
consumer.seekToEnd(partitions);
```