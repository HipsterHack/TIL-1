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

아래는 그 구조이다.

![카프카](https://github.com/DanSeo/TIL/blob/master/MQ/kafka-cluster-overview.png)


# 성능

* [링크](http://blog.embian.com/19) 에 따르면 
``` 
1. Broker 수 증가에 따라 Throughput이 증가한다(1대->2대 : 33%, 2대->3대: 28%, SSD 사용시).
2. SSD를 사용하는 것이 HDD를 사용하는 것보다 우수한 성능을 보인다(최대 2.7배).
3. Producer 4대 / Broker 5대의 구성에서부터 Gigabit 대역폭에 가까운 Throughput을 보인다(129.7MB/sec).
```

라고 한다. 즉 broker 머신 추가보단 sharding 같이 토픽별로 나누어 관리하는게 성능상 효율은 좋다.
