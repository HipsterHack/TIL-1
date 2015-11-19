## Length, size, count 


### Length

* 메소드를 호출할때 요소의 개수를 세지 않는다.
* length 는 O(1) 연산속도를 가진다.
* enumerable 일부분이 아니다. 
* Ruby core에서는 같은이름으로 alias 로 size 있다.
* Rails의 ActiveRecord의 경우 현재 로드한 데이터 수를 구할때 사용한다.
* Array 의 요소 개수를 의미

### Size
* ActiveRecord의 경우는 쿼리를 스킵하여 개수를 센다.
* 데이터 로드 전에 개수를 셀때 주로 유용하다.

### count

* Enumerable#count 다. 
* 파라미터로 블록을 받는다. 이 블록은 개수를 확인하는데 쓰인다.
* 요소를 하나씩 뒤져 가면서 개수를 세는 연산이다.
* Rails의 ActiveRecord의 경우, 쿼리를 날려 전체 개수를 센다.



## 참고 
* http://batsov.com/articles/2014/02/17/the-elements-of-style-in-ruby-number-13-length-vs-size-vs-count
