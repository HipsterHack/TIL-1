## Active Recrod Scope


Active record에는 scope라는 개념이 있다.

Active record로 DB에서 데이터를 조회할 때 조회 범위를 줄여준다거나 필터링 할때 주로 사용한다.

예를 들면, 블로그에서 발행된 포스트만 조회한다는 의미를 담고 있을때,


	Post.published

위와 같이 조회하도록 한다.

자 여기서 분명한 특징을 생각해보자.

조회 한걸 봤을때 클래스 메소드가 아닐까?

일종의 클래스 메소드가 맞다. 다만 클래스 메소드처럼 클래스가 로드 되는 시점에 블록을 평가하는 형태가 하나, 호출되는 시점에 블록을 평가하는 형태 두가지가 있다.


	class Member < ActiveRecord::Base
		scope :leaved, where (member_type: 'LEAVED')
		scope :basic, -> { where(member_type: 'BASIC' ) }
		...
	end

scope의 :leaved는 클래스가 로드 되는 시점에 평가된다. bascic의 경우엔 이 scope이 호출되는 시점에 :basic scope을 평가하는 방식을 사용한다.
  
Rails 4부터는 :leaved의 형태는 사용을 권장하지 않는다.

scope은 메소드에 비해 가벼운(?) 사용이 가능하다. 블록에 수행할 내용이 없으면 예외 발생 없이 다음 호출 포인트로 넘어간다. (클래스 메소드의 경우엔 예외가 발생하기 쉽다.)


	scope: by_member_type, -> member_type { where(member_type: member_type) if member_type.present? }


요렇게 사용하면

	# 	Member.logged_in 와 같은 결과
	Member.by_member_type(nil).logged_in
	

요런식으로 호출했을때 by_member_type을 호출 하지 않은 구문과 같다.



### 참고 
[Active Record scope vs class method](http://blog.plataformatec.com.br/2013/02/active-record-scopes-vs-class-methods/)

* 이거 보고 정리해서 그런지 차이가 안난다.-_-;
