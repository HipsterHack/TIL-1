# 직렬화

* 자바에서 직렬화는 객체를 byte 스트림으로 인코딩하고 다시 바이트스트림으로부터 객체로 복원하는 프레임워크를 제공하는 것이다.

## 직렬화 방법
* 간단하다.
```
implements Serializable
```

## 직렬화 구현시  고려점
* 필드가 외부 API가 된다. 즉 필드로 접근하는게 가능하다.
* 직렬화 이후 내부 구현 변경하면 변경으로 직렬화 호환이 되지 않을수도 있다.(버전 문제가 발생)
 + `ObjectOutputStream.putFields`와 `ObjectInputStream.readFields`를 사용하면 호환문제가 해결된다. (하지만 코드는 지저분해지겠지.)
 +  직렬화는 내부적으로 Stream unique identifier가 있다. 만약 method를 추가한다거나 필드를 추가하면 UID가 변경된다. 
* 보안 허점을 증대시킨다
 + 언어 영역을 벗어나는 방식으로 객체를 생성한다. 감춰진 생성자를 사용하므로 감춰진 생성자에서 불변성이 지켜지는지 확인해야한다.
* 새 버전 클래스 배포 부담이 증대된다.
 + 이진 호환성 의미적 호환성이 잘 지켜지는지 확인해야한다. 직렬화가 제대로 되는지 원본 객체의 복제본이 잘 만들어지는지 확인해야한다.
* 상속을 위해 만들어진 인터페이스나 클래스는 Serialzable을 쓸 필요가 없다. 역시 확장도 필요없다. 
* 직렬화 가능하면서 필드를 갖는 클래스를 만들땐 주의해야한다. 디폴트값으로 초기화 되는 경우 클래스의 불변성이 깨진다면 `readObjectNoData` 메소드를 반드시 추가해야한다.
* 매개변수가 없는 기본 생성자를 만드는걸 고려해야한다. 
* 내부클래스는 static 멤버가 아니면 Serializable로 구현할수 없다. 내부 클래스의 유효범위에 있는 클래스가 어떻게 대응할지 정의가 불분명하다.

## 자체 직렬화를 사용하자
* writeObject, readObject를 구현해서 자체 직렬화를 만들자.
```
void writeObject(ObjectOutputStream s) throws IOException {
  s.defaultWriteObject();
}

void readObject(ObjectInputStream s) throws IOException, ClassNotFoundException {
  s.defaultReadObject();
  ...
}
```
* 기본 직렬화는 객체의 물리적 표현과 논리적 내용이 동일할때 적합함.
* 불변 규칙 준수와 보안 안전을 위해 `readObject`메소드를 제공하자.
* 안쓰는 데이터는 `transient`키워드를 사용해서 직렬화 내용에서 빼자.
* `ObjectInputStream.defaultWriteObject()`를 호출해서 defaultWrite를 호출하자. 직렬화 형태에 영향을 주어 유연성이 좋아진다.
* 객체 전체 상태를 읽는 메소드는 동기화 처리를 해야한다. 기본 직렬화를 사용하려면 `writeObject`를 사용한다.


