# 덜 알려진 리눅스 커맨드

1. `sudo!!` 바로 이전에 실행했던 커맨드에 sudo를 써서 수행해준다. 예를 들면 root로만 access 가능한 test.txt 파일을 vi로 열었을 때
```
 $ vi test.txt
# sudo vi test.txt
 $ sudo!! 
```
2. `python -m SimpleHTTPServer` : 현재를 working dir로 지정한 8000번 포트의 웹서버를 만든다
3. `mtr` ping 과 traceout 커맨드를 합친 명령어
4. Ctrl+x+e 를 누르면 에디터가 바로 나타난다. 
5. `nl` output이 라인번호가 붙여져서 나온다.
6. `shuf` file이나 폴더에서 file의 라인, 파일, 폴더가 선택된다.
7. `ss` 소켓 통계를 보여준다.
8. `Last` 마지막으로 로그인 했던 사용자가 했던 히스토리를 보여준다.
9. `curl ifconfig.me` 머신의 외부 IP를 보여준다.
10. `tree` 현재 디렉토리의 파일과 하위 폴더를 재귀적으로 탐색해서 보여준다.
11. `Pstree` 현재 돌고 있는 프로세스와 자식 프로세스를 재귀적으로 탐색해서 보여준다.
12. `<space> command` : 커맨드 히스토리를 기록하지 않는다.
13. `stat` : 파일과 파일 시스템의 상태 정보를 보여준다.
14. `<alt>. And <esc>` :  마지막 아규먼트를 첫번째로 
15. `Pv` : 할리우드 영화같이 출력을 시뮬레이팅 해준다.
16. `Mount | column -t ` : 마운트한 파일 시스템을 괜찮은 포맷으로 보여준다.
17. `Ctrl + l` 쉘 프롬프트를 지워준다.
18. `curl -u gmail_id –silent “https://mail.google.com/mail/feed/atom” | perl -ne ‘print “\t” if //; print “$2\n” if /(.*)/;’.` : 읽지 않은 메일을 터미널에 보여준다
19. `file` 파일에 관한 출력 정보
20. `id`: 사용자 그룹의 아이디를 프린트한다.


## 참고 

http://www.tecmint.com/51-useful-lesser-known-commands-for-linux-users/
