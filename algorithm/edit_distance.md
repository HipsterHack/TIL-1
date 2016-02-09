# Edit Distance

* string 2개의 편집 차이를 세는 것을 말한다.
* 편집은 아래기준을 바탕으로 센다.
  + 삽입
  + 대체
  + 삭제

## 예

```
Input:   str1 = "geek", str2 = "gesek"
Output:  1
str1에 s를 삽입함으로써 두개가 같음을 알 수 있다.

Input:   str1 = "cat", str2 = "cut"
Output:  1
str1에 'a'를 'u'로 대체 하면 같음을 알 수 있다.

```

## 대충 구현한 알고리즘 

m은 str1의 길이
n은 str2의 길이


1. 만약 두 스트링의 마지막 두 글자가 같으면, 마지막 문자들과 남은 문자열의 수를 무시한다.
 + m-1, n-1

2. 그게 아니면 str만 삽입, 삭제, 대체를 했을 때 Edit distance를 구한다. 재귀적으로 가장 작은 cost만 취한다.
  + insert : m, n-1
  + remove : m-1, n
  + replace : m-1, n-1

```
  public int editDistance(String str1, String str2, int m, int n){
    if ( m == 0 ) return n;
    if ( n == 0 ) return m;

    if ( str1.charAt(m - 1) == str2.charAt( n - 1 ) ){
      return editDistance(str1, str2, m - 1, n - 1);
    }

    return 1 + Math.min(editDistance(str1, str2, m, n - 1), 
                        editDistance(str1, str2, m - 1, n),
                        editDistance(str1, str2, m - 1, n - 1));
  }

```

* 시간 복잡도 O(3^m)

## Dynamic programming을 이용한 풀이법


![Subprogram](http://d1gjlxt8vb0knt.cloudfront.net//wp-content/uploads/EditDistance-1024x618.png)


위의 그래프에서 보듯이 부분 문제가 중복되는 것을 볼 수 있다. 

즉, 중복 문제 풀이를 방지하면 꽤 빠른 속도로 구할 수 있다.

```
  public int editDistance(String str1, String str2, int m, int n){
     int[][] dp = new int[m + 1][n + 1];

     for (int i = 0; i < m + 1; i++){
       for (int j = 0; j < n + 1; j++){
         if ( i == 0 ) db[i][j] = j;
         else if ( j == 0 ) db[i][j] = i;
         else if ( str1.charAt(i - 1) == str2.charAt(j - 1) ) dp[i][j] = dp[i-1][j-1];
         else dp[i][j] = 1 + Math.min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1]);
         
       }
     }
     return dp[m][n];
  }
```


* 시간복잡도: O(m x n)
* 공간복잡도: O(m x n)



