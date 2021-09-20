# Moic Segmentation
Image segmentation based on DL



### Data

> [K-fashion Dataset](https://aihub.or.kr/aidata/30755) from AI-Hub

**원천 데이터:** 2020년 8월부터 11월까지 국내 패션 이미지 1000만 건 중 모델 얼굴 모자이크 작업 등 필터링을 거쳐 데이터 구축에 적합한 의류 착장 이미지 120만 건.

본 데이터는 2단계의 레이블링 작업과 2단계 검수 작업을 거쳐 학습데이터로 구축된 상태이다.



### Features

|       대분류       |                          세부 속성                           |
| :----------------: | :----------------------------------------------------------: |
|   상의 카테고리    |     탑, 블라우스, 티셔츠, 니트웨어, 셔츠, 브라탑, 후드티     |
|   하의 카테고리    |            청바지, 팬츠, 스커트, 레깅스, 조거팬츠            |
|  아우터 카테고리   |         코트, 재킷, 점퍼, 패딩, 베스트, 가디건, 짚업         |
|  원피스 카테고리   |                       드레스, 점프수트                       |
|        컬러        | 블랙, 화이트, 그레이, 레드, 핑크, 오렌지, 베이지, 브라운, 옐로우, 그린, 카키 … 실버 |
|       디테일       | 비즈, 단추, 니트꽈배기, 체인, 컷오프, 블브레스티드, 드롭숄더, 자수, 프릴, 프린지 … 퍼프 |
|       프린트       | 체크, 스트라이프, 지그재그, 호피, 지브라, 도트, 카무플라쥬, 페이즐리, 아가일 … 믹스 |
|        소재        | 퍼, 무스탕, 스웨이드, 헤어니트, 코듀로이, 시퀸, 데님, 저지, 니트 … 스판덱스 |
|     상의 기장      |                        크롭, 노멀, 롱                        |
|     하의 기장      |                미니, 니렝스, 미디, 발목, 맥시                |
|    아우터 기장     |                  크롭, 노멀, 하프, 롱, 맥시                  |
|    원피스 기장     |                미니, 니렝스, 미디, 발목, 맥시                |
|      소매기장      |               민소매, 반팔, 캡, 7부소매, 긴팔                |
|       넥라인       | 라운드넥, 유넥, 브이넥, 홀토넥, 오프숄더, 원숄더, 스퀘어넥, 노카라, 후드 … 스위트하트 |
|        칼라        | 셔츠칼라, 보우칼라, 세일러칼라, 숄칼라, 폴로칼라, 피터팬칼라, 너치드칼라 … 밴드칼라 |
| 상의,아우터,원피스 |                타이트, 노멀, 루즈, 오버사이즈                |
|        하의        |              스키니, 노멀, 와이드, 루즈, 벨보텀              |



### Model

- **FCN (Fully Convolutional Network)**
  - 기존 Classification용 CNN 모델(AlexNet, VGG 등)의 경우 물체가 어떤 class에 속하는지는 예측할 수 있지만 parameter의 개수와 차원을 줄이는 layer들을 가지고 있어서, 자세한 위치정보를 잃고 물체가 어디 존재하는지 예측할 수 없데 됨.
  - FCN의 경우 fully connected 층을 1x1 convolution 층으로 바꿈
  - 이미지의 크기와 상관 없이 segmentation map을 만들 수 있게 됨

- ~~Faster RCNN: CNN + RPN~~
  - 정확하고, 인식률이 좋다.
  - 느리다. 애초에 실시간용으로 개발된 것이 아니기에.
- YOLO(You Only Look Once)
  - 최초의 Real-Time Object Detection
  - 빠르고 사용 쉽다. v3의 경우 비교적 정확
  - 겹쳐진 사물의 구분 어렵다.
- SSD(Single Shot MultiBox Detector)
  - 비교적 빠르고 비교적 정확하다
  - YOLO에 비해 사용이 쉽지는 않다.



### Development  

Clothes Segmentation은 이미지(영상 프레임) 내 의류의 위치를 픽셀 단위로 예측하는 과정이며, 이미지 내 각 픽셀에 대해 클래스를 예측하는 방향으로 진행된다.

Clothes Segmentation을 크게 아래 3가지의 영역으로 분류해보자.

- Semantic Segmentation : 같은 클래스를 갖는 물체들에게 객체 개념 없이 모두 같은 클래스로 인식 (e.g. top, trouser, ...)
- Instance Segmentation : 같은 클래스를 갖는 물체들에게 객체 개념을 부여 (e.g. top1, top2, hat1, hat2, hat3, ...)
- Panoptic Segmentation : Semantic Segmentation과 Instance Segmentation이 합쳐진 개념 (엄밀하게 조금 다름.) 으로, 두 가지 특성을 모두 갖는 분야

이중 가장 기본이 되는 Semantic Segmentation부터 시도해본다.



Semantic Segmentation은 FCN (Fully Convolutional Network)를 기준으로 DeepLab 계열 모델과 U-Net 계열 모델로 구분지을 수 있다.





