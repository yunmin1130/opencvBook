# QR 코드 검출

 - `QRCodeDetector` 객체를 생성하여 검출 가능

```c++
bool QRCodeDetector::detect(InputArray img, OutputArray points) const;
```
변수 | 설명
--- |:---
img | 입력 영상.
points | QR 코드를 감싸는 사각형의 네 꼭지점 좌표
반환값 | QR 코드를 검출하면 true, 아니면 false

```c++
std::string QRCodeDetector::decode(InputArray img, InputArray points,
									OutputArray straight_qrcode = noArray());
```

변수 | 설명
--- |:---
img | 입력영상
points | QR 코드를 감싸는 사각형의 네 꼭지점 좌표
straigt_qrcode | 정사각형 QR 코드 영상
반환값 | QR 코드에 포함된 문자열

 - `straigt_qrcode` 에는 정사각형 형태로 투영 변환된 QR 코드 영상이 반환된다.
 - `QRCodeDetector::detectAndDecode()` 함수를 사용하면 검출과 해석을 한꺼번에 수행한다.

```c++
void decode_qrcode()
{
	VideoCapture cap(0);

	if (!cap.isOpened()) {
		cerr << "Camera open failed!" << endl;
		return;
	}

	QRCodeDetector detector;

	Mat frame;
	while (true) {
		cap >> frame;

		if (frame.empty()) {
			cerr << "Frame load failed!" << endl;
			break;
		}

		vector<Point> points;
		String info = detector.detectAndDecode(frame, points);

		if (!info.empty()) {
			polylines(frame, points, true, Scalar(0, 0, 255), 2);
			putText(frame, info, Point(10, 30), FONT_HERSHEY_DUPLEX, 1, Scalar(0, 0, 255));
		}

		imshow("frame", frame);
		if (waitKey(1) == 27)
			break;
	}
}
```