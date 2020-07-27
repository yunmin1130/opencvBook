# 템플릿 매칭

 - 입력 영상에서 특정 템플릿 영상을 찾아내는 방법
 - for 문을 돌아가며 템플릿 영상과의 유사도 혹은 비유사도 계산
 - 6가지의 매칭방법 이용
 - 정규화된 상관계수 매칭 방법이 가장 좋은 결과를 제공

```c++
void matchTemplate(InputArray image, InputArray templ, 
                    OutPutArray result, int method, InputArray mask = noArray());
```

변수 | 설명
--- |:---
image | 입력 영상
templ | 템플릿 영상
result | 비교 결과를 저장할 행렬. CV_32FC1
method | 템플릿 매칭 비교 방법. TemplateMatchModes 열거형 상수 중 하나를 지정
mask | 찾고자 하는 템플릿의 마스크 영상

```c++
void template_matching()
{
	Mat img = imread("circuit.bmp", IMREAD_COLOR);
	Mat templ = imread("crystal.bmp", IMREAD_COLOR);

	if (img.empty() || templ.empty()) {
		cerr << "Image load failed!" << endl;
		return;
	}

	img = img + Scalar(50, 50, 50);

	Mat noise(img.size(), CV_32SC3);
	randn(noise, 0, 10);
	add(img, noise, img, Mat(), CV_8UC3);

	Mat res, res_norm;
	matchTemplate(img, templ, res, TM_CCOEFF_NORMED);
	normalize(res, res_norm, 0, 255, NORM_MINMAX, CV_8U);

	double maxv;
	Point maxloc;
	minMaxLoc(res, 0, &maxv, 0, &maxloc);
	cout << "maxv: " << maxv << endl;

	rectangle(img, Rect(maxloc.x, maxloc.y, templ.cols, templ.rows), Scalar(0, 0, 255), 2);

	imshow("templ", templ);
	imshow("res_norm", res_norm);
	imshow("img", img);

	waitKey();
	destroyAllWindows();
}
```