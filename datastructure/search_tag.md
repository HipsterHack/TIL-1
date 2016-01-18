# 문제풀이

 태그를 이용한 특정 단어들을 검색하는 기능을 만드려고 한다.
예를 들면, 
```
"구글" : "안드로이드", "검색", "모바일"
"애플" : "iOS", "모바일", "맥북"

input1 : "iOS", "모바일"
output1 : "애플"
input2 : "모바일"
output2 : "구글", "애플"
```
어떤 자료구조를 사용할건지는 마음대로 이고, 예제와 같이 입력과 출력 결과 갯수는 단수개도, 복수개도 될 수 있습니다.

## 풀이

```
package vector;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class WordFinder {
	Map<String, List<String>> map = new HashMap<String, List<String>>();
	void addTag(String word, String[] tags){
		if ( tags == null || tags.length == 0 ){
			return ;
		}
		for (String tag : tags){
			List<String> l = null;
			if ( map.containsKey(tag)){
				l = map.get(tag);
				if ( l == null){
					l = new ArrayList<String>();
				}
			}
			else{
				l = new ArrayList<String>();
				map.put(tag, l);
			}
			l.add(word);
		}
	}

	Set<String> findByTag(String[] list){
		Set<String> result = new HashSet<String>();
		if (list == null || list.length == 0 )
			return result;
		for(int i = 0 ; i < list.length; i++){
			String input = list[i];
			if ( !map.containsKey(input))
				continue;
			List<String> matched = map.get(input);
			if (result.size() == 0){
				result.addAll(matched);
			} else if (matched != null){
				result.retainAll(matched);
			}
		}

		return result;
	}
	public static void main(String[] args){
		WordFinder finder = new WordFinder();
		finder.addTag("google", new String[] { "android", "search", "mobile" } );
		finder.addTag("apple", new String[] {"iOS", "mobile", "macbook"} );
		System.out.println(finder.findByTag(new String[]{"iOS", "mobile"}));
		System.out.println(finder.findByTag(new String[]{"mobile"}));
		System.out.println(finder.findByTag(new String[]{"pc", "iOS"}));
	}
}

```
