# awk를 netstat, wach 명령어로 사용할때 발생하는 이슈

netstat과 watch 명령어를 혼합해서 모니터링할 때, 잘 모니터링이 안되는 경우가 있다.

```
 watch -n 1  "netstat -an | grep 3306  | grep EST | awk '{print $5'} | sort | uniq -c | sort -k 1,1r"
```

$ 앞에 \만 넣으면 된다..

```
watch -n1 "netstat -an | grep 3306  | grep EST | awk '{print \$5'} | sort | uniq -c | sort -k 1,1r"
```
