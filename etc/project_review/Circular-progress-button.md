# SimpleStateButton
CircularProgressButton 에서 "State Change Sample" 에서 사용한
기능만 분리해서 구현한다.

버튼의 변화를 'IDLE', 'ERROR', 'SUCCESS' 3 Level 로 나누어
사용자의 호출에 따라 버튼이 변하도록 구현한다.

## 속성
3 Level 에 대한 설정을 할 수 있도록 속성을 받는다.

## StateListDrawable
버튼의 상태(pressed, normal 등) 에 따라 drawable을 변경하기 위해 `StateListDrawable`을 사용한다.
3 Level에 대해 pressed, normal 에 해당하는 객체를 생성한다.

## MorphingAnimation
level이 변경될 때 버튼이 자연스럽게 표시되도록 animation을 구현한다.
CircularProgressButton 에서는 `MorphingAnimation` 을 사용해서 구현하고 있다.

MorphingAnimation 은 'width, bgColor, strokeColor, corner' 에 대한 Animation을
정의하고 사용하고 있다.



## 참고
[github](https://github.com/dmytrodanylyk/circular-progress-button)
