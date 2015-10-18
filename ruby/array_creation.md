# [Ruby] 배열 생성방법

루비에는 여러가지 배열 생성 방법이 있다. 

먼저 기본적인 배열 생성법이다. 보통 가장 많이 사용하는 방법이다. 아래처럼 배열을 선언해준다.

```
  arr = ["a", "b", "c", "d"]
	# 빈 배열 생성
	arr2 = []
```

Array 클래스의 new를 이용해서 배열을 생성하는 방법도 있다.

```
	arr_by_new = Array.new # []
	nils_arr = Array.new(2) # [nil, nil] 
	# true 값으로 초기화한다.
	true_arrs = Array.new(2, true) # [true, true] 
```

다차원 배열을 만들고 싶다면? block을 이용해서 배열을 생성하면 된다. 그리고 각 배열을 Hash로 초기화 싶다면? 마찬가지다.
```
	arrs = Array.new(2) { Array.new(2) } # [[nil, nil],[nil,nil]]
	# arrs2 = [{}, {}]
	arrs2 = Array.new(2) do
		Hash.new
	end 
```
