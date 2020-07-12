# 컬러 영상 다루기

## 컬러 영상의 픽셀 값 참조

 - `Mat::at()`함수를 사용해 픽셀 참조 가능
 - 템플릿으로 정의되어 있어 다음과 같이 나타낼 수 있음 `Vec3b$ pixel = img.at<Vec3b>(x, y)`
 - `Mat::ptr()`함수를 이용해 시작 주소를 얻어 올 때도 자료형 명시해야 함
 - `Vec3b* ptr = img.ptr<Vec3b>(x)`

 (색 반전 코드와 사진 링크)

## 색 공간 변환
 
 - RGB 색 공간은 영상 처리에선 좋지 못 한 자료형
 - 색상 구분이 용이한 HSV, HSL 색 공간
 - 휘도 성분이 구분되어 있는 YCrCb, YUV

```c++
void cvtColor(InputArray src, OutputArray dst, int code, int dstCn = 0);
```

변수 | 설명
--- |:---
src | 입력 영상. CV_8U, CV_16U, CV_32F 중 하나의 깊이를 사용
dst | 결과 영상. src와 크기 및 깊이가 같다
code | 색 공간 변환 코드. ColorConversionCodes 열겨형 상수 중 하나를 지정
dstCn | 겨로가 영상의 채널 수. 0이면 자동으로 결정

 `cvtColor()` 함수는 입력 영상의 색 공간을 변환하여 보여준다. 여기서는 사용성이 높은 몇 가지 색 공간 변환에 대해 설명한다.

### BGR2GRAY & GRAY2BGR

 - 영상처리에서 색상 정보 활용도가 높지 않은 경우 그레이 스케일로 변환하여 처리
 - `Y = 0.299R + 0.587G + 0.114B`
 - GRAY2BGR 의 경우 BGR 모든 색상 공간에 같은 픽셀 값을 넣게 되어 여전히 회색으로 보임

### BGR2HSV & HSV2BGR

 - HSV 모델은 색상, 채도, 명도로 표현, 원뿔 형태로 표현 가능
 - H 는 색으로 0 ~ 360 의 값을 갖게 됨, 원뿔에서 원의 각도로 정의, 보통 0~179의 정수로 표현
 - S 는 채도로 색의 순도를 나타냄, 원뿔에서 원의 중심으로 부터 거리로 정의
 - V 는 명도로 밝고 어두움을 나타냄, 원뿔의 축을 따라 올라가면서 증가

 [!HSV](HSV.jpg)

### BGR2YCrCb & YCrCb2BGR

 - Y 성분은 밝기 또는 휘도, BGR 에서의 그레이 스케일 계산 공식과 같은 공식으로 구함
 - Cr, Cb 성분은 색상 또는 색차 정보
 - YCrCb는 영상을 그레이스케일 정보와 색상 정보로 분리하여 처리할 때 유용

## 색상 채널 나누기

 - 컬러 영상을 다루다 보면 한 성분만을 필요로 하는 경우 발생
 - 이런 경우 3채널 Mat 객체를 1채널 Mat 객체 세 개로 분리해야 함

```c++
void split(const Mat& src, Mat* mvbegin);
void split(InputArray src, OutputArrayOfArrays mv);
```

변수 | 설명
--- |:---
src | 입력 다채널 행렬
mvbegin | 분리된 1채널 행렬을 저장할 Mat 배열 주소. 영상 배열 개수는 src 영상 채널 수와 같아야 한다.
mv | 분리된 1채널 행렬을 저장할 벡터

 `split()`함수를 이용하면 입력영상 src를 Mat 자료형의 배열 혹은 vector<Mat> 형식의 변수로 바꿀 수 있다. 반대로 1채널 행렬 배열을 합치려면 `marge()`함수를 이용해야 한다.

 ```c++
 void merge(const Mat* mv, size_t count, OutputArray dst);
 void merge(InputArrayOfArrays mv, OutputArray dst);
 ```

변수 | 설명
--- |:---
mv | 1채널 행렬을 저장하고 있는 배열 또는 벡터. 모든 행렬은 크기와 깊이가 같아야 한다.
count | (mv 가 Mat 타입의 배열인 경우) Mat 배열의 크기
dst | 출력 다채널 행렬

(BGR 컬러 영상 나누는 예제 및 사진)

