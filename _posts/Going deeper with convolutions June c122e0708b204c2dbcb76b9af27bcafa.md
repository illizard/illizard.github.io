# Going deeper with convolutions June

Description: GoogLeNet
Journal/Conference: IEEE
Year: 2015
작성일: 2021년 3월 9일
작성자: 익명
태그: CNN, Computer Vision, Neural Network

# 1. Introduction을 읽고 논문에 대한 질문 적기

- 얘도 vgg가 말한 거처럼 층을 깊게 쌓았는데 어디가 달랐길래 1등을 했을까 - 뎁스 뿐만 아니라 넓이까지 층을 높였다

혜진 : 인센셥 설명이나 정의 / 혜비안 프린스펄 / 

네트워크에 대한 비용같은것 - 비용은 구조가 다른 모델들과 성능은 비슷한데 컴퓨팅 자원이 줄어서 더 좋은 효율이다 

  

# 2. Background를 읽고, 5문장 이내로 요약하기

 - 채널의 사이즈를 줄인다고 하면 알쥐비 3채널에서 1채널로 줄이거나 2채널로 줄이는 건데 특징이 잘 유지가 되나?  

LeNet-5를 시작으로, 컨볼루션 신경망(CNN)은 일반적으로 표준 구조.  적은 데이터에서는 오버피팅 문제가 있어, 드롭아웃을 사용하여 과적합 문제를 해결해왔다. 맥스풀링시에 정확한 공간 정보를 잃어 버릴 수 있었음에도, AlexNet은 localization과 object detection 및 human pose estimation 분야에서 성공적인 성과를 거뒀다. 

NIN : 1x1 conv layer가 네트워크에 추가되어 depth를 증가시켜, 네트워크 가중치의 벡터 표현을 증가시키기 위한 방법으로서, 컴퓨팅 연산시의 병목을 제거하기 위한 차원 축소 모듈임. 또 뎁스를 늘림과 동시에 넓이를 증가시킴

RCNN : 전체적인 디텍션 문제를 두개의 하위 태스크로 분할 

1단계 :  추상적인 특징을 활용하여, 클래스에 고려하지않고 객체가 있을만한 지역을 추천 

2단계: CNN 분류기를 사용해서 해당 객체 좌표들의 클래스 식별하는 단계 

- two stage approach는 low-level feature를 활용한 bounding box segmentation의 정확성 뿐만 아니라, state-of-the-art CNN의 강력한 classification power를 활용할 수 있다

NIN : 1x1 conv layer가 네트워크에 추가되어 depth를 증가시켜, 네트워크 가중치의 벡터 표현을 증가시키기 위한 방법으로서, 컴퓨팅 연산시의 병목을 제거하기 위한 차원 축소 모듈임. 또 뎁스를 늘림과 동시에 넓이를 증가시킴

1. computational bottleneck을 제거하기 위한 dimension reduction module로써 사용
2. depth를 늘리는 것과 더불어, 큰 성능의 저하없이 width의 증가를 위함

RCNN : 전체적인 디텍션 문제를 두개의 하위 태스크로 분할 

1. color와 texture 같은 low-level feature를 활용하여, category에 구애받지 않는 방식으로 object location proposal을 생성
2. CNN classifier를 사용하여, 해당 location들의 object category를 식별
이러한 two stage approach는 low-level feature를 활용한 bounding box segmentation의 정확성 뿐만 아니라, state-of-the-art CNN의 강력한 classification power를 활용할 수 있다

# 3. Approach 단계 정리하기

논문에서 어떤 원인을 해결하기 위해 시도하는 방법이 무엇인지 구체화하기.

문제 : 네트워크가 깊어질 수록 오버피팅이나 기울기 소실, 연산량 증가로 인한 컴퓨팅 자원 소모

방법 : FC layer나 convolution 내부를 sparse한 것으로 교체하여, sparsity을 도입하는 것

목적 : 네트워크 크기(가중치의 개수)를 증가시켜 성능을 향상시키는 것이 딥러닝의 목적 

1. 일반적으로 bigger size는 더 많은 수의 parameter를 가진다는 것을 의미하므로, 네트워크가 overfitting 되기 쉬운 경향이 있으며, 특히 labeled training set이 제한적일 경우에는 더욱 심해진다.
2. 네트워크의 크기가 증가함에 따라, computaional resource이 극도로 증가한다.

⇒위 두 가지 문제를 해결하는 근본적인 방법은 FC layer나 convolution 내부를 sparse한 것으로 교체하여, sparsity을 도입하는 것이다

