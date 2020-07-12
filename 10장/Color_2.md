# 컬러 영상 처리 기법

## 컬러 히스토그램 평활화

 - `equalizeHist()`함수는 그레이스케일 영상만 입력 가능
 - BGR 영상을 각 채널별로 히스토그램 평활화를 수행하면 원본과 다른 영상이 만들어진다.
 - 각 채널간의 비율이 깨지게 되면서 전혀 다른 영상이 만들어진다.

 (대충 평활화 하면 안되는 사진)

 - 컬러 영상에서 히스토그램 평활화를 하려면 명암 정보만을 사용해야 함
 - YCrCb 색 공간을 사용할 경우 Y 성분에 대해서만 히스토그램 평활화 수행
 - 색 정보는 변하지 않고 명암비만 증가하게 됨

 (컬러 영상 히스토그램 에제 사진)

## 색상 범위 지정에 의한 영역 분할

 - HSV 색 공간은 색상 정보가 따로 있어 특정 생상 영역 추출하기 유리
 - 녹생의 경우 H 값이 60에 가까운지 조사하여 추출 가능
 - 입력 채널이 두 개 이상이면 두 채널의 픽셀 값이 지정 범위를 만족할 때 흰색으로 지정
 - Mat 객체를 upperb, lowerb 로 지정할 경우 모든 픽셀에 각기 다른 범위 설정 가능

```c++
void inRange(InputArray src, InputArray lowerb, InputArray upperb, OutputArray dst);
```

변수 | 설명
--- |:---
src | 입력 영상
lowerb | 하한 값. 주로 Mat 또는 Scalar 객체를 지정
upperb | 상한 값. 주로 Mat 또는 Scalar 객체를 지정
dst | 출력 마스크 영상. 입력 영상과 크기가 같고, 타입은 CV_8UC1

 `inRage()`함수는 src의 픽셀 밗이 지정 범위에 포함되어 있으면 흰색, 아니면 검은색으로 채워진 마스크 영상을 출력한다. 만일 입력 영상이 그레이스케일 영상일 경우 특정 밝기범위의 픽셀 영역을 추출할 수 있다. 

 (대충 dst 수식)

 (대충 트랙바 넣은 예제 및 사진)

## 히스토그램 역투영

 - `inRange()`함수는 부색 같은 미세한 변화가 있는 색을 찾기 어려움
 - 기준 영상으로 부터 찾고자 하는 객체의 컬러 히스토그램을 구하고, 이를 이용해 히스토그램과 부합하는 영역을 찾아냄
 - `calcBackProject()`함수를 이용해 히스토그램 역투영을 적용할 수 있음

```c++
void calcBackProject(const mat* images, int nimages,
                    const int* channels, InputArray hist,
                    OutputArray backProject, const float** ranges,
                    double scale = `, bool uniform = true);
```

변수 | 설명
--- |:---
images | 입력 영상의 배열 또는 입력 영상의 주소. 영상의 배열인 경우, 모든 영상의 크기와 깊이는 같아야 한다.
nimages | 입력 영상 개수
channels | 역투영 계산 시 사용할 채널 번호 배열
hist | 입력 히스토그램
backProject | 출력 히스토그램 역투영 영상. 입력 영상과 같은 크기, 같은 깊이를 갖는 1채널 행렬
ranges | 각 차원의 히스토그램 빈 범위를 나타내는 배열의 배열
scale | 히스토그램 역투영 값에 추가적으로 곲할 값
uniform | 히스토그램 빈의 간격이 균등한지를 나타내는 플래그