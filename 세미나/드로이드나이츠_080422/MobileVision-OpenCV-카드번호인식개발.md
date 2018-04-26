# 소개
장혁재 : linked-in/hyukjae-jang
김종민 : github.com/devetude

# 카드인식개발
LinePay 에서 카드 인식 사용
양각카드가 아닌 프린트된 신용카드 인식
멤버쉽 카드 포함 범용 카드 인식
바코드 , QR 코드

## card.io
카드 테두리 찾기 > ISO 표준 비율로 변환 > 수직위치 찾기 > 수평위치 찾기 > 전처리 > 숫자인식
카드영역추출, 카드번호 위치찾기, 번호인식
## 왜 양각만 되나? 양각 알고리즘
Fullyconnected nerual network 을 이용한 수직 위치 찾기
Convolution neural network 를 이용한 숫자인식

## Mobile VIsion
google 에서 만듦

## OpenCV 를 활용한 자동차 번호판 인식, 숫자 위치 찾기
비슷한 크기의 문자가 일정 개수 이상 수평으로 나열

### alogirithm
Canny, Contour, Poly 적용 > 크기로 필터링 > 수평으로 연속된 영역 그룹으로 묶기

## 전체 구조도
양각된 숫자는 ocr 에서 인식이 안됨

## 바코드 인식
바코드 인식을 하기에 해상도가 너무 낮음 (card.io) 문제
### 해결
preview 를 키워보자
### Side-effect
기기마다 preview, 가로세로비율, preview format

## 문자 인식
해상도(35x428 pixels) 인식 불가능
입력 이미지의 외곽 부분은 인식 못함 (최소 40pixels)
숫자를 영문자로 오인식
뒤집어진 글자는 인식 불가능
card.io 내부 처리와 충돌해서 카드 이미지가 멋대로 뒤집혀서 처리됨

## 이미지 선명도와 인식 정확도
카드 테두리 인식 못함
focus : laplacian, meanStdDev (Sharpness 계산)
너무 느린 인식 속도와 낮은 정확도
1장의 선명한 사진
onAutoFocus, FOCUSED_LOCKED > 2s timeout 사용 으로 촬용

## 직관적인 결과 수정
DragShow를 사용한 Drag & Drop 수정 UI
Hidden buttons

## 잡다한 이야기
### Google Mobile Vision 학습데이터 설치
meta-data 를 넣으면 자동으로 다운 받음
학습데이터 앱 외부에 설치

### cardio 코드 재활용
jar 배포를 염두에 둔 오래된 코드

### Machine Learning, MNIST, Deel learning
카드 이미지 수집도 어렵습니다






[card.io-Android-SDK](https://github.com/card-io/card.io-Android-SDK)
[card.io](https://github.com/card-io/card.io-Android-source)
