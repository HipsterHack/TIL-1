# [Ruby] ==, ===, eql?, equal?

Ruby 2.2.0 기준으로 작성해보겠다.
루비를 처음 접할땐, 같은지 비교하는 메소드와 연산이 도합 4개나 되서 햇갈렸었다. 
(주로 다뤘던 자바는 두개로 단순한데 반해 루비는 4개씩이나 제공한다.)

각각의 용도에 대해 한번 정리를 해봤다.

## == (일반 비교)

일반적인 같은 값 비교 연산자.
보통 자식 클래스에서 override해서 많이 사용한다. 
ruby에서 값 비교할때 주로 사용하는 연산자가 바로 이것이다.
아래는 test case로 만든 예제다.	
재밌는 점은 Hash의 경우 키값만 가지고 비교를 한다.

```
  def test_generic_equality
    str = "a"

    assert_true str == str
    assert_true str == str.clone
    assert_true 1 == 1.0

    num = 1
    assert_true num == 1
    assert_true num == 1.0

    ht = Hash.new(data: 1)
    ht2 = Hash.new(data: 1)
    ht3 = Hash.new(data: 2)
    assert_true ht == ht2
    assert_true ht == ht3
  end
```

## === (case문 비교)

case 문에서 사용하는 비교 연산자가 ===이다.
자식 클래스에서 override해서 구현한다. case-when 문에서 비교할때 주로 사용한다.
ruby에서 코어 라이브러리에서 이 연산자를 override해서 구현 한 것은 아래 3가지다.

* Range
* Regex
* Proc

```
  def test_object_case_equality
    num = 1
    assert_true num === 1
    assert_true num === 1.0

    str = "a"
    assert_true str === "a"
    assert_true str === str.clone

    ht = Hash.new(data: 1)
    ht2 = Hash.new(data: 1)
    ht3 = Hash.new(data: 2)

    assert_true ht === ht2
    assert_true ht === ht3

    case_data = 50
    case case_data
      when (0..5) then fail()
      when (5..51) then assert_include((5..51).to_a, case_data)
    end
  end
  ```

## eql? (hash 비교)
같은 해쉬키를 가지고 있으면 true를 리턴하는 메소드다. Hash에서 같은지를 비교할때 사용한다
Object에 있는 메소드이기때문에 사용은 할 수가 있는데. Object의 기본 구현은 ==와 동일하다
단, Numeric 타입은 ==와 구현이 약간 다르다.(1과 1.0을 타입까지 비교를 한다.)

```
  def test_hash_equality
    ht = Hash.new(data: 1)
    ht2 = Hash.new(data: 2)
    assert_true ht.eql? ht2

    assert_false 1.eql? 1.0
  end

```


## equal? (id값 비교)
객체의 id 를 비교해서 같은지 여부를 알려주는 메소드다. 
이 함수는 override하면 안된다. 내부적으로 이걸 이용해서 객체 id가 같은지를 판별하기 때문이다.
그래서 dup등으로 객체를 새로 생성해서 비교 하면 false가 나오게 된다.

```
  def test_identity_equality
    str = "a"
    dup_str = str.dup
    assert_false str.equal? (dup_str)
    assert_true str.equal?(str)
  end
```

## 참고 
* [스택오버플로우 질문](http://stackoverflow.com/questions/7156955/whats-the-difference-between-equal-eql-and)
* [루비 공식 API 문서](http://ruby-doc.org/core-2.2.3/Object.html#method-i-eql-3F)
