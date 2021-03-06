# 어파인 변환

## 어파인 변환

영상의 기하학적 변환이며, 영상을 구성하는 픽셀의 배치 구조를 변경함으로써 전체 영상의 모양을 바꾸는 작업   
기하학적 변환은 픽셀 값은 유지하나, 위치를 변경하는 작업으로, 고유의 함수 형태로 표현 가능   
어파인 변환은 영상을 평행이동, 회전, 크기 변환을 하는 변환 종류의 통칭   
6개의 파라미터가 필요하며, 이를 2X3 행렬로 변환한 것이 `어파인 변환 행렬` 
  - 어파인 변환은 직선의 상태 등은 유지되어야 하므로, 평행사변형 이동이라 볼 수 있음
  - 평행사변형 이동이므로, 좌표는 3개 필요함((x1, y1), (x2, y2), (x3, y3))
  - 나머지 1개 좌표는 평행사변형이므로 자동 결정

[getAffineTransform()](https://docs.opencv.org/master/da/d54/group__imgproc__transform.html#ga8f6d378f9f8eebb5cb55cd3ae295a999) 함수 제공
- 참조 : [Affine Transformations](https://docs.opencv.org/master/d4/d61/tutorial_warp_affine.html)

**2X3 어파인 변환 행렬의 형태:**

기본 이동 수식: ![](images/affine_example_1.png)   
수학적 편의로 변경: ![](images/affine_example_2.png)


c++:

```cpp

```

python:

```python

```

---

## 이동 변환

- 가로 또는 세로 방향으로 일정 크기만큼 이동시키는 연산(Shift 연산)

**어파인 변환 행렬:**

```
대충 이동 변환 어파인 변환 행렬
```

**코드:**

```cpp

```

---

## 전단 변환

- 직사각형 형태의 영상을 한쪽 방향으로 밀어서 평행사변형 모양으로 변형되는 변환(총밀림 변환)
- 원점은 원점에 머물러 있는 상태

**어파인 변환 행렬:**

```
대충 이동 변환 어파인 변환 행렬
```

**코드:**

```cpp

```

---

## 크기 변환

- 전체적인 크기를 확대 또는 축소하는 변환
- 매우 빈번하게 사용하는 변환
- [resize()](https://docs.opencv.org/master/da/d54/group__imgproc__transform.html#ga47a974309e9102f5f08231edc7e7529d) 함수 기본 제공
- 보간법(Interpolation) 알고리즘 선택 가능

**어파인 변환 행렬:**

```
대충 이동 변환 어파인 변환 행렬
```

**보간법 지정 열거형 상수:**
- 표 정리 필요

**코드:**

```cpp

```

---

## 회전 변환

- 특정 좌표를 기준으로 영상을 원하는 각도만큼 회전하는 변환
- 문자 인식 OCR 시스템 등에서 사용됨
- [getRotationMatrix2D()](https://docs.opencv.org/master/da/d54/group__imgproc__transform.html#gafbbc470ce83812914a70abfb604f4326) 항수 기본 제공

**어파인 변환 행렬:**

```
대충 이동 변환 어파인 변환 행렬
```

**코드:**

```cpp

```

---

## 대칭 변환

- 좌우를 서로 바꾸거나, 상하를 뒤집는 형태의 변환
- 보간법이 필요하지 않음
- [flip()](https://docs.opencv.org/master/d2/de8/group__core__array.html#gaca7be533e3dac7feb70fc60635adf441) 항수 기본 제공

**어파인 변환 행렬:**

```
대충 이동 변환 어파인 변환 행렬
```

**코드:**

```cpp

```