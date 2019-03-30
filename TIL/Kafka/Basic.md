### Kafka 기초
---

Kafka : 분산 메시징 시스템으로 대용량 스트림 정보를 처리하기 위한 오픈 소스 프로젝트
ActiveMQ, RabbitMQ 등의 기술과 비교되지만 대량 처리에 특화되어 있어 기존 MQ에 비해 TPS가 우수하다.
역으로 범용적인 MQ가 제공하는 다양한 기능은 제공되지 않을 수 있다.

기존의 메시징 시스템에서는 broker가 consumer에게 메시지를 push해 주는 방식인데 반해,
Kafka는 consumer가 broker로부터 직접 메시지를 가지고 가는 pull 방식으로 동작하기 때문에
consumer는 자신의 처리능력만큼의 메시지만 broker로부터 가져오기 때문에 최적의 성능을 낼 수 있다.

---
#### 참고

https://epicdevs.com/17

https://soul0.tistory.com/499

https://taetaetae.github.io/2017/11/02/what-is-kafka/

https://sarc.io/index.php/miscellaneous/1436-kafka