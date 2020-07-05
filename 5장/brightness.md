# 밝기

## 이미지 불러오기

[imread](https://docs.opencv.org/master/d4/da8/group__imgcodecs.html#ga288b8b3da0892bd651fce07b3bbd3a56)

파라미터:

- filename
- flags: [ImreadModes](https://docs.opencv.org/master/d4/da8/group__imgcodecs.html#ga61d9b0126a3e57d9277ac48327799c80)

c++:

```cpp
#include <opencv2/imcodecs.hpp>

Mat cv::imread(const String & filename, 
                 int          flags = IMREAD_COLOR)

// 이미지를 불러올 수 없을 때
Mat::data==NULL
```

python:

```python
retval = cv.imread(filename[, flags])
```

- 파일 확장자가 아닌 이미지 타입으로 결정한다.
- 컬러: **B G R**
- 그레이스케일: `IMREAD_GRAYSCALE`


```cpp
Mat origin = imread("lenna.bmp", IMREAD_COLOR);
Mat grayscale = imread("lenna.bmp", IMREAD_GRAYSCALE);

Mat color2Gray;
cvtColor(origin, color2Gray, COLOR_BGR2GRAY);

Mat emptyGrayScale(480, 640, CV_8UC1, Scalar(0));
```

---

## 수식

**밝기 조절 방정식:**

![dst(x, y) = src(x, y) + n](https://latex.codecogs.com/svg.latex?dst(x,%20y)%20=%20src(x,%20y)%20+%20n)

**dst 픽셀 값 범위:**

![0 \leq pixel \leq 255](https://latex.codecogs.com/svg.latex?0%20\leq%20pixel%20\leq%20255)

**포화 연산:**

![
saturate(x) = \left\{\begin{matrix}
0 & x < 0 \\
255 & x > 255 \\ 
x & 
\end{matrix}\right.
](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20saturate%28x%29%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%200%20%26%20x%20%3C%200%20%5C%5C%20255%20%26%20x%20%3E%20255%20%5C%5C%20x%20%26%20%5Cend%7Bmatrix%7D%5Cright.)

**스케일 범위를 제한한 밝기 조절 방정식:**

![dst(x, y) = saturate(src(x, y) + n)](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20dst%28x%2C%20y%29%20%3D%20saturate%28src%28x%2C%20y%29%20&plus;%20n%29)

---

## 예제

```cpp
Mat src = imread("lenna.bmp", IMREAD_GRAYSCALE);
Mat dst;
```

**밝기 조절 & 포화 연산:**

```cpp
dst = src + 100;
dst = src - 100;
src += 100;
```

**함수:**

```cpp
add(src, 100, dst);
subtract(src, 100, dst);
```

**픽셀 조절:**

```cpp
for (int j = 0; j < src.rows; j++) {
    for (int i = 0; i < src.cols; i++) {
        int v = src.at<uchar>(j, i) + 100; // 밝기 조절
        dst.at<uchar>(j, i) = saturate_cast<uchar>(v); // 포화 연산
    }
}
```
