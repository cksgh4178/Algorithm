# Chapter 4. 정렬
<br>
<br>
## 소개
정렬 알고리즘 $\rightarrow$ 집합의 원소들을 순서대로 재배치하는 알고리즘

정렬 알고리즘을 공부하는 이유
* 정렬은 자주 사용되므로 실용적
* 많은 종류의 정렬 알고리즘이 개발되어 있고 이를 알고 있으면 여러 문제에 대한 관점이 달라짐
* 정렬은 최악의 경우와 평균적인 경우에 대한 강력한 하한선을 쉽게 유도할 수 있는 몇 안되는 문제 중의 하나 $\Rightarrow$ 강력한 하한선을 유도할 수 있다는 것은 최적의 정렬 알고리즘이 존재한다는 의미

정렬된 집합에서 각 원소는 **키(key)** 라는 식별자를 가지고 있으며 이 키를 오름차순으로 정렬
정렬 과정에서 각 키들을 비교하는 연산 외에 키에 다른 연산을 해선 안됨
$\rightarrow$ 알고리즘을 분석하기 위한 측정치는 **키들 간의 비교 횟수**
<br>

## 삽입 정렬 (Insertion Sort)
삽입 정렬은 아이디어가 자연스럽고 일반적이며 최악의 경우와 평균 수행 분석이 쉬움
<br>

### 전략

