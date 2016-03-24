# Redis Cluster 에서의 Pub/Sub은 동작 방식

* Redis 클러스터에서 pub/sub는 잘 동작합니다. 한 노드에서 subscribe하고 다른 노드에서 publish하면 받을 수 있다.

현재 구현 방식을 찾아보면  클라이언트는 클러스터 내부의 임의의 노드에 접속해서 subscribe 명령을 남긴다.
이러면 서버는 해쉬테이블에 `server.pubsub_channels` 라는 채널을 생성해둔다.
그리고 자신을 제외하고 메시지를 모든 노드에 보낸다.

즉 클러스터가 많아지면 성능 이슈가 필연적으로 발생한다.


## 참고
(https://charsyam.wordpress.com/2016/03/21/%EC%9E%85-%EA%B0%9C%EB%B0%9C-redis-cluster-%EC%97%90%EC%84%9C%EC%9D%98-pubsub%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%A0%EA%B9%8C/)
