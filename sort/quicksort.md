# Quicksort
* 꽤 빠른 정렬 방식.
* 데이터가 많더라도 안정적인 정렬 방식(사용하는 메모리가 그리 크지 않다)
* pivot 선택에 따라서 성능이 많이 좌지우지 된다.
  + 중앙값을 사용하는 방식을 쓰거나(아래 구현은 이것.)
  + random 값을 pivot으로 쓴다.
* pivot을 최대값이 되면 최악의 케이스가 된다. 중앙값을 선택하면 최적의 케이스

![sort image](http://www.algolist.net/img/sorts/quick-sort.png)

## 성능
* 시간 복잡도는 최적일때는 O(N Log N) 최악일 때는 O(N^2). 
* 공간 복잡도는 O(Log N)

## 코드

```
	public static void quickSort(int[] arrs, int left, int right) {
		int index = partition(arrs, left, right);

		if (left < index - 1) {
			quickSort(arrs, left, index - 1);
		}
		if (index < right) {
			quickSort(arrs, index, right);
		}
	}

	private static int partition(int[] arrs, int left, int right) {
		int pivot = arrs[(left + right) / 2];
		while (left <= right) {
			while (arrs[left] < pivot)
				left++;
			while (pivot < arrs[right])
				right--;

			if (left <= right) {
			 // swap
				int temp = arrs[left];
				arrs[left] = arrs[right];
				arrs[right] = temp;
				
				left++;
				right--;
			}
		}
		return left;
	}

```

# 참고

http://www.algolist.net/Algorithms/Sorting/Quicksort
