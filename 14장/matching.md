# 특징점 매칭

두 영상에서 추출한 특징점 기술자를 비교하여 서로 비슷한 특징점을 찾는다.

[DMatch](https://docs.opencv.org/master/d4/de0/classcv_1_1DMatch.html): 특징점 매칭 정보 저장

- Brute-Force: 모든 기술자 집합의 거리를 조사
- Fast Library approximate nearest neighbors: 근사화된 최근방 이웃을 구현한 라이브러리

Homography: 3차원 공간상의 평면을 서로 다른 시점에서 바라봤을 때 획득되는 영상 사이의 관계. 투시 변환과 같다.
