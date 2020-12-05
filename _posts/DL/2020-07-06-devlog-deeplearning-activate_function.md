---
layout: post
title:  "[Deep Learning] 딥러닝 학습의 다양한 기술들을 알아보자 [1] Activate Function"
subtitle: "Activate Function"
categories: devlog
tags: deeplearning
use_math: true
---

아래 설명드릴 내용은 딥러닝 관련 다양한 학습기술들을 살펴보고자 합니다. 
<br/>
 <br/>
연구소 인턴으로 일하면서 딥러닝 스터디를 하게 되었는데 제가 공부를 하면서 배운 내용들을 이 카테고리에 정리할 예정입니다. 미국 스탠포드 대학의 [CS231n](https://www.google.co.kr/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&sqi=2&ved=0ahUKEwjRkaLSyLzTAhVFnZQKHVIlCKEQFgghMAA&url=http%3A%2F%2Fcs231n.stanford.edu%2F&usg=AFQjCNHK3W1B3pbCvVlwKseIa18p7vPsAA&sig2=Lez1Eruk0Q60GK-il-qjtA)과 역시 같은 대학의 [CS224d](https://www.google.co.kr/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiTztbayLzTAhUFl5QKHf8MBjsQFgghMAA&url=http%3A%2F%2Fcs224d.stanford.edu%2F&usg=AFQjCNG1D6WOf9hSDyugLLEk8TxsdTjt2w&sig2=jXE0EVMekjNGesD0DvmrQw) 강좌를 인용하였습니다. 전반적인 내용을 요약하였습니다. 그럼 시작하겠습니다.
<br/>
 <br/>

## 딥러닝 학습의 일반적 절차

딥러닝의 일반적 절차는 다음과 같습니다.

1. **적절한 네트워크 선택**

   1) **구조(structure)** : Single words vs Bag of Words, etc.

   2) **비선형성(nonlinearity) 획득 방법** : ReLu vs tanh, etc.

2. **그래디언트 체크** : 네트워크를 구축했는데 그래디언트 계산이 혹시 잘못될 염려가 있으므로 잘됐는지 체크해봅니다

3. **학습 파라메터 초기화** : 초기화 방법에도 여러가지가 있으므로 적절히 선택합니다

4. **학습 파라메터 최적화** : Stochastic Gradient vs Adam, etc.

5. **과적합 방지** : dropout vs regularize, etc.


<br/>
 <br/>

## 비선형성 획득 : 활성함수

뉴럴네트워크의 개별 뉴런에 들어오는 입력신호의 총합을 출력신호로 변환하는 함수를 **활성화함수(activation function)**라고 합니다. 활성화함수 유무는 초창기 모델인 **퍼셉트론(perceptron)**과 뉴럴네트워크의 유일한 차이점이기도 하죠. 활성화함수는 대개 **비선형함수(non-linear function)**를 씁니다. 활성화함수로 왜 선형함수를 쓰면 안되는 걸까요? '밑바닥부터 시작하는 딥러닝'의 한 글귀를 인용해보겠습니다.

> 선형함수인 $h(x)=cx$를 활성화함수로 사용한 3층 네트워크를 떠올려 보세요. 이를 식으로 나타내면 $y(x)=h(h(h(x)))$가 됩니다. 이는 실은 $y(x)=ax$와 똑같은 식입니다. $a=c^3$이라고만 하면 끝이죠. 즉, 은닉층이 없는 네트워크로 표현할 수 있습니다. 뉴럴네트워크에서 층을 쌓는 혜택을 얻고 싶다면 활성화함수로는 반드시 비선형 함수를 사용해야 합니다.

그러면 뉴럴네트워크 활성화함수 몇 가지 살펴보겠습니다.

<br/>
 <br/>

### 시그모이드

로지스틱 함수로도 불립니다. 원래 수식 및 미분식은 아래와 같습니다.

$$\sigma (x)=\frac { 1 }{ 1+{ e }^{ -x } } \\ \sigma '\left( x \right) =\sigma (x)(1-\sigma (x))$$

시그모이드 함수의 범위는 $[0,1]$이고요, 그래프의 모양은 아래와 같습니다.

