

>컨슈머 API의 핵심은 서버에 추가 데이터가 들어왔는지 폴링하는 단순한 루프다.


## poll()
```java
ConsumerRecords<String, String> records = consumer.poll(Duration.ofSeconds(10));
```

- poll() 메서드는 카프카를 무한으로 polling 처리하는 메서드임.
- session timeout 값을 Parameter로 넘겨받아. poll()이 block될 수 있는 최대 시간을 결정한다.
	- 데이터를 넘겨받기까지의 대기시간.
	- 값이 0으로 지정되어 있거나, 버퍼에 데이터가 없을 경우 즉시 retrun.
- 레코드들이 저장된 `ConsumerRecords` 객체를 retrun한다.
- 새 컨슈머에서 처음으로 poll() 메서드를 호출하면 컨슈머는 GroupCoordinator를 찾아서 컨슈머 그룹에 참가하고, 파티션을 할당받는다.
- 리밸런스에 연관된 callback들과 함께 리밸런스 처리가 이루어진다.
-  🔴 `max.poll.interval.ms` 에 지정된 시간 이상으로 호출되지 않을 경우, 컨슈머는 죽은것으로 판정되어 컨슈머 그룹에서 <mark style="background: #FF5582A6;">퇴출</mark>된다. 따라서 폴링 루프안에서 예측 불가능한 시간동안 블록되는 작업을 수행하는 것을 피해야 함 !!!

