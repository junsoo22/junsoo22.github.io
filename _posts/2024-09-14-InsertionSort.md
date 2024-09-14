---
title: Insertion Sort
author: JS
date: 2024-09-14 21:30:00 +0900
categories: [Sorting, Algorithm, Time Complexity, Insertion]
tags: []
render_with_liquid: false
published: true
---

삽입정렬에 대해 알아보자


# 삽입 정렬이란?
정렬 범위를 1칸씩 확장해나가면서 새롭게 정렬 범위에 들어온 값을 기존 값들과 비교하여 알맞은 자리에 꼽아주는 알고리즘입니다.

- 필요할때만 위치를 바꿉니다.
- 이미 앞에 있는 것들은 정렬이 되어있는 상태입니다.

ex) List: <5,2,4,6,1,3>  
여기서 Key: 리스트의 각 숫자들('5, '2, '4)  
정렬된 리스트: <2,4,5>    
  
  만약 key=='6'이라면 5뒤에 삽입-> <2,4,5,6>  

![image](./assets/img/sort/InsertionSort.png)  
  
## code
### 파이썬 
 ```python
 def insertSort(arr):
    for end in range(1, len(arr)):
        i = end
        while i > 0 and arr[i - 1] > arr[i]:
            arr[i - 1], arr[i] = arr[i], arr[i - 1]
            i -= 1

arr=[5,2,8,6,1,9,3,7]
insertSort(arr)
print(arr)

```
출력결과: [1, 2, 3, 5, 6, 7, 8, 9]

### c언어
```cpp
#include <stdio.h>

// 함수 선언
void InsertionSort(int arr[], int n);

int main() {
	int arr[7] = { 7, 8, 5, 2, 4, 6, 3 }; // 정렬할 배열
	InsertionSort(arr, 7); // 정렬 함수 호출

	// 정렬된 배열 출력
	printf("Sorted array: \n");
	for (int i = 0; i < 7; i++) {
		printf("%d ", arr[i]);
	}
	return 0;
}

void InsertionSort(int arr[], int n) { // 배열 매개변수 수정
	int i, key, j;

	for (i = 1; i < n; i++) {
		key = arr[i]; // 현재 요소를 key로 설정
		j = i - 1;

		// key보다 큰 요소들을 오른쪽으로 이동
		while (j >= 0 && arr[j] > key) {
			arr[j + 1] = arr[j];
			j = j - 1;
		}
		arr[j + 1] = key; // key를 올바른 위치에 삽입
	}
}
```
출력결과: [2, 3, 4, 5, 6, 7, 8]
  
## Running Time
- pseudo code
  - ![image](./assets/img/sort/pseudocode.png)  
  - Cost: 각 절차에 필요한 명령어 수  
  Times: 각 procedure가 수행되는 횟수  
  tj: while loop가 실행되는 횟수  
  for문과 while문의 test부분은 루프문보다 한 번 더 실행된다.  
  ![image](./assets/img/sort/time.png) 
   
  ### Best case 
  만약 리스트 A[1...n]이 이미 정렬되어있다면, tj는 항상 1이다.  
    ![image](./assets/img/sort/best.png)  
    그럼 an+b 꼴이 나오므로 일차함수가 된다.
  
  ### Worst case
  만약 리스트 A[1..n]이 완전 역전된 순서로 되어있다면 tj는 항상 j이다.   
   ![image](./assets/img/sort/worst.png)  
   그럼 an^2+bn+c가 되므로 이차함수가 된다.

Best case: an+b  
Worst case: an^2+bn+c  