![](https://i.imgur.com/HpSpWal.png)

시그모이드 함수를 1차 미분한 그래프는 아래와 같습니다.

![](https://i.imgur.com/WpKD6kW.png)

시그모이드 함수는 개별 뉴런이 내뱉어 주는 값을 S자 커브 형태로 자연스럽게 활성화를 해주기 때문에 예전부터 인기가 좀 있었습니다. 다만 입력값이 -5보다 작거나 5보다 클 경우에는 그래디언트 값이 지나치게 작아지고(=이렇게 되면 학습이 잘 안되죠), exp 연산이 다소 무겁다(=학습이 느려지죠)는 단점이 있습니다. 

아울러 $σ(x)$의 범위는 $[0,1]$로서 모두 0 이상의 값을 지닌다는 문제가 있습니다. 이게 단점이 되는 이유는 바로 학습 속도와 관련이 있는데요. 예컨대 아래와 같은 뉴런이 있고 활성화함수 $f$가 시그모이드라고 가정해봅시다.

![](https://i.imgur.com/euw7qQu.png)


$x_0$, $x_1$, $x_2$는 모두 0 이상의 값을 갖습니다. 이들은 직전 층에서 시그모이드 함수에 의해 그 값이 양수로 활성화됐기 때문입니다. 여기에서 역전파시 최종 Loss에서 출발해 시그모이드 적용 직전의 $w_ix_i+b$ 각각에 들어오는 그래디언트를 $δ$라고 두겠습니다. 그렇다면 $w_i$의 그래디언트는 아래와 같습니다.

$$\frac { \partial L }{ \partial { w }_{ i } } ={ x }_{ i }\times \delta $$

앞서 말씀드렸듯 $x_0​$, $x_1​$, $x_2​$는 모두 0 이상이기 때문에 $δ​$가 양수라면 Loss에 대한 $w_0, w_1, w_2​$ 각각의 그래디언트가 모두 양수, 반대라면 모두 음수 값이 될 것입니다. 따라서 데이터 $x​$와 파라메터 $w​$를 2차원 벡터로 가정해 본다면 $w​$의 그래디언트는 2사분면과 4사분면 쪽 방향이 될 수는 없습니다.

결과적으로 $w$ 학습시 아래 그림처럼 허용되는 방향에 제약이 가해져(요소값이 모두 양수인' 1사분면과' 모두 음수인 '3사분면' 쪽 방향만 선택 가능) 학습속도가 늦거나 수렴이 어렵게 됩니다. 이 문제는 함수값이 0에 대해 대칭(zero-centered)인 하이퍼볼릭탄젠트 같은 함수를 쓰면 극복할 수 있다고 합니다.


![](https://i.imgur.com/bvyJYfy.png)

<br/>
 <br/>

### 하이퍼볼릭탄젠트

하이퍼볼릭탄젠트는 시그모이드 함수의 **크기와 위치를 조절(rescale and shift)**한 함수입니다. 시그모이드 함수와의 관계식과 미분식은 각각 아래와 같습니다.

$$
\begin{align*}
tanh(x)&=2\sigma (2x)-1\\ &=\frac { { e }^{ x }-{ e }^{ -x } }{ { e }^{ x }+{ e }^{ -x } } 
\\tanh'\left( x \right) &=1-tanh^{ 2 }\left( x \right) 
\end{align*}
$$

하이퍼볼릭탄젠트의 범위는 $[-1,1]$입니다. 그래프의 모양은 시그모이드 함수와는 달리 0을 기준으로 대칭인 점을 확인할 수 있습니다. 이 때문에 하이퍼볼릭탄젠트는 시그모이드를 활성화함수로 썼을 때보다 학습 수렴 속도가 빠르다고 합니다.

![](https://i.imgur.com/xaQpDt4.png)

하이퍼볼릭탄젠트를 1차 미분한 그래프는 아래와 같습니다. 시그모이드함수와 마찬가지로 $x$가 -5보다 작거나 5보다 크면 그래디언트가 0으로 작아지는 점을 볼 수 있습니다. 이것이 하이퍼볼릭탄젠트의 단점입니다.

![](https://i.imgur.com/0mVuW9h.png)

<br/>
 <br/>

### Rectified Linear Unit (ReLU)

ReLU는 아래와 같이 정의됩니다.

$$f(x)=max(0,x)$$

그래프의 모양은 아래와 같습니다. $x$가 양수이기만 하면 그래디언트가 1로 일정하므로 그래디언트가 죽는 현상을 피할 수 있고, 미분하기도 편리해 계산복잡성이 낮습니다. 실제로 시그모이드나 하이퍼볼릭탄젠트 함수 대비 학습수렴 속도가 6배나 빠르다고 합니다.

![](https://i.imgur.com/SAxRPcy.png)


다만 위 그림에서 확인할 수 있듯 0을 기준으로 대칭인 모양은 아닙니다. 아울러 $x$가 음수이면 그래디언트가 무조건 0이 된다는 단점이 있습니다. 이를 극복하기 위해 **Leaky ReLU**가 고안되었습니다.

<br/>
 <br/>


### Leaky ReLU

Leaky ReLU의 식은 아래와 같습니다.

$$f(x)=max(0.01x,x)$$

그래프의 모양은 다음과 같습니다. $x$가 음수일 때 그래디언트가 0.01이라는 점을 제외하고는 ReLU와 같은 특성을 지닙니다.

![](https://i.imgur.com/SXq4jmJ.png)

<br/>
 <br/>

### Exponential Linear Units (ELU)

ELU는 ReLU의 특성을 공유하고요, 그래디언트가 죽지 않는다는 장점이 있다고 합니다. 다음 수식과 같습니다.

$$f(x)=x\quad if\quad x>0\\ f(x)=\alpha ({ e }^{ x }-1)\quad if\quad x\le 0$$

<br/>
 <br/>

### Maxout Neurons

MN은 다음과 같습니다. 연결된 두 개의 뉴런 값 중 큰 값을 취해 비선형성을 확보합니다. 다만 활성화함수를 적용하기 위해 필요한 연산량이 많다는 단점이 있습니다.

$$f(x)=max({ w }_{ 1 }^{ T }x+{ b }_{ 1 },{ w }_{ 2 }^{ T }x+{ b }_{ 2 })$$