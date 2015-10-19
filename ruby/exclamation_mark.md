# [Ruby] 느낌표는?

* 느낌표(!)가 있는 메소드를 호출하면 루비에선 '호출객체를 바꾼다'는 의미이다.
* 느낌표 메소드가 있다면, 호출 객체를 안건드리고 복사본을 만드는 메소드도 있다는 의미다.

아래 예제에서 compact 함수는 array에서 nil값을 제거해주는 메소드이다.
compact는 현재 배열을 건드리지 않는다. compact!는 현재 배열을 바꿔버린다.

```
  def test_compact_series
    compactable_arr = [1, nil, 2, nil, "Data", "asset", nil]

    assert_false compactable_arr.compact.include? nil
    assert_true compactable_arr.include? nil

    assert_false compactable_arr.compact!.include? nil
    assert_false compactable_arr.include? nil
  end
```
