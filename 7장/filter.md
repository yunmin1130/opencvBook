# 영상의 필터링

## 필터링 연산 방법

 - 영상 처리에서 필터링은 원하는 정보만 통과시키고 원치 않는 정보는 걸러내는 작업
 - 마스크(mask) 라고 불르는 작은 크기의 행렬을 이용 (커널, 윈도우 라고 부르기도 함)
 - 1x3의 직사각형 행렬부터 7x7의 정방형 행렬까지 다양한 행렬이 필터링 연산에 사용
 - 보통 행렬 정중앙을 고정점으로 사용
 - 입력 영상과 마스크 행렬의 곱셈, 덧셈 연산을 이용해 필터링 연산
 - 입력 영상 외곽의 경우 `borderType`열거형 상수를 이용해 처리

 일반적인 필터링은 `filter2D()` 함수를 이용해 수행

 ```c++
 void filter2D(InputArray src, OutputArray dst, int ddepth,
                InputArray kernel, Point anchor = Point(-1, -1), 
                doubld delta = 0, int borderType = BORDER_DEFAULT);
 ```
 
 변수 | 의미
 --- |:---
 src | 입력 영상
 dst | 출력 영상. src와 같은 크기, 같은 채널 수를 갖는다
 ddepth | 결과 영상의 깊이
 kernel | 필터링 커널. 1채널 실수형 행렬
 anchor | 고정점 좌표, `Point(-1, -1)`을 지정하면 커널 중심을 고정점으로 사용
 delta | 필터링 연산 후 추가적으로 더할 값
 borderType | 가장자리 픽셀 확장 방식

## 엠보싱 필터링

 - 엠보싱은 영상에서 올록볼록한 형태로 만든 객체의 윤곽 또는 무늬를 띄도록 변환하는 필터
 - 보통 입력 영상에서 변화가 적은 평탄한 지역은 회색으로 설정, 엣지 부분은 좀더 밝거나 어둡게 설정하여 엠보싱 효과 적용
 - (그림)은 간단한 형태의 엠보싱 필터 마스크, 대각선 형태로 +1 혹은 -1 값이 지정되어 있는 3x3형태
 - 평탄할 경우 0에 가깝고 엣지일 경우 음수 혹은 양수의 형태를 갖게 됨

`대충 엠보싱 필터 코드 주소가 나타날 곳`

`대충 엠보싱 필터 사진이 나타날 곳`

