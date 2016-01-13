# JSONP

- 웹 페이지에서 다른 호스트 웹페이지로 데이터를 요청할 때 사용하는 방법이다.
- 동일 출처 원칙을 어기고 부득이하게 데이터를 주고 받을 때 이 방식을 사용한다.
- Jquery가 참 잘 지원하기 때문에, 원리를 잊는 사람들이 종종 있다. 

## 원리 
원리는 간단하다. script 요소는 타 도메인에서 데이터를 가져올 수 있다.
그 때문에 다른 도메인에서 데이터를 주고 받을 때 사용한다.

## 한계
* Get Method 만 사용해서 호출할 수 있다. `<script>` 요소를 사용하기 때문이다.

## 예제 

### Pure Javascript

```
function drawList()
{
    var js = document.createElement('script');
    var  = encodeURIComponent(document.getElementById('str').value);
    var url = "${요청 URL}";
  
    js.setAttribute('src', url);
    js.setAttribute('type', 'text/javascript');
    document.getElementsByTagName('body')[0].appendChild(js);
 
    return false;
}
```

* `createElement`를 통해서 script 요소를 만든다.
* 이후 로드가 완료되면 js 수행이 된다. 

### In JQuery

```
function drawList()
{  
    var s = encodeURIComponent(document.getElementById('str').value);
     
    $.ajax({
        dataType: "jsonp",
        url: "${요청 URL}",
        type: "GET",
        data: {'param': 'value'},
        success: function(data){
            loadList(data);
        }
    });
  
    return false;
}
```
* ajax 호출과 큰 차이가 없다.