![figure](https://thomaslockblog.files.wordpress.com/2018/01/insertion-sort-model11.jpg?w=760)

1. 그림에서 x는 아직 검사되지 않은 부분의 가장 왼쪽 원소

2. x를 먼저 밖으로 빼내고(지역 변수로 복사) 그 자리를 빈 칸으로 남겨둠

3. 반복적으로 x를 빈칸의 왼쪽 원소와 비교

4. x가 더 작으면 그 원소를 빈 칸으로 옮기고 그 원소가 있던 곳은 빈칸으로 남겨둠(빈 칸은 왼쪽으로 한 칸 이동)

5. 현재 빈 칸의 왼쪽에 아무 원소도 없거나, 현재 빈 칸의 왼쪽에 있는 원소가 x보다 더 작거나 같을 때 중단

6. x를 빈 칸에 삽입
<br>
### 알고리즘과 분석
```python
def Shift_Vac(E, index, key):
    vacant = index
    loc = 0
    while(vacant > 0):
        if(E[vacant-1] <= key):
            loc = vacant
            break
        E[vacant] = E[vacant-1]
        vacant -= 1  
    return loc

def Insertion_sort(E, n):
    # 모든 원소에 대해 검색 
    for i in range(n):
        current = E[i]
        loc = Shift_Vac(E, i, current)
        E[loc] = current
```

#### 최악의 경우 복잡도
각 단계에서 키 비교의 최대 횟수는 $i$번 (지금 검색하고 있는 위치)
따라서 전체는 $W(n) \leq \sum_{i=1}^{n-1}i  = \frac{n(n-1)}{2}  \in \Theta(n^2)$

#### 평균 복잡도
어떤 원소가 특정 위치에 있을 확률은 모두 같다고 가정하면 $x$가 어떤 특정 위치에 있을 확률은 모두 $\frac{1}{(i+1)}$이므로 평균 비교 횟수는

$\frac{1}{(i+1)}\sum_{j=1}^{i}{j +\frac{1}{i+1}(i)} = \frac{i}{2} + \frac{i}{i+1} = \frac{i}{2} + 1 - \frac{1}{i+1}$

$n-1$번의 모든 삽입에 더하면

$A(n) = \sum_{i=1}^{n-1}(\frac{i}{2}+1\frac{1}{i+1}) = \frac{n(n-1)}{4}+n-1-\sum_{j=2}^{n}\frac{1}{j} \approx \frac{n^2}{4} \in \Theta(n^2)$



* 키의 비교에 의해 정렬하고 비교를 한 번 하여 최대 하나의 역을 제거하는 알고리즘은 최악의 경우 적어도 $n(n-1)/2$ 번의 비교를 해야 하고, 평균적으로 $n(n-1)/4$ 번의 비교를 해야 한다
<br>
## 분할 정복
<br>
분할 정복 알고리즘 설계 패러다임의 원칙 : 주어진 문제를 작은 부문제로 나누어 더 쉽게 해결하고자 하는 것 (*divide & conquer & combine*)
분할 정복의 대표적인 예제 : 이진 탐색 (binary search)

분할 정복 알고리즘을 설계하기 위해서는 서브루틴 directly solve, divide, combine을 명시해야함

입력이 나누어진 작은 $k$개의 인스턴스: $I_1, I_2, I_3, \dots, I_k$
크기가 $n$인 입력에 대해 
$B(n)$ : directly sove에 의해 수행되는 단계의 개수
$D(n)$ : divide에 의해 수행되는 단계의 개수
$C(n)$ : combine에 의해 수행되는 단계의 개수
라 할 때 알고리즘에 의해 수행되는 작업의 양

```python
solve(I)
	n = size(I)
	if (n <= smallsize):
		solution = directlysolve(I)
    else:
    	divide I into I1, I2, ..., Ik
    	for i in range(k):
    		Si = solve(Ii)
        solution = combine(S1, S2, ..., Sk)
return solution
```
$T(n) = D(n) + \sum_{i=1}^{k}T(size(I_i)) + C(n) \qquad  for \;n > small size$
$T(n) = B(n)   \qquad for \; n \leq small size $

대부분의 분할 정복 알고리즘에서 분할 단계 혹은 결합 단계는 매우 단순(둘 중 하나는 어려움)
퀵 정렬 : 어려운 분할(hard division)  쉬운 결합(easy combine)
병합 정렬 : 쉬운 분할(easy division)  어려운 결합(hard combine)
실제 작업은 대부분 **어려운** 부분에서 이루어짐
<br>

## 퀵 정렬
<br>

퀵 정렬 : 분할 정복 알고리즘 중의 하나이고 가장 널리 쓰이고 있음 (C.A.R Hoare, 1962)

### 퀵 정렬 전략
* 배열에서 정렬할 원소들 중 가장 *작은* 값을 갖는 키들이 *큰* 값을 갖는 키들의 앞쪽에 가도록 재배열
* *작은* 값을 갖는 키들과 *큰* 값의 키들이 모여있는 두 부분에 대해 재귀적으로 정렬을 반복

정렬할 배열을 $E$ 라고 하고 정렬하고자 하는 배열의 시작 인덱스와 끝 인덱스는 각각 first, last라 하면, 처음에는 first = 0, last = n-1이 됨.
**축값(pivot)** 이라 불리는 원소 선택 후 축값을 이용하여 분할

* $first \leq i < splitPoint, E[i].key < pivot$
* $splitPoint < i \leq last, E[i].key \geq pivot$
* 퀵 정렬은 서브루틴 partition에 파라미터로써 축값만을 넘기고 다른 원소들을 재배열하여 다음 과 같은 splitpoint를 찾음으로써 정렬
배열 E의 splitPoint는 빈 칸 $\rightarrow$ 축값을 E[splitPoint]에 저장

![그림](https://media.vlpt.us/images/ash3767/post/abcda457-b2fe-4bf7-a59b-d623dfd7912e/image.png)

퀵 정렬 프로시저는 전처리 과정으로서 E[first]와 E[last] 사이에 있는 임이의 키를 선택하여 분할가능

선택된 원소는 지역 변수 pivot으로 옮겨지고 pivot이 E[first]가 아니면 partition이 호출될 때 E[first]가 빈칸이 되도록 E[first]를 pivot으로 선택된 원소의 위치로 옮겨진다
<br>

## 정렬된 수열의 병합
<br>
## 병합 정렬
<br>
## 키 비교에 의한 정렬 알고리즘의 하한선
<br>
## 힙 정렬
<br>
## 네 개의 정렬 알고리즘 비교
<br>
## 쉘 정렬
<br>
## 기수 정렬
<br>