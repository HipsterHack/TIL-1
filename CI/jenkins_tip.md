#  젠킨스(jenkins) 작업 정보 받기

아래 URL 로 현재 빌드되고 있는 작업 정보를 받을 수 있다. 
```
http://도메인/job/잡이름/api/json
http://도메인/job/잡이름/api/xml
```

depth 매개변수를 사용하여 적당히 잘라낼 수 있다. 

```
http://도메인/job/잡이름/api/json?depth=1
http://도메인/job/잡이름/api/json?depth=2
```

# 젠킨스(jenkins) 작업이 실행 중인지 확인하기

```
http://도메인/job/잡이름/lastBuild/api/json 
http://도메인/job/잡이름/lastBuild/api/xml
```
json으로 building가 'True'이면 작업 완료상태다. 
