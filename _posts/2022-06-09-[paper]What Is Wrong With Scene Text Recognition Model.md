---
title: "[paper]What Is Wrong With Scene Text Recognition Model"
author_profile: false
toc: True	
toc_label: "Table of Contents"
toc_icon: True
toc_sticky:	True
categories: 
    - Paper Review
tags: 
    - Computer Vision
    - Optical Character Recognition
    - Scene Text Recognition
last_modified_at: 2022-06-09
---
# **Abstract**

 최근 몇 년간 scene text recognition(STR) 모델에 대한 많은 새로운 제안들이 소개되었다. 반면 각 주장들은 기술의 끝까지 도달했다고 주장했지만, training과 evaluation 데이터 셋의 일관적이지 못한 선택때문에 업무 현장에서의 전체적이고 공정한 비교는 많이 누락되었다. 이 논문은 이 어려움에 대한 세 가지 주된 기여를 다룬다. 첫 번째로, 우리는 훈련과 검증 데이터 셋의 비일관성과 비일관성으로 인한 성능 차이를 조사한다. 두 번째로, 우리는 대부분의 기존 STR 모델에 적합한 통일된 네 단계의 STR 프레임워크를 소개한다. 이 프레임워크를 사용하면, 기존에 제안된 STR 모듈들의 확장된 평가를 할 수 있고, 기존에 발견하지 못했었던 모듈 조합의 발견을 할 수 있다.  세 번째로, 우리는 하나의 일관된 훈련과 검증 데이터 셋에서 정확도, 속도, 메모리 수요 측면에서의 성능에 대한 모듈별 공헌을 분석한다. 이러한 분석은 기존의 모듈들의 성능 획득을 알아내기 위한 현재의 비교에서의 장애물을 제거한다. 우리 코드는 모두가 사용 가능할 수 있다.   

# **1. Introduction**

- OCR 시스템은 깨끗하게 잘 정제된 문서에서는 좋은 성능을 보임, 그러나 다양한 텍스트가 나타나는 실제 세상 이미지와 완벽하지 않은 조건에서 찍힌 장면에서는 좋은 성능을 보이지 못함.

![2022-06-09- paper What Is Wrong With Scene Text Recognition Model_1](https://user-images.githubusercontent.com/57248121/172964721-740222db-67fb-4424-84fa-7f19d2c1444f.png){: width="50%" height="50%"}{: .left}

![2022-06-09- paper What Is Wrong With Scene Text Recognition Model_2](https://user-images.githubusercontent.com/57248121/172964758-81f0f667-b658-40b7-b81c-22b14b20c13a.jpg){: width="50%" height="50%"}{: .right}