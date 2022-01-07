---
title: "[paper]Deep Residual Learning for Image Recognition"
author_profile: false
toc: True	
toc_label: "table of contents"
toc_icon: False
toc_sticky:	True
categories: 
    - Paper Review
tags: 
    - Deep Learning
last_modified_at: 2022-01-05
---

# Abstract

심층 신경망은 훈련하기가 더 어렵다. 이전에 사용된 네트워크보다 훨씬 더 심층적인 네트워크 훈련을 용이하게 하기 위한 잔차 학습 프레임워크를 제시한다. 우리는 참조되지 않은 함수를 학습하는 대신 레이어 입력을 참조하여 레이어를 학습 잔여 함수로 명시적으로 재구성한다. 우리는 이러한 잔류 네트워크가 최적화하기 더 쉽고 상당히 높아진 깊이에서 정확도를 얻을 수 있음을 보여주는 포괄적인 경험적 증거를 제공한다. ImageNet 데이터 세트에서는 VGG 네트보다 8배 깊지만 여전히 복잡도가 낮은 최대 152개의 레이어를 가진 잔여 네트워크를 평가합니다. 이러한 잔류 네트의 앙상블은 ImageNet 테스트 세트에서 3.57% 오류를 달성한다. 이 결과는 ILSVRC 2015 분류 과제에서 1위를 차지했다. 우리는 또한 100 및 1000 레이어를 가진 CIFAR-10에 대한 분석을 제시한다. 표현 깊이는 많은 시각적 인식 작업에서 가장 중요하다. 오로지 우리의 극도로 깊은 표현 때문에, 우리는 COCO 객체 감지 데이터 세트에서 28%의 상대적 향상을 얻는다. 심층 잔류 네트는 ILSVRC & COCO 2015 경기1에 제출한 기초이며, ImageNet 탐지, ImageNet 현지화, COCO 탐지 및 COCO 분할 작업에서 1위를 차지했다.

⇒ residual function을 사용하여, 이전 네트워크들보다 망이 깊고 학습시키기 쉬운 모델을 개발함. ImageNet 테스트 세트에서 3.57% 오류를 달성. ILSVRC 2015 분류 과제에서 1위를 차지

# Introduction

# Related work

이미지 인식에서, VLAD는 사전에 관한 residual vectors에 의해 인코딩되는 표현이며, Fisher Vector[30]는 VLAD의 확률적 버전[18]으로 공식화될 수 있다. 두 가지 모두 이미지 검색 및 분류를 위한 강력한 얕은 표현입니다 [4, 48]. 벡터 양자화의 경우, residual vectors 인코딩[17]이 원래 벡터를 인코딩하는 것보다 더 효과적인 것으로 나타났다. 저수준 비전 및 컴퓨터 그래픽에서 부분 미분 방정식(PDE)을 해결하기 위해 널리 사용되는 Multigrid 방법[3]은 시스템을 multiple scales에서 하위 문제로 재구성하며, 각 하위 문제는 더 얇은 scales와 더 미세한 scales 사이의 residual 솔루션을 담당한다. Multigrid의 대안으로는 계층적 기초 전제조건 [45, 46]이 있으며, 이는 두 scale 사이의 residual 벡터를 나타내는 변수에 의존한다. 이러한 solver는 솔루션의 residual feature을 모르는 표준 solver보다 훨씬 빠르게 수렴하는 것으로 나타났습니다 [3, 45, 46]. 이러한 방법은 좋은 재구성 또는 전제 조건이 최적화를 단순화할 수 있음을 시사한다.

지름길로 연결되는 관행과 이론[2, 34, 49]은 오랫동안 연구되어 왔다. 다층 퍼셉트론(MLP)을 훈련하는 초기 관행은 네트워크 입력에서 출력으로 연결된 선형 레이어를 추가하는 것이다 [34, 49]. [44, 24]에서 몇 개의 중간 계층이 보조 분류기에 직접 연결된다.사라지거나 기울어지는 문제를 해결하기 위해 사용할 수 있습니다. [39, 38, 31, 47]의 논문은 바로 가기 연결에 의해 구현된 계층 응답, 기울기 및 전파 오류를 집중하기 위한 방법을 제안한다. [44]에서 "인셉션" 계층은 바로 가기 가지와 몇 개의 더 깊은 가지들로 구성된다.우리가 한 작업이Concurrent,[15]의 게이팅 기능과 함께[42,43]현재 바로 가기 연결“고속 도로 네트워크”.이러한 게이트와,parameter-free 우리의 정체성 바로 가기에 대조의 매개 변수가 있data-dependent 있다. 때 외부인 출입이 지름길은“폐쇄”(영점이 다가오고), 고속 도로 네트워크의 계층들non-residual 기능을 나타낸다.반면에, 우리의 공식은 항상 중요하지만 우리의 정체성 바로 가기 문을 닫지 않는다, 그리고 함께 추가 잔류 기능 배울 수 있는 모든 정보가 항상으로 전달되다 잔류 기능을 배운다.게다가,high-way 네트워크 극도로 증가 깊이(예를 들어, 100대 이상의 층)과 정확성 이득을 증명하지 않고 있다.

# Deep Residual Learning - Method

3.1 Residual Learning

3.2 Identity Mapping by Shortcuts

3.3 Network Architectures

