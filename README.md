# Sorting-Algorithm


6가지의 정렬 알고리즘에 대해서 각각 간단한 개념을 이야기 한 후,<br>
정방향 데이터 / 역방향 데이터 / 랜덤 데이터 3가지 경우에 대해서 수행시간을 비교해 볼 것이다. <br>
그리고 마지막으로 데이터가 랜덤으로 주어지는 경우 6가지 알고리즘의 수행시간을 그래프를 통해 비교해 볼 것이다.<br>

데이터 개수에 따른 각 정렬 알고리즘의 수행시간을 측정한 주요코드는 다음과 같다.<br>
(수 많은 블로그와 깃허브 , 인터넷 정보들을 참고하였다. )
```c
#define MAX_SIZE 32 //데이터의 개수 지정
#define SWAP(x,y,t) ((t)=(x), (x)=(y), (y)=(t))    
int original[MAX_SIZE];    //랜덤함수로 만든 데이터를 저장할 원본 배열
int list[MAX_SIZE];    //각 정렬 알고리즘에서 사용할 데이터 배열
int n; //데이터의 개수를 받는 전역변수 설정
clock_t start, finish, used_time=0;    //실행 시간 측정을 위한 변수
```
MAX_SIZE 의 값을 수정하면서 데이터의 개수를 계속 변경해 줄 수 있다.<br>
그 후 , 6가지 정렬 알고리즘을 구현해주고 ( 이 코드는 생략한다 / 밑에 장에서 작성하긴 할 것이다.) <br>

```c
//실행 시간을 측정 및 출력
void CalcTime(void)
{
    used_time=finish-start;
    printf("완료 소요 시간은 %f sec\n \n ",(float)used_time/CLOCKS_PER_SEC);
}
```
각 정렬 알고리즘의 수행시간을 측정하여 나타내 주는 함수를 작성한다.

```c
void main ()
{
    int i;
    
    n=MAX_SIZE;
    for(i=0; i<n; i++)
        original[i]=rand();
    
    printf("데이터의 개수 : %d\n\n", n);
 
    CopyArr();
    start=clock();
    selection_sort(list, n);
    finish=clock();
    CalcTime();
    
    CopyArr();
    start=clock();
    insertion_sort(list, n);
    finish=clock();
    CalcTime();
 
    CopyArr();
    start=clock();
    bubble_sort(list, n);
    finish=clock();
    CalcTime();
 
    CopyArr();
    start=clock();
    shell_sort(list, n);
    finish=clock();
    CalcTime();
 
    CopyArr();
    start=clock();
    printf("합병 정렬 중... ");
    merge_sort(list, 0, n);
    finish=clock();
    CalcTime();
 
    CopyArr();
    start=clock();
    printf("퀵 정렬 중... ");
    quick_sort(list, 0, n);
    finish=clock();
    CalcTime();
}
```
그 후 정렬 알고리즘을 수행 후 , 수행시간 측정 함수를 호출하는 main 문을 작성한다.<br>

