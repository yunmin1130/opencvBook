# HOG 알고리즘과 보행자 검출

 - HOG(Histograms of Oriented Gradients)
 - 말 그대로 그래디언트 방향 히스토그램이다.
 - 그래디언트의 크기와 방향을 이용해 특징 벡터 정의
 - 이후 SVM(Support Vector Machine) 알고리즘을 이용해 객체 검출
 - 64x128 크기의 영상 계산
 - 8x8 단위를 쉘, 16x16 단위는 블록이다.
 - 쉘에서 20도 단위로 방향성분을 구분하여 9개의 방향 히스토그램 생성

```c++
static std::vector<float> HOGDescriptor::getDefaultPeopleDetector();
```
 
 - 보행자 검출을 위해 훈련된 분류기 계수

```c++
virtual void HOGDescriptor::setSVMDetector(InputAray svmdetector);
```

변수 | 설명
--- |:---
svmdetector | 선형 SVM 분류기를 위한 계수

 - 보행자 검출이 목적이라면 `HOGDescriptor::getDefaultPeopleDetector()`함수를 인자로 전달하면 된다.

```c++
virtual void HOGDescriptor::detectMultiScale(InputArray img, 
											std::vector<Rect>& foundLocations,
											std::vector<double>& foundWeights,
											double hitThreshold = 0,
											Size winStride = Size(),
											Size padding = Size(),
											double scale = 1.05,
											double finalThreshold = 2.0,
											bool useMeanshiftGrouping = false) const;
```

변수 | 설명
--- |:---
img | 입력 영상.
foundLocations | 검출된 사각형 영역 정보
foudnWeights | 검출된 사각형 영역에 대한 신뢰도
hitThreshold | 특징 벡터와 SVM 분류 평면까지의 거리에 대한 임계값
winStride | 쉘 윈도우 이동 크기
padding | 패딩 크기
scale | 검색 윈도우 크기 확대 비율
finalThreshold | 검출 결정을 위한 임계값
useMeanshiftGrouping | 겹쳐진 검색 윈도우를 합치는 방법 지정 플래그

 - 객체 영역 검출
 - 객체 사각형 영역과, 신뢰도를 함께 반환한다.

```c++
int main()
{
	VideoCapture cap("vtest.avi");

	if (!cap.isOpened()) {
		cerr << "Video open failed!" << endl;
		return -1;
	}

	HOGDescriptor hog;
	hog.setSVMDetector(HOGDescriptor::getDefaultPeopleDetector());

	Mat frame;
	while (true) {
		cap >> frame;
		if (frame.empty()) 
			break;

		vector<Rect> detected;
		hog.detectMultiScale(frame, detected);

		for (Rect r : detected) {
			Scalar c = Scalar(rand() % 256, rand() % 256, rand() % 256);
			rectangle(frame, r, c, 3);
		}

		imshow("frame", frame);

		if (waitKey(10) == 27)
			break;
	}

	return 0;
}
```