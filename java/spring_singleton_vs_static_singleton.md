
# Spring singleton vs static singleton

* 두 싱글톤은 인스턴스 생명주기가 다르다.
 + static 싱글톤은 classloader 사이에 공유한다
 + spring 싱글톤은 ApplicationContext 기준이다.

 ## static 싱글톤

 JVM이 실행되면 여러개의 클래스로더로 클래스를 로딩한다. 
 예를 들면 톰캣은 Bootstrap - System - Common - Webapp1, webapp2 ... 
 이런 식으로 클래스 로더를 관리한다.
 즉, war파일 단위로 클래스를 관리한단 것. 즉, war 파일이 다르면 같은 클래스더라도 참조할 수 없다.(static도 마찬가지)
 대신 상위 클래스로더의 클래스는 참조가 가능하다.

 ## spring 싱글톤

Spring은 DispatcherServlet이라는 Servlet을 사용한다. 
즉 WEB-INF 아래에 web.xml 로 DispatcherServlet을 여러개 등록하면 독립된 context가 생긴다.
이 context안에서 Spring의 Singleton은 하나의 Spring IoC Container 안에서 하나만 생성되고 공유된다.
즉 spring singleton은 등록한 DispatcherServlet 기준으로 서로 독립적이다.  


 # 참고 자료
 [http://blog.daum.net/rollin/8097082]
 [http://stackoverflow.com/questions/5132604/why-use-spring-applicationcontext-hierarchies]
