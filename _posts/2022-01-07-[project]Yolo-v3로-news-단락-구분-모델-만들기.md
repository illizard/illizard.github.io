---
title: "[project]Yolo v3로 news 단락 구분 모델 만들기"
author_profile: false
toc: True	
toc_label: "Table of Contents"
toc_icon: True
toc_sticky:	True
categories: 
    - Project
tags: 
    - Computer Vision
    - Object Detection
    - Optical Character Recognition
last_modified_at: 2022-01-07
---

<aside>
💡 [https://towardsdatascience.com/object-detection-on-newspaper-images-using-yolov3-85acfa563080](https://towardsdatascience.com/object-detection-on-newspaper-images-using-yolov3-85acfa563080) 참고해서 news-yolo를 따라한 내용이지만 다른 언어의 신문으로 할 경우, 파라미터들 수정 필요

</aside>

# Goal : Tesseract OCR 입력 되기 전 신문 지면을 분석하는 모델만들기

- Tesseract ocr은 해당 이미지 내 가장 많은 글자 크기를 기준으로 하기 때문에, 다수 글자보다 크거나 작은 것들이 이미지로 인식됨
- 큰 글자 / 작은 글자 / 평균 글자 다 따로 보면 됨

# 신문 클래스 4개 - 영자 신문

- 신문 로고 / 기사 제목 / 줄글 / 이미지

# 학습

```bash
cd ./darknet

# Makefile로 환경 설치, 필요 패키지 컴파일 
make

# Start Training 
./darknet detector train ./news-yolo/custom_data/detector.data ./news-yolo/custom_data/newspaper-yolo.cfg
```

1. 깃 클론된 darknet안에서 make로 컴파일
2. ./darknet 하위에 darknet.sh, [darknet.so](http://darknet.so) 파일들이 생성됨 
3. cfg파일 수정해야함(학습 / 추론 시 다르게 해줘야 함)
- 학습시에는 testing부분을 주석처리하고 / 추론시에는 학습에 batch=64, subdivisions=16 부분을 주석처리
- 그외에도 클래스 개수, 필터 등 파라미터 수정

```python
#newspaper-yolo.cfg

[net]
# 학습 시킬때는 Testing 부분 막고, 추론 시킬때는 Training 부분 막기 
# Testing
#batch=1
#subdivisions=1
# Training
batch=64
subdivisions=16
width=608
height=608
channels=3
momentum=0.9
decay=0.0005
angle=0
saturation = 1.5
exposure = 1.5
hue=.1

learning_rate=0.001
burn_in=1000
max_batches = 500200       # 이터레이션
policy=steps
steps=400000,450000            # 이터레이션 x 배치 = 스텝
scales=.1,.1
```

4. custom.names 수정(g3 스토리지 경로/darknet/news-yolo/custom_data)

```python
# custom.names
headline
image
text
logo
```

5. detector.data 수정 (g3 스토리지 경로/darknet/news-yolo/custom_data/detector.data)

```python
# detector.data
classes=4
train=/content/drive/MyDrive/news/darknet/news-yolo/custom_data/train.txt
names=/content/drive/MyDrive/news/darknet/news-yolo/custom_data/custom.names
backup=/content/drive/MyDrive/news/darknet/backup/
```

6. train.txt 수정 (g3 스토리지 경로/darknet/news-yolo/custom_data/train.txt): 트레이닝 이미지들의 경로에서 이미지를 가져옴

- jpg 파일들만 받게 되어있음

```python
# train.txt
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/001.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/023.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/024.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/020.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/011.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/010.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/003.JPG
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/013.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/029.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/016.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/032.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/004.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/005.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/028.JPG
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/018.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/021.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/017.JPG
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/022.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/031.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/012.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/015.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/009.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/030.jpg
/content/drive/MyDrive/news/darknet/news-yolo/custom_data/images/014.jpg
```

### 학습 시작

GPU연결 안되어 있으면 진짜 1에폭 돌리는데도 10시간 걸림

연결되있으면 8200번 돌리는 데 8시간 정도 걸림

# 추론

```bash
#%cd /content/darknet/
./darknet detector test ./news-yolo/custom_data/detector.data  ./news-yolo/custom_data/newspaper-yolo.cfg ./backup/newspaper-yolo_800.weights ./data/rodong.jpg -thresh 0.5
```

- 명령어 마지막에 threshold 값(-thresh 0.5) 줘야함

![(https://assets/image/post_image/project/2022-01-07-Yolo-v3로-news-단락-구분-모델-만들기/[project]layer_info.png]
(https://assets/image/post_image/project/2022-01-07-Yolo-v3로-news-단락-구분-모델-만들기/[project]layer_info.png)

# 이슈

- 환경세팅할 때, make하면 [darknet.sh](http://darknet.sh) 만들어짐
- !export DISPLAY=:0 
추론된 이미지를 볼떄 x server error가 나는 데도 사진은 보여짐
- train.txt 파일 제일 아래에 빈칸이 있어서 오류가 났음
    
    ![https://assets/image/post_image/project/[project]2022-01-07-Yolo-v3로-news-단락-구분-모델-만들기/file_has_blank_in_final_line.png]
    (https://assets/image/post_image/project/[project]2022-01-07-Yolo-v3로-news-단락-구분-모델-만들기/file_has_blank_in_final_line.png)
    

## 추론 결과

max_batch = 8200

step = 6560, 7380

![https://illizard.github.io/assets/image/post_image/project/[project]2022-01-07-Yolo-v3로-news-단락-구분-모델-만들기/confidence_score.png.png]
(https://assets/image/post_image/project/[project]2022-01-07-Yolo-v3로-news-단락-구분-모델-만들기/confidence_score.png.png)

# 링크

<aside>
💡 욜로 모델로 영자 신문 데이터에 파인튜닝하기 [https://towardsdatascience.com/object-detection-on-newspaper-images-using-yolov3-85acfa563080](https://towardsdatascience.com/object-detection-on-newspaper-images-using-yolov3-85acfa563080)

</aside>

[https://zeuseyera.github.io/darknet-kr/2_YOLO/yolo.html](https://zeuseyera.github.io/darknet-kr/2_YOLO/yolo.html)

[https://eehoeskrap.tistory.com/370](https://eehoeskrap.tistory.com/370)

[https://stackoverflow.com/questions/56199478/what-is-the-purpose-of-ignore-thresh-and-truth-thresh-in-the-yolo-layers-in-yolo](https://stackoverflow.com/questions/56199478/what-is-the-purpose-of-ignore-thresh-and-truth-thresh-in-the-yolo-layers-in-yolo)

[https://webnautes.tistory.com/1186](https://webnautes.tistory.com/1186)

[https://twinparadox.tistory.com/601](https://twinparadox.tistory.com/601)

[https://eungbean.github.io/2018/12/04/EOD-cannot-connect-to-X-server-0.0/](https://eungbean.github.io/2018/12/04/EOD-cannot-connect-to-X-server-0.0/)

[https://askubuntu.com/questions/175611/cannot-connect-to-x-server-when-running-app-with-sudo](https://askubuntu.com/questions/175611/cannot-connect-to-x-server-when-running-app-with-sudo)

pytorch YOLOv3 custom data 학습시키기 (custom data 처리) [https://ys-cs17.tistory.com/27](https://ys-cs17.tistory.com/27)
