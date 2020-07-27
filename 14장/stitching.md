# 영상 이어 붙이기

## 파노라마(Panorama) 영상

여러 장의 영상을 서로 이어 붙여서 `하나의 큰 영상`을 만드는 기법  
사용할 영상 간 일정 비율 이상의 `겹치는 영역` 존재   
위치를 분간 가능한 `유효한 특징점`이 존재해야 함   
파노라마 영상 제작 순서   
- 특징점 검출 -> 매칭 수행을 통한 호모그래피 구하기

[FAST](https://docs.opencv.org/master/d5/d51/group__features2d__main.html#ga816d870cbdca71c6790c71bdf17df099)

**코드:**
c++:

```cpp
void FAST(InputArray image, std::vector<KeyPoint> & keypoints,
       int threshold, bool nonmaxSuppression = true);
```

**예시**

![](images/stitching_example_1.png)
