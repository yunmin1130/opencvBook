# 투시 변환

## 투시 변환

- 어파인 변환보다 자유도가 높음
- 직사각형 형태의 영상을 임의의 볼록 사각형 형태로 변경할 수 있음
- 두 직선 간의 평행 관계는 깨어질 수 있음
- 점 네 개의 이동 관계에 의해 결정됨
- 3X3 형태의 행렬로 표현
  - 총 8개의 파라미터(4개의 좌표, ((x1, y1), (x2, y2), (x3, y3), (x4, y4))) 로 표현 가능하지만, 편의상 3X3으로 표현
- [perspectiveTransform()](https://docs.opencv.org/master/d2/de8/group__core__array.html#gad327659ac03e5fd6894b90025e6900a7) 함수 제공
- 결과 영상을 확인하려면 [warpPerspective()](https://docs.opencv.org/master/da/d54/group__imgproc__transform.html#gaf73673a7e8e18ec6963e3774e6a94b87) 함수를 사용

**3X3 어파인 변환 행렬의 형태:**
```
대충 어파인 함수가 들어갈 자리, 2X3이 나오는 과정을 보여주기
```

c++:

```cpp

```

python:

```python

```