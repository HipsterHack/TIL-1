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

