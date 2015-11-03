## [Rails] mini test를 사용한 모델 테스트 팁

### mini 테스트는 Fixture를 사용한다.

(즉 DB를 이용해서 테스트를 하는데 test case마다 한번 전체 테이블의 데이터를 지우고 Fixture를 한번 로딩하는 반복을 이용한다.)

### mini test에서는 test case 마다 transaction의 begin, rollback 을 사용한다.


대부분 DB(MS SQL 빼고)에서 중첩 transaction을  지원하지 않으므로 테스트 케이스 테스트 중에 오류가 발생할 수 있다.

트랜잭션이 걸려 있는 경우, 

1. transaction을 스킵하는 기능을 만듬
2. [AOP를 적용](http://blog.arkency.com/2013/07/ruby-and-aop-decouple-your-code-even-more/) 
3. transaction을 사용하지 않는다.
4. helper class를 통해서 테스트 범위 밖으로 밀어버린다.

위의 4가지 방법을 이용해서 트랜잭션 중첩을 회피 해야한다.

### Fixture가 싫다면, 일종의 mock를 만들어주는 [FactoryGirl](https://github.com/thoughtbot/factory_girl)를 쓰자.)
### fixture yml에 id를 지정안하면 mini test가 알아서 id를 만들어버린다. (대신 유니크는 보장한다)
### fixture yml에 null로 설정하면 빈값(nil)이 들어간다

### Fixture 만드는 법

Fixture를 이용해서 테스트를 할때 주의점이 한가지 있다. Fixture는 개수가 많아지면 속도가 느려진다. 현재 DB의 데이터를 모두 지웠다가 Fixture 데이터를 넣고 테스트가 완료 되면 롤백한다. 이 과정을 진행하다보니 기존에 데이터가 많거나 Fixture 수가 늘어나면 늘어날수록 느려진다. 

이걸 해결하려면 test_helper.rb에서 

	class ActiveSupport::TestCase
	  fixtures :all

이 부분을 수정하는 것이 좋다. 

각 테스트 케이스 setup 에서 fixture를 로딩하도록 만들거나 헬퍼를 통해서 필요한 것만 로딩하는 걸 만들어주도록 한다.


Fixture는 YML 형태로 보통 사용한다
아래는 예제다.

	# users.yml
	daniel:
	  name: Daniel
	  job: programmer
	  workplace : google
	son:
	  name: son
	  job: judgement
	  workplace: unknown


테스트에서는 아래와 같이 사용할 수 있다. users.yml이면 .yml 만 뺀 형태 메소드를 레일즈에서 제공한다.

	class UserTest < ActiveSupport::TestCase
	  test "test user" do 
	    assert_equal "Daniel", users(:daniel)
	  end
	end

동적인 데이터도 만들어줄 수 있는데 ERB 형태로 fixture 데이터를 생성할 수 있다. 


	<% 1.upto(100) do |idx| %>
	fix_<%= idx %>:
	  id: <%= idx %>
	  name: person_<%= idx %>
	  created_at: <%= DateTime.now %>
	  option: nil
	<% end %>

100개의 fixture 데이터를 만들었다. (option은 nil값이 들어가고 created_at은 현재 시간이 지정된다.

fixture로 belongs_to, has_one, has_many 관계가 있다고 생각하자. 레일즈는 fixture에 정의한 label을 이용해서 연결해준다.
아래는 예다

	# class User < ActiveRecord::Base 내부 선언
		belongs_to :job

	# class Job < ActiveRecord::Base 내부 선언 
		has_many :users

	# users.yml
	daniel:
	  name: Daniel
	  job: programmer
	  workplace : google
	kim:
	  name: Kim
	  job: programmer
	  workplace : google
	son:
	  name: son
	  job: judgement
	  workplace: unknown

	# jobs.yml
	programmer:
	  name: Programmer
	judgement:
	  name: Judgement

user 클래스 객체에서 job에 접근을 할 수 있다. users 테이블에서 보면 job_id 가 정의되어 있는데 job_id는 jobs fixture에 들어간 id나 레일즈가 자동으로 생성한 id가 들어가는걸 볼 수 있다. 자동으로 모든 세팅을 레일즈가 해준다. 

모델에 관계 설정은 없고, id 값을 가져다 써야할 땐 어떻게 할까? 이럴땐 ActiveRecord::FixtureSet.identify() 를 사용할 수 있다. Fixture의 label로 레일즈가 어떤 id를 넣었는지 알수 있다.
아래 처럼 사용하면 된다.

	# users.yml
	daniel:
	  name: Daniel
	  job_id: <%= ActiveRecord::FixtureSet.identify(:programmer)
	  workplace : google
	kim:
	  name: Kim
	  job_id: <%= ActiveRecord::FixtureSet.identify(:programmer)
	  workplace : google
	son:
	  name: son
	  job_id: <%= ActiveRecord::FixtureSet.identify(:judgement)
	  workplace: unknown

	# jobs.yml
	programmer:
	  name: Programmer
	judgement:
	  name: Judgement


### 참고자료

* [효과적인 fixture 사용법](http://blog.bigbinary.com/2014/09/21/tricks-and-tips-for-using-fixtures-in-rails.html)
* [AOP for ruby](http://blog.arkency.com/2013/07/ruby-and-aop-decouple-your-code-even-more/)
* [FactoryGirl](https://whatdoitest.com/getting-friendly-with-fixtures)
* [Fixtures API 문서](http://api.rubyonrails.org/v3.2.8/classes/ActiveRecord/Fixtures.html) : 최신버전엔 사용 안내서가 없다.
