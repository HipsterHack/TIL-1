## Rails Cache 
* 이 내용은 http://guides.rubyonrails.org/caching_with_rails.html 페이지 내용 정리 
* 4 기준 레일즈 제공 캐시는  fragment cache, low level cache, SQL cache 이다.
* 4 이전에 제공한 page, action 캐시는 레일즈에서 제거 당했다. 

###  Page Cache 
* 가장 단순한 캐시.
*  contoller의 action 결과를 통째로 저장한다.
*  예전의 php 에서 html 파일로 떨궈서 사용하는 것을 젬이 대신 해준다.
** 그 때문에 아래처럼 사용한다

```
 class WeblogController < ActionController::Base
   caches_page :show, :new
 
   def update
     List.update(params[:list][:id], params[:list])
     expire_page action: 'show', id: params[:list][:id]
     redirect_to action: 'show', id: params[:list][:id]
   end
 end
```

### Action Cache 
* before_filter가 실행되서 cache를 적용할지 말지를 결정한다.
* 그리고 그 결과에 따라서 action의 캐싱한 결과를 보여줄지 말지 결정한다.
* filter 에서 인증 여부에 따라 페이지를 해당 보여줄지 말지 결정 해야 한다면 action 캐시를 사용해야 한다.
* 사용 예
```
class ListsController < ApplicationController
  before_filter :authenticate, except: :public

  caches_page :public

  caches_action :index, if: Proc.new do
    !request.format.json?  # cache if is not a JSON request
  end

  caches_action :show, cache_path: { project: 1 },
    expires_in: 1.hour

  caches_action :feed, cache_path: Proc.new do
    if params[:user_id]
      user_list_url(params[:user_id], params[:id])
    else
      list_url(params[:id])
    end
  end
end
```

### Fragment Cache
* erb 랜더링 결과를 캐시한다.
* erb태그 처럼 사용 가능하다.
* 사용 예는 아래와 같다.
<pre>
<% cache('all_available_products') do %>
  All available products:
<% end %>
</pre>


### SQL Cache

* 모든 쿼리는 request 단위로 SQL질의 결과를 서버 메모리에서 캐싱을 하고 있다. 
* hibernate 같은 ORM이 하는 기능을 기본적으로 하고 있다.


### low level cache

Rails.cache를 이용해서 코딩한다.
아래와 같이 cache를 작성하면 마지막에 호출한 return 한 값은 cache로 들어간다.
(굳이 write를 쓸 필요가 없다. 코딩시 참고하자)

```
 result = Rails.cache.fetch(cache_key, expires_in: 1.hour) do
  Article.all
 end
```
 
 
### 참고 
* [page cache](https://github.com/rails/actionpack-page_caching)
* [action cache](https://github.com/rails/actionpack-action_caching)
* [SQL 캐시](http://guides.rubyonrails.org/caching_with_rails.html#sql-caching)