하지만 희소 행렬을 계산하는것은 매우 비효율 , 지나치게 깊은 네트워크는 오버피팅의 위험, Vanishing gradient의 위험을 가집니다. 이 뿐만이 아니라, 깊어질수록 필요한 연산의 양 또한 따라서 증가합니다. 이러한 문제를 해결하는 것은 Dropout을 사용하는 네트워크처럼 sparse하게 연결되는 구조를 만드는 것입니다. 하지만 컴퓨터의 연산은 Dense할수록 빠르다는 특징이 있습니다. 이 둘 사이의 타협을 위해 Inception 모듈이 등장하게 됩니다.

덴스하게 서브메트릭스로 클러스터링 

![Going%20deeper%20with%20convolutions%20June%20c122e0708b204c2dbcb76b9af27bcafa/Untitled.png](Going%20deeper%20with%20convolutions%20June%20c122e0708b204c2dbcb76b9af27bcafa/Untitled.png)

# 4. Experiment 정리하기(예: A 방법은 어떤 방법인지, 데이터 셋 구조, 실험 방법)

GoogLeNet은 Inception module 내부의 reduction/projection layer를 포함한 모든 convolution에서 ReLU를 사용한다. Receptive field는 zero mean RGB data에서 224x224 크기를 가진다. 또한, parameter가 있는 경우만 계산하면 22-layer에 해당한다. 네트워크에 사용 된 layer를 개별적으로 카운팅하면 약 100개 정도 사용하고 있다.

# 5. 논문과 관련된 다른 논문이 어떤 것이 있는지, 논문 제목 및 정보 적기

