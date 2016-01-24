# static lock vs non-static lock


static synchronized는  선언한 클래스에서 lock을 공유한다.
non-static synchronized는 인스턴스 자체에서만 lock을 공유한다.

아래는 예제코드다 

예) 

```
public class MyClass1 {
  private static final Object lock = new Object();
  public MyClass1() {
    //unsync
    synchronized(lock) {
      //sync
    }
    //unsync
  }
}
```


```
public class MyClass2 {
  private final Object lock = new Object();
  public MyClass2() {
    //unsync
    synchronized(lock) {
      //sync
    }
    //unsync
  }
}
```

# 참고 

http://stackoverflow.com/questions/18356795/static-versus-non-static-lock-object-in-synchronized-block
