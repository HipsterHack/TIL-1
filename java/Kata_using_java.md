# Kata using Java

# 카타1
* 엑셀 컬럼을 숫자로 나타내는 메소드를 만들어라.
* "A" 는 1, "AA"는 27.. 이런 형태로 만들면 된다.
* 아래는 구현코드

```
public class TitleToNumber {
    private static final int COLUMN_SIZE = Character.getNumericValue('Z') - Character.getNumericValue('A') + 1;
    public static long titleToNumber(String title) {       
        if (title == null || title.length() == 0){
          return -1L;
        }
        
        if ( title.matches("[^A-Z]+") ){
          return -1L;
        }
        
        char[] titleChars = title.toCharArray();
        long position = 0L;
        int titleLength = titleChars.length;

        for (int i = 0; i < titleLength; i++) {
          char ch = titleChars[i];
          position += calculateExcelCellNumber(ch, i, titleLength);
        }
        
        return position;
    }
    
    private static long calculateExcelCellNumber(char ch, int index, int length){
      return (long)Math.pow(COLUMN_SIZE, (length - index - 1)) * (long)distanceFromA(ch);
    }
    
    private static int distanceFromA(char ch){
      return Character.getNumericValue(ch) - Character.getNumericValue('A') + 1;
    }
}
```

# 카타2
* 원의 넓이를 구하여라.

```
public class Circle {
  public static double area(double radius) {
    if ( radius <= 0.0D )
      throw new IllegalArgumentException();
      
    return Math.PI * Math.pow(radius, 2);
  }
}
```

# 카타3
* Functional Programming 익히기
* 
```
import java.util.function.Function;

public class FunctionalProgramming {
  public static Function<Student, Boolean> f = p -> "John Smith".equals(p.getFullName()) && "js123".equals(p.studentNumber);

}
```

# 카타4

* `array1`, `array2`를 파라미터로 받는다.
* `array1`의 요소가 `array2`의 요소의 부분 문자열만 남긴다.
* 결과 값은 정렬된 array를 돌려준다. 

```
import java.util.HashSet;
import java.util.Arrays;

public class WhichAreIn { 
	
	public static String[] inArray(String[] array1, String[] array2) {
			HashSet<String> set = new HashSet<String>();
			for (int i = 0; i < array1.length; i++) {
				for (int j = 0; j < array2.length; j++) {
					if (array2[j].contains(array1[i])) {
						set.add(array1[i]);
					}
				}
			}
			String[] result = set.toArray(new String[set.size()]);
			Arrays.sort(result);
			return result;
	}
}
```

# 카타5
* 거북이 A, B가 경주를 한다.  A가 B보다 앞서 있고, B 속도가 A속도보다 빠르다. B가 A를 언제쯤 따라잡을 수 있는가?
* 예를 들면 아래와 같다.
* 거북이 A가 1시간마다 720 feet 간다
* 거북이 B는 1시간마다 850 feet 간다
* 거북이 A는 B보다 70 feet 앞서 있다.
* 자 그럼 몇시간 몇분 몇초 만에 거북이 B가 따라잡는가?

## 풀이
* `속력 * 시간 = 거리` 공식을 이용한다.

```
public class Tortoise {
    public static final int HOUR_SECOND = 3600;
    public static final int MINUTE_SECOND = 60;
    
    public static int[] race(int v1, int v2, int g) {
        if ( v1 >= v2 ) return null;
        
        float sec = g * HOUR_SECOND / (v2 - v1);
        return new int[] { 
          (int)(sec / HOUR_SECOND), 
          (int)((sec % HOUR_SECOND) / MINUTE_SECOND), 
          (int)((sec % HOUR_SECOND) % MINUTE_SECOND)  
          };
    }
}
```

