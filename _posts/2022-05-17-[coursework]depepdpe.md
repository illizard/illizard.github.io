---
title: "[paper]Encoder-Decoder with Atrous Separable Convolution for Semantic Image Segmentation"
author_profile: false
toc: True	
toc_label: "table of contents"
toc_icon: True
toc_sticky:	True
categories: 
    - Paper Review
tags: 
    - Computer Vision
    - Segmentation
last_modified_at: 2022-01-05
---

# ***Related Work***

### *Atrous Convolution*

![https://bloglunit.files.wordpress.com/2018/07/atrous.png?w=644&h=308](https://bloglunit.files.wordpress.com/2018/07/atrous.png?w=644&h=308)

Atrous convolution 예시. **[출처](http://www.mdpi.com/2072-4292/9/5/498/htm)**

Atrous convolution은 기존 convolution과 다르게, 필터 내부에 빈 공간을 둔 채로 작동하게 됩니다. 위 예시에서, 얼마나 빈 공간을 둘 지 결정하는 파라미터인 rate *r*=1일 경우, 기존 convolution과 동일하고, *r*이 커질 수록, 빈 공간이 넓어지게 됩니다.

Atrous convolution을 활용함으로써 얻을 수 있는 이점은, 기존 convolution과 동일한 양의 파라미터와 계산량을 유지하면서도, field of view (한 픽셀이 볼 수 있는 영역) 를 크게 가져갈 수 있게 됩니다. 보통 semantic segmentation에서 높은 성능을 내기 위해서는 convolutional neural network의 마지막에 존재하는 한 픽셀이 입력값에서 어느 크기의 영역을 커버할 수 있는지를 결정하는 receptive field 크기가 중요하게 작용합니다. Atrous convolution을 활용하면 파라미터 수를 늘리지 않으면서도 receptive field를 크게 키울 수 있기 때문에 DeepLab series에서는 이를 적극적으로 활용하려 노력합니다.

### *Spatial Pyramid Pooling*

![https://bloglunit.files.wordpress.com/2018/07/aspp1.png?w=740](https://bloglunit.files.wordpress.com/2018/07/aspp1.png?w=740)

Atrous spatial pyramid pooling (ASPP)

Semantic segmentaion의 성능을 높이기 위한 방법 중 하나로, spatial pyramid pooling 기법이 자주 활용되고 있는 추세입니다. DeepLab V2에서는 feature map으로부터 여러 개의 rate가 다른 atrous convolution을 병렬로 적용한 뒤, 이를 다시 합쳐주는 atrous spatial pyramid pooling (ASPP) 기법을 활용할 것을 제안하고 있습니다. 최근 발표된 **[PSPNet](https://arxiv.org/abs/1612.01105)**에서도 atrous convolution을 활용하진 않지만 이와 유사한 pyramid pooling 기법을 적극 활용하고 있습니다. 이러한 방법들은 multi-scale context를 모델 구조로 구현하여 보다 정확한 semantic segmentation을 수행할 수 있도록 돕게 됩니다. DeepLab 에서는 ASPP를 기본 모듈로 사용하고 있습니다.

### *Encoder-Decoder*

![https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/u-net-architecture.png](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/u-net-architecture.png)

U-Net 구조

Encoder-decoder 구조 또한 semantic segmentation을 위한 CNN 구조로 자주 활용되고 있습니다. 특히, **[U-Net](https://arxiv.org/abs/1505.04597)**이라 불리는 encoder-decoder 구조는  정교한 픽셀 단위의 segmentation이 요구되는 biomedical image segmentation task의 핵심 요소로 자리잡고 있습니다. 왼쪽의 encoder 부분에서는 점진적으로 spatial dimension을 줄여가면서 고차원의 semantic 정보를 convolution filter가 추출해낼 수 있게 됩니다. 이후 오른쪽의 decoder 부분에서는 encoder에서 spatial dimension 축소로 인해 손실된 spatial 정보를 점진적으로 복원하여 보다 정교한 boundary segmentation을 완성하게 됩니다.

U-Net이 여타 encoder-decoder 구조와 다른 점은, 위 그림에서 가운데 놓인 회색 선입니다. Spatial 정보를 복원하는 과정에서 이전 encoder feature map 중 동일한 크기를 지닌 feature map을 가져 와 prior로 활용함으로써 더 정확한 boundary segmentation이 가능하게 만듭니다.

이번에 발표된 DeepLab V3+에서는 U-Net과 유사하게 intermediate connection을 가지는 encoder-decoder 구조를 적용하여 보다 정교한 object boundary를 예측할 수 있게 됩니다.

### *Depthwise Separable Convolution*

![https://bloglunit.files.wordpress.com/2018/07/conv.png?w=200&h=360](https://bloglunit.files.wordpress.com/2018/07/conv.png?w=200&h=360)

Convolution. **[출처](https://eli.thegreenplace.net/2018/depthwise-separable-convolutions-for-machine-learning/)**

입력 이미지가 8×8×3 (*H*×*W*×*C*) 이고, convolution filter 크기가 3×3 (*F*×*F*) 이라고 했을 때, filter 한 개가 가지는 파라미터 수는 3×3×3 (*F*×*F*×*C*) = 27 이 됩니다. 만약 filter가 4개 존재한다면, 해당 convolution의 총 파라미터 수는 3×3×3×4 (*F*×*F*×*C×N)* 만큼 지니게 됩니다.

![https://bloglunit.files.wordpress.com/2018/07/depth_conv.png?w=300&h=299](https://bloglunit.files.wordpress.com/2018/07/depth_conv.png?w=300&h=299)

Depthwise convolution. **[출처](https://eli.thegreenplace.net/2018/depthwise-separable-convolutions-for-machine-learning/)**

Convolution 연산에서 channel 축을 filter가 한 번에 연산하는 대신에, 위 그림과 같이 입력 영상의 channel 축을 모두 분리시킨 뒤, channel 축 길이를 항상 1로 가지는 여러 개의 convolution 필터로 대체시킨 연산을 depthwise convolution이라고 합니다.

![https://bloglunit.files.wordpress.com/2018/07/depthwise_separable_conv.png?w=300&h=435](https://bloglunit.files.wordpress.com/2018/07/depthwise_separable_conv.png?w=300&h=435)

Depthwise separable convolution. **[출처](https://eli.thegreenplace.net/2018/depthwise-separable-convolutions-for-machine-learning/)**

이제, 위의 depthwise convolution으로 나온 결과에 대해, 1×1×*C* 크기의 convolution filter를 적용한 것을 depthwise separable convolution 이라 합니다. 이처럼 복잡한 연산을 수행하는 이유는 기존 convolution과 유사한 성능을 보이면서도 사용되는 파라미터 수와 연산량을 획기적으로 줄일 수 있기 때문입니다.

예를 들어, 입력값이 8×8×3 이고 16개의 3×3 convolution 필터를 적용할 때 사용되는 파라미터의 개수는

- Convolution: 3×3×3×16 = 432
- Depthwise separable convolution: 3×3×3 + 3×16 = 27 + 48 = 75

임을 확인할 수 있습니다.

Depthwise separable convolution은 기존 convolution filter가 spatial dimension과 channel dimension을 동시에 처리하던 것을 따로 분리시켜 각각 처리한다고 볼 수 있습니다. 이 과정에서, 여러 개의 필터가 spatial dimension 처리에 필요한 파라미터를 하나로 공유함으로써 파라미터의 수를 더 줄일 수 있게 됩니다. 두 축을 분리시켜 연산을 수행하더라도 최종 결과값은 결국 두 가지 축 모두를 처리한 결과값을 얻을 수 있으므로, 기존 convolution filter가 수행하던 역할을 충분히 대체할 수 있게 됩니다.

픽셀 각각에 대해서 label을 예측해야 하는 semantic segmentation은 난이도가 높은 편에 속하기 때문에 CNN 구조가 깊어지고 receptive field를 넓히기 위해 더 많은 파라미터들을 사용하게 되는 상황에서, separable convolution을 잘 활용할 경우 모델에 필요한 parameter 수를 대폭 줄일 수 있게 되므로 보다 깊은 구조로 확장하여 성능 향상을 꾀하거나, 기존 대비 메모리 사용량 감소와 속도 향상을 기대할 수 있습니다.