# awk 

* 패턴과 검색을 하기 위해 만들어진 유틸리티.

## 응용법
* 특정 형식에 맞게 이루어져있는지 검사
* 리포트 만들기


## 사용법

```
awk [-f 프로그램파일] [-F 필드 구분자] ["패턴{액션}"] [처리할 파일명
```

### 패턴

- BEGIN 첫번째 레코드를 읽기전에 액션 실행
- END   마지막 레코드를 읽고 난뒤 액션 실행
- PATTERN 각 라인별로 액션 실행

```
awk BEGIN { print "begin" } { s += $1 } END { print "s is ",s }
```


## 예제

```

% cat awkprog2
{sum += $3}
END {printf "The average of the ages is %.2f\n", sum/NR}

% cat awkprog3
{sum += $3 
++no}
END {printf "The average of the ages is %.2f\n", sum/no}

<결 과>
% awk -f awkprog3 students
```
