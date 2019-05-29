# Kafka 기초

Kafka : 분산 메시징 시스템으로 대용량 스트림 정보를 처리하기 위한 오픈 소스 프로젝트
ActiveMQ, RabbitMQ 등의 기술과 비교되지만 대량 처리에 특화되어 있어 기존 MQ에 비해 TPS가 우수하다.
역으로 범용적인 MQ가 제공하는 다양한 기능은 제공되지 않을 수 있다.

기존의 메시징 시스템에서는 broker가 consumer에게 message를 push 해 주는 방식이지만,
Kafka는 consumer가 broker로부터 직접 message를 가지고 가는 pull 방식으로 동작하기 때문에
consumer는 자신의 처리능력만큼의 message만 broker로부터 가져오기 때문에 최적의 성능을 낼 수 있다.

기존 메시징 시스템보다 대용량의 실시간 로그 처리에 특화되어 설계된  시스템으로 기존 범용 메시징 시스템 대비 TPS가 매우 우수하지만 특화된 시스템이기 때문에 다른 기존 범용 시스템들이 제공하는 다양한 기능들은 제공되지 않는다.

message를 메모리에 저장하는 시스템과 달리 파일 시스템에 저장한다.

분산 시스템을 기본으로 설계되어 있기 때문에, 기존 다른 시스템에 비해 분산 및 복제 구성을 쉽게 할 수 있다.

### Kafka 용어 간단 정리

producer : message 생산(발행)자.
consumer : message 소비자
consumer group : consumer들끼리 메세지를 나눠서 가져간다. offset을 공유하여 중복으로 가져가지 않는다.
broker : 카프카 서버를 가리킴
zookeeper : 카프카 서버(+클러스터) 상태를 관리
cluster : 브로커들의 묶음
topic : message 종류
partitions : topic이 나눠지는 단위
Log : 1개의 message
offset : 파티션 내에서 각 message가 가지는 unique id

### 큰 message를 Kafka로 처리

Kafka는 큰 message를 처리하기 위한 것이 아니다. message의 최대 크기는 1MB이다. (Apache Kafka의 브로커 기본 설정 message.max.bytes)
만약 크기를 더 늘리고 싶다면 위 설정을 이용해서 크기를 늘리고 producer와 consumer의 네트워크 버퍼를 늘려야 한다.

Kafka는 message의 양이 많지만 크기가 크지 않은 경우에만 가장 잘 작동한다는 점을 기억해야 한다.

---
#### 참고

https://epicdevs.com/17

https://soul0.tistory.com/499

https://taetaetae.github.io/2017/11/02/what-is-kafka/

https://sarc.io/index.php/miscellaneous/1436-kafka

https://stackoverflow.com/questions/21020347/how-can-i-send-large-messages-with-kafka-over-15mb