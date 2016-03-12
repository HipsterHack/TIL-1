# 카프카란?

아파치 카프카는 LinkedIn에서 만든 MQ이다. 이전에 사용하던 ActiveMQ와 Splunk를 대체하기 위하여 개발하였다.


# 카프카의 특징 

* Scala/Java로 작성되어 있다.
* 분산처리(Distributed Processing)가 가능하다.
* Log Data를 디스크에 기록한다.
* Publish-Subscribe구조로 되어 있다.
* 확장 가능(Scalablility)하다.
* 처리량이 높다.(High Throughput)
* 응답속도가 빠르다.(Low Latency)


# 카프카의 아키텍쳐

Publish-subscribe 구조를 가진다. Producer는 Message를 Broker로 발행한다. Consumer는 메시지를 구독하는 형태다.
Broker는 클러스터로 관리한다. 그리고 Zookeeper가 각 노드를 모니터링한다. 
