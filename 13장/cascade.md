# 캐스케이드 분류기와 얼굴 검출

 - 24x24 크기로 영상을 정규화후 유사-하르 필터 집합으로 특징 정보 추출
 - 흰색 영역 픽셀은 덧셈, 검은색 영역 필셀은 뺄셈

### 에이다부스트 알고리즘

 - 유사-하르 필터 중에서 얼굴 검출에 효과적
 - 24x24 부분 영상에서 특징 개수 6천개로 감소

### 비올라 존스 알고리즘

 - 캐스케이드 구조 도입
 - 얼굴이 아닌 영역을 걸러내는 구조
 - 적은 특징사용 -> 점차 많은 특징 사용으로 걸러낸다
 - `CascadeClassifier` 구조체 제공

 ```c++
 void CascasdeClassifier::load(const String& filename);
 CascadeClassifier::CascadeClassifier(const String& filename);
 ```

 변수 | 설명
 --- |:---
 filename | 불러올 분류기 XML 파일 이름

 OpenCV는 미리 훈련된 검출을 위한 분류기 XML 파일 제공. 이 파일들은 %OPENCV_DIR%\etc\haarcascades 폴더에 있다.

 ```c++
 bool CascadeClassifier::empty() const
 ```

 - 분류기 파일을 정상적으로 불러오면 false, 아니면 true 반환

```c++
void CascadeClassifier::detectMultiScale(InputArray image, vector<Rect>& objects,
                                        double scaleFactor = 1.1, int minNeighbors = 3,
                                        int flags = 0, Size minSize = Size(),
                                        Size maxSize = Size());
```

변수 | 설명
--- |:---
image | 입력 영상
objects | 검출된 객체의 사각형 좌표 정보
scaleFactor | 검색 윈도우 확대 비율 >1
minNeighbors | 검출 영역으로 선택하기 위한 최소 검출 횟수
flags | 사용X
minSize | 검출할 객체의 최소 크기
maxSize | 검출할 객체의 최대 크기

 - image 에서 객체 사각형 영역 검출
 - 3컬러이면 그레이스케일로 변환하여 사용

```c++
void detect_eyes()
{
	Mat src = imread("kids.png");

	if (src.empty()) {
		cerr << "Image load failed!" << endl;
		return;
	}

	CascadeClassifier face_classifier("haarcascade_frontalface_default.xml");
	CascadeClassifier eye_classifier("haarcascade_eye.xml");

	if (face_classifier.empty() || eye_classifier.empty()) {
		cerr << "XML load failed!" << endl;
		return;
	}

	vector<Rect> faces;
	face_classifier.detectMultiScale(src, faces);

	for (Rect face : faces) {
		rectangle(src, face, Scalar(255, 0, 255), 2);

		Mat faceROI = src(face);
		vector<Rect> eyes;
		eye_classifier.detectMultiScale(faceROI, eyes);

		for (Rect eye : eyes) {
			Point center(eye.x + eye.width / 2, eye.y + eye.height / 2);
			circle(faceROI, center, eye.width / 2, Scalar(255, 0, 0), 2, LINE_AA);
		}
	}

	imshow("src", src);

	waitKey();
	destroyAllWindows();
}
```