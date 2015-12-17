## Git CheatSheet

괜찮은 (한글로된) Git CheatSheet이다.
꽤 정리를 잘했다.

[http://kwonnam.pe.kr/wiki/git/cheatsheet](http://kwonnam.pe.kr/wiki/git/cheatsheet)

### 커밋로그 author 바꾸기 
회사 깃헙에 개인 계정으로 커밋을 했다.
으앗 큰일났다!

하지만 Git에서 과거 커밋의 author를 일괄로 변경
마지막 커밋의 author는 아래와 같이 변경할 수 있다.

```
git commit --amend --author "Daniel Seo <mail@mail.com>"
```

### 커밋을 취소하는 방법
* reset을 이용하면 된다.
```
git reset --soft HEAD~1
```
* HEAD는 최신 커밋을 가리키는 포인터이다. 즉 위 명령이면 최신에서부터 1개 커밋만큼 아래로 포인터를 이동시킨다.
* 이 명령어 뒤에 git commit 을 한다면, 아까 변경을 커밋한다.
```
git reset --hard HEAD~1
```
* 최신의 1개 커밋을 지운다.
* 지워도 삭제한 커밋을 확인할 수 있다
* 그리고 되살릴수도 있다(90일 안이면..)
```
git reflog 
git checkout -b branchName sha
```
### work directory 내용을 저장하는 방법
* 작업하고 있는 내용을 무시하고 다른 브랜치에 작업해야한다면?

#### 임시 저장 
* git stash로 지금작업내용을 임시 저장한다.
```
git stash
```
#### 이제 다시 work dir로 되돌리려면? 
```
git stash pop
```
* 기본적으로 pop는 stack의 팝처럼 적용하고 stash를 지운다. 
* stash를 남겨두거나, 특정 stash를 반영하려면 아래처럼 한다.
```
git stash apply stash@{0}
```

#### stash  삭제 
* 아래 예처럼 drop 을 이용하면 특정 stash를 삭제할 수 있다
```
git stash drop stash@{0}
```

* 지금까지 stash한 내용을 확인하려면?
```
git stash list
```
* 
