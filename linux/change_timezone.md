## 리눅스 서버 타임존 설정하는 방법 

요즈음에는 개인이나 회사나 서버를 이용할때 IDC에 직접 넣기 보단 클라우드 서비스를 주로 이용한다

그런데, AWS나 해외 클라우드 시스템을 사용하면, 시스템 로컬 시간이 그 지역 서버시간에 맞춰져 있다. 
(AWS로 서버를 발급받으면 PST(미국 태평양 시간)인 경우가 많다. 아마도 미국쪽 서버를 받아서 그렇겠지-_-;)

프로그램에서 시스템 시간을 사용하는 경우 서비스 운영시간의 차이가 있으면 당연히 문제가 발생한다.

그래서 프로그램에서 주로 사용하는 시간대로 맞춰 주는 것이 여러모로 용이하다.

운영팀과의 커뮤니케이션과 개발 모니터링을 위해서도 그게 훨 유용하다.

한 서버에 여러시간대 서비스를 만들지 않는 한은 말이다.

그래서, 현재 사용하고 있는 localtime 을 구하려면 

*  date 를 사용한다. 맨 뒤에 Timezone을 확인할 수 있다.
```  
  date
  >> 2015년 11월  2일 월요일 11시 36분 55초 JST
```  
* /etc/localtime 값을 확인. 현재 타임존 값인 JST 를 확인할 수 있음
```
  sudo cat /etc/localtime 
```
* ll -a 로 어느 타임존을 사용하는지 알 수 있다.(보통 심볼릭 링크로 연결 해두기 땜시로)
```
ll -a /etc/localtime
 /etc/localtime -> /usr/share/zoneinfo/Asia/Tokyo
```

위의 3가지 방법을 사용할 수 있다.

그럼 어떻게 바꿔야 할까?

리눅스 Timezon 정보는 /usr/share/zoneinfo/ 아래에 있다.

한국의 경우엔 Asia/Seoul의 파일을 Timezone 으로 사용하면 된다.
```
/usr/share/zoneinfo/Asia/Seoul
```
이제 기존 파일을 대체해보자. 
시스템 변경이므로 당연히 루트 권한이 필요하다. sudo 로 얻자.

```
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

ln의 명령어중 f옵션은 파일이 존재하면 덮어 쓴다.
