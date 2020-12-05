---
layout: post
title:  "[Algorithm] 이분 탐색(Binary Search)에 대해 알아보자"
subtitle: "Binary Search"
categories: devlog
tags: algorithm
use_math: true
---

이번 글에서는 **이진탐색(binary search)** 알고리즘에 대해 살펴보도록 하겠습니다. 코드는 [이곳](github.com/TheAlgorithms/Python)을 참고하였습니다. 그럼 시작하겠습니다.

<br/>
<br/>

## Binary Search

이진탐색 알고리즘이란 오름차순으로 정렬된 리스트에서 특정한 값의 위치를 찾는 알고리즘입니다. 알고리즘 특성상 정렬된 리스트에만 사용할 수 있다는 단점이 있지만, 검색이 반복될 때마다 탐색 대상 데이터 수는 직전의 절반이 되므로 속도가 빠르다는 장점이 있습니다. 

이진탐색 알고리즘을 직관적으로 나타낸 그림은 다음과 같습니다. 아래와 같이 17개 요소로 이뤄진 리스트에서 7의 위치를 찾는 이진탐색 알고리즘은 화살표 방향처럼 수행이 됩니다. 



<a href="https://imgur.com/hhiR6QU"><img src="https://i.imgur.com/hhiR6QU.png" width="500px" title="source: imgur.com" /></a>

<br/>
<br/>



## 수행방식

위 그림 예시 기준으로 이진탐색 알고리즘 수행방식을 살펴보겠습니다. 우선 리스트의 중앙을 찾습니다. 요소가 17개이므로 중앙값은 여덟번째(리스트 요소가 n개라면 n/2를 내림한 값)에 있는 요소(14)가 됩니다. 이 중앙값과 찾고자 하는 값(7)의 크기를 비교합니다. 여기서 찾고자 하는 값이 중앙값보다 작으므로 중앙값보다 큰 값(18, 19, 21, 24, 37, 40, 45, 71)들은 탐색 대상에서 제외합니다. 

<br/>


이번엔 [1, 3, 4, 6, 7, 8, 10, 13, 14]를 새로운 리스트로 보고 같은 작업을 반복 수행합니다. 리스트 중앙을 찾습니다. 새로운 리스트의 요소가 9개이므로 중앙값은 새로운 리스트의 네번째 요소(원래 리스트의 네번째 요소)이며 그 값은 6이 됩니다. 이 중앙값과 찾고자 하는 값(7)의 크기를 비교합니다. 여기서 찾고자 하는 값이 중앙값보다 크므로 중앙값보다 작은 값(1, 3, 4)들은 탐색 대상에서 제외합니다.

<br/>

이번엔 [6, 7, 8, 10, 13, 14]를 새로운 리스트로 보고 같은 작업을 반복 수행합니다. 리스트 중앙을 찾습니다. 새로운 리스트의 요소가 6개이므로 중앙값은 새로운 리스트의 세번째 요소(원래 리스트의 여섯번째 요소)이며 그 값은 8이 됩니다. 이 중앙값과 찾고자 하는 값(8)의 크기를 비교합니다. 여기서 찾고자 하는 값이 중앙값보다 작으므로 중앙값보다 큰 값(10, 13, 14)들은 탐색 대상에서 제외합니다.

이번엔 [6, 7, 8]을 새로운 리스트로 보고 같은 작업을 반복 수행합니다. 리스트 중앙을 찾습니다. 새로운 리스트의 요소가 3개이므로 중앙값은 새로운 리스트의 두번째 요소(원래 리스트의 다섯번째 요소)이며 그 값은 7이 됩니다. 이 중앙값과 찾고자 하는 값(7)의 크기를 비교합니다. 같으므로 이 중앙값의 위치(원래 리스트의 다섯번째)를 최종 산출물로 반환합니다.

<br/>

## C++ 구현

이진탐색 알고리즘의 **pseudo code**는 다음과 같습니다. 아래 코드에서 *value*은 찾고자 하는 값, *sorted_collection*는 주어진 **정렬된** 리스트, *left*는 이 리스트의 왼쪽 끝 인덱스, *right*는 오른쪽 끝 인덱스를 의미합니다.


이진 탐색 은 두 가지 버전으로 구현할 수 있습니다. 첫 째는 재귀(Reculsive) 고 두 번째는 반복(Interative) 입니다. 이해하기 쉬운 Interative 부터 확인하겠습니다.

<br/>

```cpp
int binary_search(int* arr, int length, int value) { 
    if (arr == nullptr || length < 0) 
        return -1; 
    int left = 0; 
    int right = length - 1; 
    int mid; 
    
    while(left <= right) { 
        mid = left + (right - left) / 2; 
        
        if(arr[mid] == value) 
            return mid; 
        else if (arr[mid < value]) left = mid + 1;
        else if (arr[mid] > value) right = mid - 1;   
    }
        return -1; 
}

```
<br/>

다음은 재귀 버전 입니다. 

<br/>

```cpp
int binary_search(int* arr, int value, int left, int right) { 
    if(left > right) 
        return -1; 
    int mid = left + (right - left) / 2; 
    
    if (arr[mid] == value) 
        return mid; 
    else if (arr[mid] > value) 
        binary_search_reculsive(arr, value, left, mid - 1); 
    else 
        binary_search_reculsive(arr, value, mid + 1, right); 
}

```



## 시간복잡도

이진탐색의 시간복잡도를 따져봅시다. 의사코드를 보면 재귀적으로 자신을 호출하는 부분을 제외하면 상수배 복잡도를 가집니다. left와 right가 같은지, A[left]가 num과 같은지, A[center]가 num과 같은지 등을 따져보고 그 조건에 맞는 몇 가지 기본 연산을 수행하면 되기 때문입니다. 아울러 이진탐색이 반복될 때마다 고려 대상이 되는 리스트의 요소 수는 절반씩 줄어듭니다. 이를 식으로 나타내면 다음과 같습니다.

<img src="https://render.githubusercontent.com/render/math?math=\Large T\left( n \right) =T\left( \frac { n }{ 2 }  \right) %2Bc">
<img src="https://render.githubusercontent.com/render/math?math=\Large =T\left( \frac { n }{ { 2 }^{ 2 } }  \right) %2Bc%2Bc">
<img src="https://render.githubusercontent.com/render/math?math=\Large =T\left( \frac { n }{ { 2 }^{ 3 } }  \right) %2Bc%2Bc%2Bc">

<img src="https://render.githubusercontent.com/render/math?math=\Large =T\left( \frac { n }{ { 2 }^{ i } } \right) %2Bc\times i">


<br/>


<img src="https://render.githubusercontent.com/render/math?math=\Large =T\left( 1 \right) %2BT(1)%2B...%2BT\left( 1 \right)%2Bc\times \log _{ 2 }{ n }
">

<br/>

<img src="https://render.githubusercontent.com/render/math?math=\Large =O\left( \log _{ 2 }{ n }  \right)">
