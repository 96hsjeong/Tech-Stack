# 버블 정렬 (Bubble Sort)
*written by sohyeon, hyemin 💡*

<br>

## 1. 버블 정렬이란?

버블 정렬은 이웃한 두 요소의 대소 관계를 비교하여 교환을 반복하는 정렬이다.

## 2. 동작 방식

<img src="./resources/BubbleSort.jpg" width="500px">

끝에 두 요소부터 시작해서 값을 비교하고 교환이 필요하면 교환하는 과정을 반복한다.  
n-1회 비교, 교환을 하고 나면 가장 작은 요소가 맨 앞으로 이동한다.  
이 과정을 `pass`라고한다.  

`pass`과정이 n-1회 반복되게 되며,  
각 `pass`의 비교,교환 과정은 n-1, n-2, n-3, ... , 1 이렇게 1회씩 줄어든다.  

비교 교환 과정의 합은 아래와 같다.  

```
(n-1)+(n-2)+(n-3)+ ... + 1 = n(n-1)/2
```

## 3. 특징

### 1) 장점

- 구현이 매우 간단하다.

### 2) 단점

- 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다.

- 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다.

- 일반적으로 자료의 교환 작업(SWAP)이 자료의 이동 작업(MOVE)보다 더 복잡하기 때문에  
  버블 정렬은 단순성에도 불구하고 거의 쓰이지 않는다.

## 4. 시간복잡도

- 비교 횟수
    * 최상, 평균, 최악 모두 일정
      : n-1, n-2, … , 2, 1 번 = n(n-1)/2

- 교환 횟수
    * 입력 자료가 역순으로 정렬되어 있는 최악의 경우,  
      한 번 교환하기 위하여 3번의 이동(SWAP 함수의 작업)이 필요하므로  
      
      -> (비교 횟수 * 3) 번 = 3n(n-1)/2
    
    * 입력 자료가 이미 정렬되어 있는 최상의 경우,  
      자료의 이동이 발생하지 않는다.  

```
T(n) = O(n^2)
```

## 5. 예제

### 1) 기본적인 버블 정렬

```Java
import java.util.Scanner;

class BubbleSort {
	// a[idx1]와 a[idx2]의 값을 바꿉니다. 
	static void swap(int[] a, int idx1, int idx2) {
		int t = a[idx1]; 
		a[idx1] = a[idx2]; 
		a[idx2] = t;
	}

	// 버블 정렬
	static void bubbleSort(int[] a, int n) {
		for (int i = 0; i < n - 1; i++)
			for (int j = n - 1; j > i; j--)
				if (a[j - 1] > a[j])
					swap(a, j - 1, j);
	}

	public static void main(String[] args) {
		Scanner stdIn = new Scanner(System.in);

		System.out.println("버블 정렬(버전 1)");
		System.out.print("요솟수：");
		int nx = stdIn.nextInt();
		int[] x = new int[nx];

		for (int i = 0; i < nx; i++) {
			System.out.print("x[" + i + "]：");
			x[i] = stdIn.nextInt();
		}

		bubbleSort(x, nx);				// 배열 x를 버블 정렬합니다.

		System.out.println("오름차순으로 정렬했습니다.");
		for (int i = 0; i < nx; i++)
			System.out.println("x[" + i + "]＝" + x[i]);
	}
}
```
위의 과정은 정직하게 알고리즘 과정을 반영한 코드이다.  

하지만 실제 입력이 적용된 알고리즘 과정을 살펴보면 모든 pass를 n-1회 반복할 필요가 없다.  

일정 pass에서 교환없이 비교과정만 이루어지는 경우가 존재하는데  
그 상황은 정렬이 완료되었다는 소리와 같다.  

교환 횟수를 기록하고 확인하는 조건이 추가된다면 불필요한 반복을 줄일 수 있다.  

### 2) 개선된 코드 1

```Java
static void bubbleSort(int[] a, int n){
    for(int i=0; i<n-1; i++){
        int exchg = 0;  // 교환 횟수 기록
        for(int j=n-1; j>i; j--){
            if(a[j-1]>a[j]){
                swqp(a, j-1, j);
                exchg++;
            }
        }
        if(exchg == 0) break;   // 교환이 이루어지지 않으면 종료
    }
}
```
