# 루비 메타 프로그래밍

* 클래스나 메소드의 메타정보를 이용해 유연한 코드를 만드는방법
* 혹자는 이해할 수 없는 코드를 만드는 방법이라 칭하기도 한다.


## 객체 안에서 인스턴스 변수에 값넣기

* 클래스 내부에서 instance_variable_set을 이용하면 인스턴스 변수에 값을 세팅할 수 있다. 
```
hash = Hash.new
hash['test'] = 'val'
hash.each { |name, value| instance_variable_set("@#{name}", value) }
```