논문 목차 순으로 잘 되어 있음 [https://89douner.tistory.com/62](https://89douner.tistory.com/62)

[https://leedakyeong.tistory.com/entry/논문-GoogleNet-Inception-리뷰-Going-deeper-with-convolutions-1](https://leedakyeong.tistory.com/entry/%EB%85%BC%EB%AC%B8-GoogleNet-Inception-%EB%A6%AC%EB%B7%B0-Going-deeper-with-convolutions-1)

[https://kangbk0120.github.io/articles/2018-01/inception-googlenet-review](https://kangbk0120.github.io/articles/2018-01/inception-googlenet-review)

[https://sike6054.github.io/blog/paper/second-post/](https://sike6054.github.io/blog/paper/second-post/)

## 핵심 : 네트워크 안에서 컴퓨팅 예산은 일정하게 유지하면서, 네트워크의 깊이와 너비를 증가할 수 있게 설계

- 더 큰 데이터와 더 큰 모델 사용
- 알고리즘의 효율성, 전력 메모리 사용의 효율성 증대 목표로 만들어짐

이전에는 대용량 데이터에서 드롭아웃으로 오버피팅을 막음. 레이어 수와 사이즈를 늘림 

맥스 풀링은 정확한 공간적 정보를 손실 , 그럼에도 알렉스넷에서 좋은 성능 

딥러닝 네트워크의 성능을 향상 시키려면 → 깊이와 너비를 늘려야함 → 파라미터를 늘림 → 적은 데이터의 경우 오버피팅 가능성 

네트워크 크기 늘리면 컴퓨터의 자원의 사용이 증가 

⇒ 두 문제를 해결하기 위해 컨볼류선에서 풀리 커넥티드에서 sparsely connected 아키텍처로 변경해야함 

그러나 현재의 하드웨어는 sparse한 매트릭스 연산에 비효율적, 상대적으로 밀도가 높은 dense 메트릭스들로 클러스트링해야함

기존 2012년 ILSVRC 우승한 Alex Net이 사용한 파라메터보다 12배나 더 적게 사용하였다고 합니다.

출처:

[https://arclab.tistory.com/161](https://arclab.tistory.com/161)

[Arc Lab.'s Blog]

지나치게 깊은 네트워크는 오버피팅의 위험, Vanishing gradient의 위험을 가집니다. 이 뿐만이 아니라, 깊어질수록 필요한 연산의 양 또한 따라서 증가합니다. 이러한 문제를 해결하는 것은 Dropout을 사용하는 네트워크처럼 sparse하게 연결되는 구조를 만드는 것입니다. 하지만 컴퓨터의 연산은 Dense할수록 빠르다는 특징이 있습니다. 이 둘 사이의 타협을 위해 Inception 모듈이 등장하게 됩니다.

대부분의 CNN에서 이미지가 Convolution연산을 거치게 되면 channel은 커지고 height와 width는 줄어듭니다. 자 구체적인 예를 들어 자세히 알아봅시다. 192 x 28 x 28(C x H x W)의 크기를 가지는 데이터가 있다고 해볼까요? 필터의 크기는 5x5, 스트라이드는 1, 패딩은 2라고 합시다. 필터가 32개 있다고 하면 결과는 32 x 28 x 28이 되겠죠. 문제는 여기서 생겨납니다. 각 필터는 192 x 5 x 5의 크기를 갖고, 결과는 32x28x28이므로, 총 필요한 연산은 192x5x5x32x28x28이 됩니다. 이는 120만개에 이르는 엄청난 연산을 필요로 하죠. 이러한 연산들을 몇 층씩 쌓는다는 것은 너무 부담스러운 일입니다.

참고 

Is there any particular reason why people pick 224x224 image size for imagenet experiments?

개체는 ImageNet 데이터 세트의 이미지 중간에 나타나는 경우가 많습니다. 최대 5개의 풀 후에 224x224는 7x7이 되어 중심점을 갖게 된다. 256x256 이미지는 8x8이며 별도의 중앙점이 없습니다. 더 있을 수도 있지만, 이것이 제가 기억하는 것입니다.

[https://www.quora.com/Why-should-we-always-resize-input-images-to-224x224-for-VGG-classification-I-have-tried-building-the-network-with-other-larger-size-input-but-out-of-memory-while-out-convolution-and-come-into-fully-connected-layer](https://www.quora.com/Why-should-we-always-resize-input-images-to-224x224-for-VGG-classification-I-have-tried-building-the-network-with-other-larger-size-input-but-out-of-memory-while-out-convolution-and-come-into-fully-connected-layer)

# 번역

### related work

LeNet-5[10]를 시작으로, 컨볼루션 신경망(CNN)은 일반적으로 표준 구조를 가지고 있다. 스택형 컨볼루션 레이어(선택적으로 대비 정규화와 최대 풀링에 이어)는 하나 이상의 완전히 연결된 레이어가 뒤따른다. 이러한 기본 설계의 변형은 이미지 분류 문헌에 널리 퍼져 있으며 MNIST, CIFAR 및 특히 ImageNet 분류 과제에서 현재까지 최고의 결과를 얻었다[9, 21]. Imagenet과 같은 대규모 데이터 세트의 경우, 최근 추세는 계층 수[12]와 계층 크기[21, 14]를 증가시키는 동시에 드롭아웃[7]을 사용하여 과적합 문제를 해결하는 것이다.

최대 풀링 레이어가 정확한 공간 정보를 잃게 된다는 우려에도 불구하고 [9]와 동일한 컨볼루션 네트워크 아키텍처가 [9, 14], 객체 감지 [6, 14, 18, 5] 및 인간 포즈 추정에도 성공적으로 채택되었다. 영장류 시각 피질의 신경과학 모델에 영감을 받아 세레 외 연구진입니다. [15] Inception 모델과 유사하게 여러 척도를 처리하기 위해 다양한 크기의 고정 Gabor 필터를 사용합니다. 그러나 [15]의 고정 2층 심층 모델과 달리 Inception 모델의 모든 필터는 학습된다. 또한 Inception 계층은 여러 번 반복되어 GoogLeNet 모델의 경우 22계층 심층 모델로 이어진다.

네트워크 내 네트워크(Network-in-Network)는 라인 등이 제안한 접근 방식이다. [12] 신경망의 대표력을 증가시키기 위해. 컨볼루션 레이어에 적용할 때, 이 방법은 일반적으로 정류된 선형 활성화가 뒤따르는 추가 1×1 컨볼루션 레이어로 볼 수 있었다[9]. 이를 통해 현재의 CNN 파이프라인에 쉽게 통합할 수 있다. 우리는 아키텍처에서 이 접근 방식을 많이 사용한다. 그러나, 우리의 설정에서 1×1 컨볼루션은 이중 목적을 가지고 있다. 가장 중요한 것은, 그것들은 주로 계산 병목 현상을 제거하기 위한 차원 축소 모듈로 사용되며, 그렇지 않으면 네트워크의 크기를 제한할 수 있다. 이를 통해 성능 저하 없이 깊이뿐만 아니라 네트워크 폭도 높일 수 있습니다.

객체 감지를 위한 현재 선도적인 접근 방식은 기르시크 등이 제안한 컨볼루션 신경망(R-CNN) 영역이다. [6] R-CNN은 전체 탐지 문제를 두 가지 하위 문제로 분해한다. 먼저 범주 없는 방식으로 잠재적 개체 제안에 색상과 슈퍼픽셀 일관성 같은 낮은 수준의 단서를 활용하고, 그 다음 CNN 분류기를 사용하여 해당 위치에서 개체 범주를 식별한다. 이러한 2단계 접근 방식은 낮은 수준의 단서를 가진 경계 상자 분할의 정확성과 최첨단 CNN의 매우 강력한 분류 능력을 활용한다. 우리는 감지 제출물에서도 유사한 파이프라인을 채택했지만, 더 높은 객체 경계 상자 리콜을 위한 다중 상자[5] 예측과 경계 상자 제안의 더 나은 분류를 위한 앙상블 접근법과 같은 두 단계 모두에서 향상된 기능을 탐구했다.

# 참고

[(GoogLeNet) Going deeper with convolutions 번역 및 추가 설명과 Keras 구현 - Aroddary’s Personal Lab](https://sike6054.github.io/blog/paper/second-post/)