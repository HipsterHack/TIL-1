# 자바를 이용한 함수형 프로그래밍 예제

## 모음의 숫자 세기 

```
	 public static int getCount(String str) {
	    return (int)str.chars().filter(c -> "aeiou".indexOf(c) != -1).count();
	  }    
```
* filter 를 이용해서 필요한 것만 남겨두고 개수를 센다. 
