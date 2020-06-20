# 4. OpenCV 주요 기능

## 4.1 카메라와 동영상 파일 다루기

영상을 관리 할 수 있는 `VideoCapture` 클래스를 중심으로 카메라 입력 처리 방법, 동영상 입력 처리 방법 및 저장 방법 등에 대해 소개한다.

### 4.1.1 VideoCapture 클래스

 OpenCV 에서는 `VideoCapture`라는 하나의 클래스를 이용하여 카메라 또는 동영상 파일로부터 정지된 영상 프레임을 받아 올 수 있다.

 ![간략화한 VideoCapture 클래스](Code_4_1.PNG)

 #### VideoCapture::open()

 `VideoCapture::open()`을 이용해 동영상 파일을 불러오거나 카메라 입력 영상을 불러올 수 있다.

 ```c++
VideoCapture::VideCapture(const String& filename, int apiPreference = CAP_ANY);
bool VideoCapture::open(const String& filename, int apiPreference = CAP_ANY);
 ```
 변수 | 의미 
 ---|:---
 `filename` | 동영상 파일 이름
 `apiPreference` | 사용할 비디오 캡처 API 백엔드
 반환값 | (`VideoCapture::open()`함수)열기가 성공하면 true, 실패하면 false

 먼저 동영상 파일을 불러오는 방법이다. 동영상 파일을 불러오려면 `VideoCapture` 객체를 생성할 때 생성자에 동영상 파일 이름을 지정하거나 객체 생성 후 `VideoCapture::open()` 멤버 함수를 호출해야 한다. `filename` 인자에는 동영상 파일 확장자를 갖는 동영상 파일, 사진 혹은 비디오 스트림 URL을 지정하여 사용할 수 있다.

 ```c++
VideoCapture::VideCapture(int index, int apiPreference = CAP_ANY);
bool VideoCapture::open(int index, int apiPreference = CAP_ANY);
 ```
 변수 | 의미 
 ---|:---
 `index` | 카메라와 장치 사용 방식 지정 번호
 `apiPreference` | 사용할 카메라 캡처 API 백엔드
 반환값 | (`VideoCapture::open()`함수)열기가 성공하면 true, 실패하면 false

 카메라 장치를 사용하기 위해서도 `VideoCapture` 객체를 생성해야 한다. `VideoCapture::open()` 함수에 전달하는 index는 다음과 같은 형태로 구성된다

 ```c++
 index = camera_id + domain_offset_id
 ```

 카메라 또는 동영상 파일을 연 다음에는 `VideoCapture::isOpened()` 멤버 함수를 이용해 성공적으로 열었는지 확인할 수 있다. 카메라 장치 혹은 동영상 파일의 사용이 끝나면 `VideoCapture::release()` 멤버 함수를 호출하여 사용하던 자원을 해제할 수 있다. `VideoCapture::release()` 함수의 경우 소멸자에서 자동 호출한다.

 ```c++
 VideoCapture& VideoCapture::operator >> (Mat& image);
 bool VideoCapture::read(OutputArray image);
 ```
  변수 | 의미 
 ---|:---
 `image` | 다음 비디오 프레임. 만약 더 가져올 프레임이 없다면 비어 있는 행렬로 설정된다.
 반환값 | 프레임을 받아 올 수 없으면 false

`>>` 연산자는 `VideoCapture::read()`함수를 호출하는 형태로 구현되어 있다. `VideoCapture::read()`함수는 `VideoCapture::grab()`함수와 `VideoCapture::retrieve()`함수를 합쳐놓은 것이다.
함수|의미
---|:---
`VideoCapture::grab()` | 카메라 장치에 다음 프레임을 획득하라는 명령을 내린다.
`VideoCapture::retrieve()` | 획득한 프레임을 실제로 받아온다.

 ```c++
 double VideoCapture::get(int propId) const;
 ```
  변수 | 의미 
 ---|:---
 `propId` | 속성 ID. VideoCaptureProperties 열거형 중 하나를 지정한다.
 반환값 | 지정한 속성 값. 만약 지정한 속성을 얻을 수 없으면 0을 반환한다.

 `VideoCapture::get()` 함수는 지정한 속성 ID에 해당하는 속성 값을 반환한다. 이 함수는 double 자료형으로 속성 값을 반환하는데, 만일 정수형 변수에 저장하려면 `cvRound()` 함수를 사용하는게 좋다.

  ```c++
 double VideoCapture::set(int propId, double value);
 ```
  변수 | 의미 
 ---|:---
 `propId` | 속성 ID. VideoCaptureProperties 열거형 중 하나를 지정한다.
 `value` | 지정할 속성 값
 반환값 | 속성 지정이 가능하면 true, 아니면 false를 반환한다.

 `VideoCapture::set()` 함수는 지정한 속성 ID에 해당하는 속성 값을 `value`로 지정한다.