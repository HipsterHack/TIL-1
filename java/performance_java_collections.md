# 자바 콜렉션 성능표

아래는 자바 collection API의 성능표이다.

* List 와 Set

|Structure|get|add|remove|contains|
|---------|---|---|------|--------|
|ArrayList|O(1)|O(1)|O(n)|O(n)|
|LinkedList|O(n)|O(1)|O(1)|O(n)|
|HashSet|O(1)|O(1)|O(1)|O(1)|
|LinkedHashSet|O(1)|O(1)|O(1)|O(1)|
|TreeSet|O(log n)|O(log n)|O(log n)|O(log n)|


* Maps

|Structure|get|add|remove|contains|
|---------|---|---|------|--------|
|HashMap|O(1)|O(1)|O(1)|O(1)|
|LinkedHashMap|O(1)|O(1)|O(1)|O(1)|
|TreeMap|O(log n)|O(log n)|O(log n)|O(log n)|



## 참고
(http://www.javacodegeeks.com/2011/04/simple-big-o-notation-post.html)
