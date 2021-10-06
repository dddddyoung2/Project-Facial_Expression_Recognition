# 얼굴표정인식 프로젝트(Facial Expression Recognition)
(해당 프로젝트는 아래의 링크를 참고했습니다.)
https://github.com/jy6zheng/FacialExpressionRecognition


## 환경 & 분석 방법
- Colab
- 딥러닝 CNN-ResNet34 
- fast.ai 라이브러리 https://docs.fast.ai/install.html



## 데이터셋
https://www.kaggle.com/jonathanoheix/face-expression-recognition-dataset
- 흑백 이미지 데이터
- 7가지 감정으로 분류(angry, disgust, fear, happy, neutral, sad, surprise)
- '알집파일을 구글 드라이브 업로드 -> colab에서 구글 드라이브 마운트 -> 압축풀기' 추천




## 얼굴 표정 인식이란?
얼굴 표정 인식이란 사람의 얼굴을 인식하고 기분에 따라 변하는 표정을 감지해 수시로 정확히 읽어내는 딥러닝 기술을 말합니다.

얼굴 표정 인식 관련 알고리즘은 Sarrafzadeh et al. (2008)부터 시작되었습니다.
기존 연구들은 Support Vector Machine (SVM)이 가장 많이 사용되었으며
그 외 Deep Belief Network(DBN), Convolutional Neural Network(CNN), Principal Component Analysis(PCA) 등이 사용되었습니다(딥러닝 표정 인식을 활용한 실시간 온라인 강의 이해도 분석 학술 자료 참고).


![image](https://user-images.githubusercontent.com/79177935/129285381-af809361-23a5-45cc-886c-39ad589a6fd2.png)

한 사람이 느끼는 감정들은 표정으로 드러나는 경우가 많은데
행복하고, 슬프고 이러한 감정을 인공지능은 어떻게 판단하는 것일까요?



## 모델 학습 과정
1. 데이터 전처리: 잘못 분류된 이미지 데이터 정리


2. 분류 모델 훈련을 위해 PyTorch의 fast.ai 라이브러리 사용
- fast.ai란 모델을 쉽게 학습할 수 있도록 도와주고, Learning rate(학습률)을 찾기 위한 시스템적인 방법을 제공하는 라이브러리로 소개
- 참고 자료: https://www.bookstack.cn/read/th-fastai-book/spilt.5.0661b9d7375f45ab.md



3. Convolutional Neural Network(CNN)의 ResNet34로 모델 학습    
- 이전의 방법보다 성능을 개선시키고 학습 시간을 줄인 방법
- ResNet 34은 마이크로소프트에서 제안한 구조로 34개의 레이어를 가지고, 3x3 층이 반복 추가된다는 점에서 VGG와 유사한 구조
- 참고 자료: https://hoya012.github.io/blog/deeplearning-classification-guidebook-1/


4. 기본 데이터에 가장 적합한 Learning rate을 찾아 적용
- Learning rate은 아주 작은 학습률로 시작해 손실이 얼마인지 파악한 다음 학습률을 일정 비율 높이면서 손실이 더 심해질 때까지 반복하는 방법
![image](https://user-images.githubusercontent.com/79177935/129286521-12e79f4f-b2d4-43cd-bbec-beef6576e6b2.png)



## 모델 학습 결과
1. Learning rate을 그래프를 통해 손실이 낮은 구간으로 자르고 학습 시킨 결과, 정확도 66%를 가지는 결과가 나왔습니다.

2. Confusion matrix를 통해 실제값과 예측값을 비교해본 결과, 'happy'가 가장 잘 분류되었으며 'disgust'의 감정은 다른 데이터에 비해 데이터 수가 부족했고 잘 분류되지 않는 것으로 보였습니다. 'angry'의 감정과 데이터를 합치는 것도 고려해야 할 것 같습니다.

3. 손실값이 가장 높은 샘플 데이터 이미지를 출력한 결과
![image](https://user-images.githubusercontent.com/79177935/129286805-dd917f2a-f07f-4c4f-bd32-5d1ba26dfe89.png)
왼쪽은 모델이 예측한 감정이고 오른쪽은 원래 분류된 감정인데,
원래 분류된 감정보다 예측된 감정이 더 적절해 보이는 경우가 많았습니다.



## 캠을 통해 실시간 감정 인식 적용 예시
학습한 모델을 OpenCV를 통해 영상에 적용하면 실시간으로 얼굴 표정을 인식할 수 있으며 감정을 확률로 보여줍니다. 눈의 모양과 입의 모양의 픽셀값들이 실시간으로 계산되는 것입니다.

![image](https://user-images.githubusercontent.com/79177935/136243790-38a4f989-0e51-4e74-8402-486bf0d88588.png)



## 모델의 한계
1. 억지로 보인 미소가 'happy'의 감정으로 예측될 가능성
 -> 눈과 입이 함께 움직일 때, happy로 분류하는 등 정교한 분석이 필요할 것으로 보입니다.

2. 데이터셋에서 감정 분류가 제대로 되지 않은 이미지가 다수
 ->데이터 전처리 과정을 추가 진행하여 모델의 성능을 개선할 필요가 있습니다.

3. 하나의 모델이라는 점
 -> ResNet34 외의 방법을 통해 모델을 만든 후 비교해 볼 필요가 있습니다.



## 활용 방안
1. 게임 몰입도 증진에 활용할 수 있습니다.

![image](https://user-images.githubusercontent.com/79177935/129287149-7d9fb624-2f20-4f77-8d04-7313a0ed7f34.png)

감정 분류에 지루함과 집중함 감정 추가하고
실시간으로 지루한 감정이 포착되면 
게임 중 장애물을 내보내 게임에 집중하게 만드는 것입니다.

2. AI 상담에 활용할 수 있습니다.

![image](https://user-images.githubusercontent.com/79177935/129287245-6b3fd302-613d-4b16-8c0f-4a14bd45ee1a.png)

고민의 카테고리별로 상담 데이터를 모으고,
친숙한 캐릭터를 골라 상담을 진행할 수 있습니다.
카메라를 통해서 실시간으로 감정과 행동을 파악해
캐릭터의 대사 중 상황과 적절한 대사를 내보내는 것입니다.



이처럼 얼굴 표정 인식은 다양하게 활용할 수 있습니다!


Thank you 😘
