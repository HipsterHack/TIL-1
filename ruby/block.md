## [Ruby] block 

Block에 대해 설명해보겠다. 
블록은 일종의 클로저라고 할 수 있다. 
실행할 내용을 하나의 함수로 묶고, 이를 객체로 만들어 원하는 시점에 호출할 수 있다. 이것이 바로 블록이다.  

레고 장난감 블럭처럼 여기저기 쉽게 가져다 낌을 수 있게 만들고자 블럭이 아닐까 싶다. 

블럭을 호출할땐 여러 방법이 있다.  루비에서의 블록은 1줄 식으로 표현할때 {} 을 사용하고, 두줄 이상일때는 do end를 사용한다.

예를 들자면 다음과 같다.

	# 1줄 짜리 {} 
	counter = 0
	4.times {  counter += 1 }
	
	# 1줄 이상 do-end
	Application::Base.transaction do 
	  # do transaction
	end 

그럼 이제 블록의 정의는 어떻게 해야하는걸까?
크게 두가지 형태가 있다. 
1줄짜리 블록 형태로 정의 하는 방법이다. 

	# Proc.new
    block = Proc.new { |x| x + 1 }
	# proc
    block = proc { |x| x + 1 }
	# lambda
    block = lambda { |x| x + 1 }
	# -> 표기법
    block = ->(x) { x + 1 }

함수 형태의 블록으로 정의 하는 방법이다.

	# n번 실행. 
	# test_times(3) { puts "test" }
	# 위 예제는 화면에 3번 test를 찍는다.
	def test_times(n)
	  if block_given?
		n.times{ yield } 
	  end 
	end
	
	# 같은 기능의 메소드이지만 꼭 블록이 있어야 한다.
	def test_times(n, &block)
		n.times{ block.call } 
	end 


첫번째 예제는 블록을 생략하는 형태이다. 마지막 파라미터로 블록을 전달한다고 가정한다.(전달하지 않는 경우도 있다고 생각한다) 

두번째 예제는 아예 명시적으로 파라미터로 선언 하는 방법이다.

이걸 사용한 덕분에 오늘 cache 적용을 손쉽게 할 수 있었다. 

오늘은 이상이다. :) 
