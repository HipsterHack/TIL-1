# Merge Sort

* Divide and Conquer 방식을 이용해 정렬한다.
* 데이터를 반으로 분할해 나누어가면서 정렬하는 방식.

![merge sort](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/300px-Merge_sort_algorithm_diagram.svg.png)


## 응용 

* fork 등을 이용해서 병렬로 mergesort를 할수도 있다.
* 이 방식을 hadoop에서 응용할 수도 있다.

## 성능

* 시간으로는 O(N LogN). 공간상으로는 O(N). 하지만 정렬 방식에 따라 다르다

## 코드 

### recursion version

```
	public static void mergesort(int[] targets, int left, int right) {
	  if ( left < hight) {
      int mid = (left + right) / 2;
      mergesort(targets, left, mid - 1);
      mergesort(targets, mid, right);
  	  merge(targets, left, mid, right);
  	}
	}

	private static void merge(int[] targets, int leftStart, int mid, int rightEnd) {
		int leftSize = mid - leftStart + 1;
		int rightSize = rightEnd - mid;

		int[] leftBuffer = new int[leftSize];
		int[] rightBuffer = new int[rightSize];

		for (int i = 0; i < leftSize; i++) {
			leftBuffer[i] = targets[leftStart + i];
		}
		for (int i = 0; i < rightSize; i++) {
			rightBuffer[i] = targets[mid + i + 1];
		}

		int leftIndex = 0;
		int rightIndex = 0;
		int offset = leftStart;
		while (leftIndex < leftSize && rightIndex < rightSize) {
			if (leftBuffer[leftIndex] <= rightBuffer[rightIndex]) {
				targets[offset] = leftBuffer[leftIndex++];
			} else {
				targets[offset] = rightBuffer[rightIndex++];
			}
			offset++;
		}

		for (int i = leftIndex; i < leftSize; i++) {
			leftBuffer[offset] = targets[i];
			offset++;
		}
		for (int i = rightIndex; i < rightSize; i++) {
			rightBuffer[offset] = targets[i];
			offset++;
		}

	}
```

### iterative version

```
	public static void mergesort(int[] targets) {

		for (int currentSize = 1; currentSize < targets.length - 1; currentSize = currentSize * 2) {
			for (int leftStart = 0; leftStart < targets.length - 1; leftStart += currentSize * 2) {
				int mid = (leftStart + currentSize) / 2;
				int rightEnd = Math.min(leftStart + currentSize * 2 - 1, targets.length - 1);

				merge(targets, leftStart, mid, rightEnd);
			}
		}
	}

	private static void merge(int[] targets, int leftStart, int mid, int rightEnd) {
		int leftSize = mid - leftStart + 1;
		int rightSize = rightEnd - mid;

		int[] leftBuffer = new int[leftSize];
		int[] rightBuffer = new int[rightSize];

		for (int i = 0; i < leftSize; i++) {
			leftBuffer[i] = targets[leftStart + i];
		}
		for (int i = 0; i < rightSize; i++) {
			rightBuffer[i] = targets[mid + i + 1];
		}

		int leftIndex = 0;
		int rightIndex = 0;
		int offset = leftStart;
		while (leftIndex < leftSize && rightIndex < rightSize) {
			if (leftBuffer[leftIndex] <= rightBuffer[rightIndex]) {
				targets[offset] = leftBuffer[leftIndex++];
			} else {
				targets[offset] = rightBuffer[rightIndex++];
			}
			offset++;
		}

		for (int i = leftIndex; i < leftSize; i++) {
			leftBuffer[offset] = targets[i];
			offset++;
		}
		for (int i = rightIndex; i < rightSize; i++) {
			rightBuffer[offset] = targets[i];
			offset++;
		}

	}
```
