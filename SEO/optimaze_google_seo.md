# 구글 SEO 

구글 SEO는 구글에서 검색을 노출하기 쉽게 URL과 페이지에 메타 정보를 넣어주는 작업이다.

## URL 에 제목을 넣은 permanent 링크를 만든다.
* 아래 예제처럼 추가한다. 
```
http://localhost/blog/seo+최적화
```

##  canonical URL을 이용한다.
head 태그에 link를 추가한다.
```
<head>
...
<link rel="canonical" href="http://www.example.com/product.php?item=swedish-fish" />
...
</head>
```

## pagination
* prev나 next URL을 헤더에 link로 넣어준다. 
이러면 크롤러가 다음 페이지로 인식해서 크롤링을 해간다. 
```
<link rel="prev" href="http://www.example.com/article?story=abc&page=2" />
<link rel="next" href="http://www.example.com/article?story=abc&page=4" /
```

## HTTPS사용

## 표준 URL 이 아니면 응답값을 301 로 처리

# 참고
[https://googlewebmastercentral.blogspot.kr/2011/09/pagination-with-relnext-and-relprev.html] 
[https://googlewebmastercentral.blogspot.kr/2009/02/specify-your-canonical.html] 
한글버전 SEO 최적화 [https://support.google.com/webmasters/answer/139066?hl=ko]