![컴파일 결과](https://user-images.githubusercontent.com/101388379/166856559-8af7885c-2402-433e-b01e-ceadbfcde24e.PNG)

위에 보이는 것처럼
이 함수를 통해 우리는 데이터 수를 입력하면 , 그 수를 처리하는데 걸리는 정렬 알고리즘들의 수행 시간을 알 수 있다.
이를 통해 역방향 데이터 / 정방향 데이터 / 랜덤 데이터 / 3가지 경우에 대해서도 알고리즘들의 수행시간을 알아볼 것이다.
<br>
위 함수는 랜덤으로 데이터가 생성되는 경우이다.<br>

만약 역방향 데이터를 정렬하려면 어떻게 해야할까? <br>
랜덤으로 생성된 데이터를 역방향으로 정렬 후 , 다시 정렬 알고리즘에 넣어주면 된다.<br>
그러므로 아래와 같이 MAX_SIZE 만큼의 데이터들을 역순배열 해주는 함수를 추가한다.
```c
void reverseArraylnt(int* array, int size) {
  int temp;

  for(int i = 0; i<MAX_SIZE / 2; i++) {
    temp = array[i];
    array[i] = array[(MAX_SIZE - 1) - i];
    array[(MAX_SIZE - 1) - i] = temp;
  }
  ```
  정렬된 데이터의 경우에는 , 우리가 공부하고 있는 정렬알고리즘이 결국 오름차순으로 정렬 하는 것이므로<br>
  6가지의 정렬 중 , 하나를 골라서 MAX_SIZE 수 만큼의 데이터들을 정렬 해 준 다음에 정렬된 배열을 한번 더 정렬 알고리즘으로 돌린다고 생각했다.
  
  이제 우리는 코드를 통해 랜덤 데이터 , 역정렬 데이터 , 정렬 데이터 3가지 경우에 수행 시간을 구하는 방법을 알았다. 이제 본격적으로 값을 측정하고 그래프를 그려보자. 데이터의 입력 수는 2의 5승부터 2의 20승까지로 지정해놓았다.


## 선택정렬

### 선택 정렬 알고리즘 기본 개념
**● 입력 배열(정렬되지 않은 값들) 이외에는 다른 추가 메모리를 요구하지 않는 알고리즘<br>**
**● 원소를 넣을 위치가 정해져 있으며 , 어떤 원소를 넣을지 선택하는 알고리즘<br>**
1. 첫 번째 위치에는 가장 최솟값을 넣는다.
2. 두 번째 위치에는 그 다음의 최솟값을 넣는다. <br>

**● 과정**
1. 주어진 배열에서 가장 최솟값을 찾는다.
2. 그 최솟값을 맨 앞에 위치한 값과 교체.
3. 그 다음 위치도 1,2 번의 방법으로 계속 교체해나간다.
4. 하나의 원소가 남을 때까지 반복한다.


**● 그림으로 보는 선택 정렬 알고리즘의 예제**

![캡처](https://user-images.githubusercontent.com/101388379/166851041-2344b236-708e-4541-9c50-700b699308e4.PNG)

**● 선택정렬 기본코드**
```c

void selection_sort(int list[], int n){
  int i, j, least, temp;

  for(i=0; i<n-1; i++){
    least = i;

 
    for(j=i+1; j<n; j++){
      if(list[j]<list[least])
        least = j;
    }

   
    if(i != least){
        SWAP(list[i], list[least], temp);
    }
  }
}```

void main(){
  int i;
  int n = MAX_SIZE;
  int list[n] = {9, 6, 7, 3, 5};

  // 선택 정렬 수행
  selection_sort(list, n);

  // 정렬 결과 출력
  for(i=0; i<n; i++){
    printf("%d\n", list[i]);
  }
}
```
이제 맨 처음에 소개한 알고리즘을 통해 랜덤데이터  , 역정렬 데이터 , 정렬 데이터의 소요시간을 살펴보고 선택정렬은 어느 경우에 가장 유리할지 살펴보자.

![선택정렬 그래프](https://user-images.githubusercontent.com/101388379/166864239-a2688e76-07f5-48da-baef-341464543cb3.PNG)


---

## 삽입정렬

### 삽입정렬 알고리즘 기본개념
**●배열의 모든 요소를 앞에서부터 이미 정렬된 배열 부분과 비교하여 , 자신의 위치를 찾아 삽입함으로 정렬을 완성하는 알고리즘.<br>**
**●매 순서마다 원소를 삽입할 수 있는 위치를 찾아 해당 위치에 넣는 알고리즘.**<br>

**●그림으로 보는 삽입정렬 알고리즘 예제**<br>

![삽입정렬](https://user-images.githubusercontent.com/101388379/166852501-19688ee5-b9d7-4c69-9404-996752134ae1.PNG)

**●삽입정렬 기본 코드** 
```c

void insertion_sort(int list[], int n){
  int i, j, key;

  for(i=1; i<n; i++){
    key = list[i]; 

    for(j=i-1; j>=0 && list[j]>key; j--){
      list[j+1] = list[j]; // 레코드의 오른쪽으로 이동
    }

    list[j+1] = key;
  }
}

void main(){
  int i;
  int n = MAX_SIZE;
  int list[n] = {8, 5, 6, 2, 4};

  // 삽입 정렬 수행
  insertion_sort(list, n);

  // 정렬 결과 출력
  for(i=0; i<n; i++){
    printf("%d\n", list[i]);
  }
}
```

---

## 버블정렬

### 버블정렬 알고리즘 기본개념
**●서로 인접한 두 원소를 검사 후 정렬하는 알고리즘**
**●인접한 2개의 값을 비교하여 크기가 순서대로 되어 있지 않으면 교환**

**●그림으로 보는 버블정렬 알고리즘 예제** <br>
![버블정렬](https://user-images.githubusercontent.com/101388379/166853015-c4319e60-a0e2-4375-b3f3-cf50dddbaba5.PNG)

**●버블정렬 알고리즘 기본 코드**
```c
// 버블 정렬
void bubble_sort(int list[], int n){
  int i, j, temp;

  for(i=n-1; i>0; i--){
    
    for(j=0; j<i; j++){
      
      if(list[j]<list[j+1]){
        temp = list[j];
        list[j] = list[j+1];
        list[j+1] = temp;
      }
    }
  }
}

void main(){
  int i;
  int n = MAX_SIZE;
  int list[n] = {7, 4, 5, 1, 3};

  // 버블 정렬 수행
  bubble_sort(list, n);

  // 정렬 결과 출력
  for(i=0; i<n; i++){
    printf("%d\n", list[i]);
  }
}
```
---
 
## 쉘 정렬

### 쉘 정렬 알고리즘 기본 개념
**●위에 설명한 삽입정렬 알고리즘을 보완한 알고리즘** <br>
**●과정**
1. 정렬해야 할 리스트를 일전한 기준에 따라 분류
2. 연속적이지 않은 여러 개의 부분 리스트 생성
3. 각 부분 리스트를 삽입 정렬 이용하여 정렬
4. 모든 부분 리스트 정렬되면 다시 전체 리스트를 더 적은 부분 리스트르 만든 후에 알고리즘 반복
5. 위 과정을 부분 리스트 개수가 1일 될 때까지 반복

**●그림으로 보는 쉘 정렬 알고리즘 기본 예제**<br>
![버블](https://user-images.githubusercontent.com/101388379/166853636-6322b80d-c481-41c6-bb73-0a4e2114b778.PNG)

**●쉘 정렬 알고리즘 기본 코드**
```c
void inc_insertion_sort(int list[], int first, int last, int gap){
  int i, j, key;

  for(i=first+gap; i<=last; i=i+gap){
    key = list[i]; // 현재 삽입될 숫자인 i번째 정수를 key 변수로 복사

    // 현재 정렬된 배열은 i-gap까지이므로 i-gap번째부터 역순으로 조사한다.
    // j 값은 first 이상이어야 하고
    // key 값보다 정렬된 배열에 있는 값이 크면 j번째를 j+gap번째로 이동
    for(j=i-gap; j>=first && list[j]>key; j=j-gap){
      list[j+gap] = list[j]; // 레코드를 gap만큼 오른쪽으로 이동
    }

    list[j+gap] = key;
  }
}

// 셸 정렬
void shell_sort(int list[], int n){
  int i, gap;

  for(gap=n/2; gap>0; gap=gap/2){
    if((gap%2) == 0)(
      gap++; // gap을 홀수로 만든다.
    )

    
    for(i=0; i<gap; i++){
     
      inc_insertion_sort(list, i, n-1, gap);
    }
  }
}

void main(){
  int i;
  int n = MAX_SIZE;
  int list[n] = {10, 8, 6, 20, 4, 3, 22, 1, 0, 15, 16};

  // 셸 정렬 수행
  shell_sort(list, n);

  // 정렬 결과 출력
  for(i=0; i<n; i++){
    printf("%d\n", list[i]);
  }
}
```
---

## 합병 정렬

---

## 퀵 정렬

### 퀵 정렬 알고리즘 기본 개념
**●다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬 알고리즘**<br>
**●분할 정복 알고리즘의 하나로 , 평균적으로 매우 빠른 수행 속도**

**●과정**
1. 리스트 안에 있는 한 요소를 선택. (고른 원소를 '피벗(pivot)'이라고 한다.)
2. 피벗을 기준으로 피벗보다 작은 요소들은 모두 피벗의 왼쪽으로 , 큰 요소들은 오른쪽으로 이동
3. 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬.
4. 부분 리스트들이 더 이상 분할이 안될때까지 반복.


**●그림으로 보는 퀵 정렬 알고리즘 기본 예제**<br>

![퀵 정렬](https://user-images.githubusercontent.com/101388379/166854439-0c3b2b72-3c39-4ad6-ac02-c2f0267eb071.PNG)

**●퀵정렬 알고리즘 기본 코드**
```c
int partition(int list[], int left, int right){
  int pivot, temp;
  int low, high;

  low = left;
  high = right + 1;
  pivot = list[left]; 

  do{
   
    do {
      low++; 
    } while (low<=right && list[low]<pivot);

   do {
      high--; //high는 right 에서 시작
    } while (high>=left && list[high]>pivot);

   if(low<high){
      SWAP(list[low], list[high], temp);
    }
  } while (low<high);

  SWAP(list[left], list[high], temp);

  return high;
}

void quick_sort(int list[], int left, int right){

  if(left<right){
   
    int q = partition(list, left, right); // q: 피벗의 위치

    quick_sort(list, left, q-1); 
    quick_sort(list, q+1, right); 
  }

}

void main(){
  int i;
  int n = MAX_SIZE;
  int list[n] = {5, 3, 8, 4, 9, 1, 6, 2, 7};

  // 퀵 정렬 수행(left: 배열의 시작 = 0, right: 배열의 끝 = 8)
  quick_sort(list, 0, n-1);

  // 정렬 결과 출력
  for(i=0; i<n; i++){
    printf("%d\n", list[i]);
  }
}
```