[9. ResNet](https://89douner.tistory.com/64)

[](https://sike6054.github.io/blog/paper/first-post/)

[(논문리뷰) ResNet 설명 및 정리](https://ganghee-lee.tistory.com/41)

[ResNet (Shortcut Connection과 Identity Mapping) [설명/요약/정리]](https://lv99.tistory.com/25)

[Deep Residual Neural Network](http://incredible.ai/artificial-intelligence/2017/10/28/Deep-Residual-Neural-Network/)

[[논문읽기] 02. Deep Residual Learning for Image Recognition : ResNet](https://leechamin.tistory.com/184)

[ResNet: Deep Residual Learning for Image Recognition (꼼꼼한 딥러닝 논문 리뷰와 코드 실습)](https://www.youtube.com/watch?v=671BsKl8d0E&t=1445s)

[고진호 Resnet : Deep Residual Learning for Image Recognition](https://www.youtube.com/watch?v=JI5kXF_OUkY)

[ResNet | AI 인공지능 기초 CNN 아키텍쳐 'Deep Residual Learning for Image Recognition' 논문 리뷰](https://www.youtube.com/watch?v=QuXIbZAUNJw)

[[Part Ⅴ. Best CNN Architecture] 8. ResNet [7] - 라온피플 머신러닝 아카데미 -](https://m.blog.naver.com/laonple/220793640991)

1. 인트로
- 요약: residual function을 사용하여, 이전 네트워크들보다 망이 깊고 학습시키기 **쉬운** 모델을 개발함. ImageNet 테스트 세트에서 3.57% 오류를 달성. ILSVRC 2015 분류 과제에서 **1위**를 차지
- 현실태: 딥 뉴럴 네트워크일수록 더 학습하기 어려움 → '**잔차를 학습하는 방법**'으로 해결 
  - 문제 1: 기울기 소실 / 폭발 → 디그레데이션(뎁스가 증가해도 어느정도 선에 다다르면 성능이 저하됨, 오버피팅 아님)  
  - 문제 2: 오버피팅 → 
더 많은 레이어를 쌓는 것만큼 쉽게 더 나은 네트워크를 학습할 수 있는가?Is learning better networks as easy as stacking more layers

- 층을 깊게 하면서도 최적화하기 위해 사용한 숏컷 커넥션 즉 아이덴티티 맾핑인데 그게 무었일까 : 어떤 연산을 해도 인풋와 동일한 값이 아웃풋으로 나오는 함수
어떤 연산이 되는 것도 아니면서도 층이 깊어져도 가중치를 온전히 주게되어 파라미터가 늘어나거나 연산량이 높아지지 않으면서 가중치를 온전히 줄수있다    

![Deep%20Residual%20Learning%20for%20Image%20Recognition%204cb499ae32f64441a103a1446268ac91/Untitled.png](Deep%20Residual%20Learning%20for%20Image%20Recognition%204cb499ae32f64441a103a1446268ac91/Untitled.png)

1. 백그라운드
- 잔차 표현: 
- 숏컷 커넥션:  
2. 메소드 
- 어떤 문제를 풀기위함인가 : gradient vanishing/explosion을 해결하기 위함 
- 방법 : 잔차 학습 프레임워크로 층을 깊게 + 숏컷 커넥션으로 아이덴티티 매핑
- 

이 방법은 few stacked layer를 underlying mapping을 직접 학습하지 않고, residual mapping에 맞추어 학습하도록 한다. underlying mapping 즉, 원래의 original mapping을 H(x)로 나타낸다면, stacked nonlinear layer의 mapping인 F(x)는 H(x)-x 를 학습하는 것이다. 따라서 original mapping은 F(x)+x 로 재구성 된다.

> stacked layer 혹은 stacked nonlinear layer는 추가 된 layer라고 생각하면 된다(Fig.2 building block 참조). 이 때, 추가된 layer들을 H(x)-x에 맞추어 학습하지만, 각 layer는 결국 H(x)를 근사하는 것이 목적이므로, F := H(x)-x를 H(x)에 대한 식으로 전개하여 original mapping이 F(x)+x 로 재구성 된 것으로 볼 수 있다.
> 

또한, 저자들은 unreferenced mapping인 original mapping보다, referenced mapping인 residual mapping을 optimize하는 문제가 더 쉽다고 가정한다. 극단적으로 H(x)에 대한 optimal solution이 identity mapping이라는 가정을 한다면, H(x)의 결과를 x가 되도록 학습하는 것보단, F(x)가 0이 되도록 학습하는 것이 쉬울 것이라는 직관에 따른 가정이다.

> H(x)=F(x)+x이기 때문에, H(x)=x가 되도록 한다는 것은 F(x)+x를 x가 되도록 학습한다는 뜻이다. 즉, residual(입력 x와의 잔차)인 F(x)가 0이 되는 것이 본 가설에서의 optimal solution인 identity mapping이 되는 것이다.
> 

F(x)+x는 “shortcut connection”으로 구현할 수 있다. shortcut connection은 하나 이상의 layer를 건너 뛴다. 본문에서 shortcut connection은 identity mapping을 수행하고, 그 출력을 stacked layer의 출력에 더하고 있다. 아래 Fig.2는 residual learning을 위한 building block 구조를 보여준다.

결과 : 이와 같은 identity shortcut connection은 별도의 parameter나 computational complexity가 추가되지 않는다. 이를 이용한 네트워크는 SGD에 따른 역전파로 end-to-end 학습이 가능하며, solver 수정 없이도 common library를 사용하여 쉽게 구현할 수 있다. 

---

1번 

근호샘 : 레지듀얼 펑션은 무엇 

혜진샘 : 1. 레지듀얼 펑션은 무엇

2. 레퍼런스 매핑과 언레퍼전스 매핑의 차이 - 인풋 엑스 값을 참조하느냐 안하는냐 차이  

3. 언 레퍼런스드 매핑을 최적화하는 거 보다 레지듀얼을 최적화하는게 더 쉬운이유 - 몰라 

4. 디그레데이션의 의미 : 웨이트들의 작아서 이 안되서 생기는 문제 

---

2번 

근호쌤 :