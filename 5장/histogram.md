# 히스토그램

Histogram: 영상의 픽셀 값 분포를 그래프 형태로 표현한 것

## 함수

### 히스토그램

[calcHist](https://docs.opencv.org/master/d6/dc7/group__imgproc__hist.html#ga4b2b5fd75503ff9e6844cc4dcdaed35d)

파라미터:

- images: 입력 영상의 배열 or 주소. 배열인 경우, 영상의 크기와 깊이는 같아야 한다.
- nimages: 입력 영상 개수
- channels: 히스토그램을 구할 채널을 나타내는 정수형 배열
- mask: 마스크 영상. 입력 영상과 같은 크기의 8비트 배열. 마스크 행렬의 원소 값이 0이 아닌 좌표의 픽셀만 계산에 사용된다. `Mat()` 또는 `noArray()`인 경우 영상 전체에 대한 히스토그램 계산
- hist: 출력 히스토그램. `CV_32F` 깊이를 사용하는 dims-차원 행렬
- dims: 출력 히스토그램 차원 수
- histSize: 각 차원의 히스토그램 배열 크기를 나타내는 배열. 각 차원의 히스토그램 bin 개수를 나타내는 배열
- ranges: 각 차원의 히스토그램 범위
  - `uniform=true`: `ranges[i]` = `[최솟값, 최대값)`으로 구성된 배열
  - `uniform=false`: `histSize[i] + 1`개의 원소로 구성된 배열
- uniform: bin 간격 균등 여부
- accumulate: 누적 플래그. `true`: 누적 계산.

c++:

```cpp
void cv::calcHist ( const Mat * images,
                    int nimages,
                    const int * channels,
                    InputArray mask,
                    OutputArray hist,
                    int dims,
                    const int * histSize,
                    const float ** ranges,
                    bool uniform = true,
                    bool accumulate = false )
```

python:

```py
hist = cv.calcHist(images, channels, mask, histSize, ranges[, hist[, accumulate]])
```

### 히스토그램 평활화

[equalizeHist](https://docs.opencv.org/master/d6/dc7/group__imgproc__hist.html#ga7e54091f0c937d49bf84152a16f76d6e)

c++:

```cpp
void cv::equalizeHist(InputArray mask, OutputArray hist)
```

python:

```py
hist = cv.equalizeHist(src, [dst])
```

---

## 수식

**히스토그램 스트레칭:**

선형 변환 기법. 그레이스케일 전 구간으로 변환.

![dst(x, y) = \frac{src(x, y) - G_{min}}{G_{max} - G_{min}} \times 255](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20dst%28x%2C%20y%29%20%3D%20%5Cfrac%7Bsrc%28x%2C%20y%29%20-%20G_%7Bmin%7D%7D%7BG_%7Bmax%7D%20-%20G_%7Bmin%7D%7D%20%5Ctimes%20255)

**히스토그램 스트레칭 직선 방정식:**

- 직선의 기울기: 255 / (G<sub>max</sub> - G<sub>min</sub>)
- y 절편: - 255	• G<sub>min</sub> / (G<sub>max</sub> - G<sub>min</sub>)

![
\begin{align*}
dst(x, y) &= \frac{255}{G_{max} - G_{min}} \times src(x, y) - \frac{255 \cdot G_{min}}{G_{max} - G_{min}} \\
&= \frac{src(x, y) - G_{min}}{G_{max} - G_{min}} \times 255
\end{align*}
](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20%5Cbegin%7Balign*%7D%20dst%28x%2C%20y%29%20%26%3D%20%5Cfrac%7B255%7D%7BG_%7Bmax%7D%20-%20G_%7Bmin%7D%7D%20%5Ctimes%20src%28x%2C%20y%29%20-%20%5Cfrac%7B255%20%5Ccdot%20G_%7Bmin%7D%7D%7BG_%7Bmax%7D%20-%20G_%7Bmin%7D%7D%20%5C%5C%20%26%3D%20%5Cfrac%7Bsrc%28x%2C%20y%29%20-%20G_%7Bmin%7D%7D%7BG_%7Bmax%7D%20-%20G_%7Bmin%7D%7D%20%5Ctimes%20255%20%5Cend%7Balign*%7D)

**히스토그램 누적 함수:**

- h(g): 그레이스케일 값이 g인 픽셀 개수

![H(g) = \sum_{0 \leq i \leq g} h(i)](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20H%28g%29%20%3D%20%5Csum_%7B0%20%5Cleq%20i%20%5Cleq%20g%7D%20h%28i%29)

**히스토그램 평활화:**

픽셀 값 분포를 전체 영역으로 골고루 나타나도록 변경한다.

- N: 픽셀 수
- L<sub>max</sub>: 최대 밝기 값. 255.

![dst(x, y) = round(H(src(x, y)) \times \frac{L_{max}}{N})](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20dst%28x%2C%20y%29%20%3D%20round%28H%28src%28x%2C%20y%29%29%20%5Ctimes%20%5Cfrac%7BL_%7Bmax%7D%7D%7BN%7D%29)

