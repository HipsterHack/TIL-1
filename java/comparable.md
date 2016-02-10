

# Comparable 인터페이스 
* 이 인터페이스는 아래 메소드를 갖는다.
```
int	compareTo(T o)
```
* equals와 유사한 특성을 갖음
* Collection API에서 sort할때 사용한다, 예를 들면 
```
Arrays.sort(a);
```

## compareTo 메소드 규칙

1. 현재 객체와 파리미터를 비교한다. 
  + 현재 값이 작으면 -1
  + 현재 값 이랑 같으면 0
  + 현재 값보다 크면 1
2. 비교가 불가능하면 ClassCastException을 발생시킨다.

3. 모든 x,y `sgn(x, compareTo(y)) == -sgn(y.compareTo(x))`

4. 이행관계의 성립
  + `(x.compareTo(y) > 0 && y.compareTo(z) > 0)` 이면 `x.compareTo(z) > 0`

5. `x.compareTo(y) == 0` 이면 `sgn(x.compareTo(z)) == sgn(y.compareTo(z))`이 되어야 한다.


6. `(x.compareTo(y) == 0) == (x.equals(y))`가 되도록 하면 좋다. 




