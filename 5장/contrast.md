# 대비

---

## 수식

**간단한 명암 조절 방정식:**

![dst(x, y) = saturate(s \cdot src(x, y))](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20dst%28x%2C%20y%29%20%3D%20saturate%28s%20%5Ccdot%20src%28x%2C%20y%29%29)

**효과적인 명암 조절 방정식:**

중간값 128 이나 영상의 평균 밝기를 기준으로 명암을 조절한다.

![dst(x, y) = saturate(src(x, y) + (src(x, y) - 128) \cdot \alpha)](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20dst%28x%2C%20y%29%20%3D%20saturate%28src%28x%2C%20y%29%20&plus;%20%28src%28x%2C%20y%29%20-%20128%29%20%5Ccdot%20%5Calpha%29)

**알파 범위:**

![-1 \leq \alpha](https://latex.codecogs.com/svg.latex?%5Cdpi%7B120%7D%20-1%20%5Cleq%20%5Calpha)

- 0과 같거나 작을 때: 명암비 감소
- 0보다 클 때: 명암비 증가

---

## 예제

**간단한 명암 조절:**

```cpp
float s = 2.f;
Mat dst = s * src;
```

**효과적인 명암 조절:**

```cpp
float alpha = 1.f;
Mat dst = src + (src - 128) * alpha;
```

**평균값을 사용한 픽셀 조절:**

```cpp
float alpha = 1.f;
float average = mean(src)[0]; // 밝기 평균: [124.049, 0, 0, 0]

for (int j = 0; j < src.rows; j++) {
    for (int i = 0; i < src.cols; i++) {
        int v = src.at<uchar>(j, i) + (src.at<uchar>(j, i) - average) * alpha; // 명암 조절
        dst.at<uchar>(j, i) = saturate_cast<uchar>(v); // 포화 연산
    }
}
```
