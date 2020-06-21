# OpenCV

- [OpenCV](https://opencv.org/)
  - [Documentation](https://docs.opencv.org/master/)
  - [Releases](https://opencv.org/releases/)

## 설치

- [Introduction to OpenCV](https://docs.opencv.org/master/df/d65/tutorial_table_of_content_introduction.html)
  - [Installation in MacOS](https://docs.opencv.org/master/d0/db2/tutorial_macos_install.html)

### macOS

#### 참고

- [Xcode에 OpenCV 설치하기](https://dgrld.tistory.com/34)
- [OpenCV on Xcode](https://www.codementor.io/@ohasanli/opencv-on-xcode-142qxx3sl8)

#### 필요한 패키지

- CMake 3.9+
- Git
- Python 2.7+
- Numpy 1.5+

[Homebrew](https://brew.sh/index_ko)로 설치한다.

#### Cmake

```bash
brew install cmake
```

#### OpenCV

```bash
brew install opencv
# or
brew upgrade opencv
```

#### pkg-config

Xcode에서 OpenCV 사용하려면 라이브러리 조회 기능이 필요하다.  

```bash
brew install pkg-config
# or
brew upgrade pkg-config
```

OpenCV4의 플래그를 검색한다.

```bash
pkg-config --cflags --libs opencv4
```

결과:

```bash
-I/usr/local/Cellar/opencv/4.3.0_5/include/opencv4/opencv -I/usr/local/Cellar/opencv/4.3.0_5/include/opencv4 -L/usr/local/Cellar/opencv/4.3.0_5/lib -lopencv_gapi -lopencv_stitching -lopencv_alphamat -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dnn_superres -lopencv_dpm -lopencv_highgui -lopencv_face -lopencv_freetype -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_intensity_transform -lopencv_line_descriptor -lopencv_quality -lopencv_rapid -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_sfm -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_superres -lopencv_optflow -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_text -lopencv_dnn -lopencv_plot -lopencv_videostab -lopencv_videoio -lopencv_xfeatures2d -lopencv_shape -lopencv_ml -lopencv_ximgproc -lopencv_video -lopencv_xobjdetect -lopencv_objdetect -lopencv_calib3d -lopencv_imgcodecs -lopencv_features2d -lopencv_flann -lopencv_xphoto -lopencv_photo -lopencv_imgproc -lopencv_core
```

#### Xcode 프로젝트 생성

1. Create a new Xcode project
2. macOS - Command Line Tool - C++
3. Project - Build Settings: 설정 추가
   - Header Search Paths
     - `/usr/local/Cellar/opencv/4.3.0_5/include`
     - `recursive`
   - Library Search Paths
     - `/usr/local/Cellar/opencv/4.3.0_5/lib`
     - `recursive`
   - Other Linker Flags
     - `pkg-config --cflags --libs opencv4` 결과 값 전체 저장
4. Project - Signing & Capabilities - Target File: 설정 추가
   - Runtime Exceptions - Disable Library Validation: 활성화

#### 테스트

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

int main(int argc, const char * argv[]) {
    
    cout << "OpenCV version: " << CV_VERSION << endl;
    
    return 0;
}
```

#### 이미지 파일 불러오기

1. Project - Signing & Capabilities - Target File: 설정 추가
   - Reource Access - Camera: 활성화
1. Project - Target File - Build Phases - Copy Files
   - Destination: Resources
   - Subpath: Nothing
   - Copy only when installing 비활성화
   - 이미지 파일 추가

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

int main(int argc, const char * argv[]) {
    
    Mat img;
    img = imread("lenna.bmp");
    
    if (img.empty()) {
        cerr << "Image load failed!" << endl;
        return -1;
    }
    
    namedWindow("image");
    imshow("image", img);
    
    waitKey();
    
    return 0;
}
```

#### 카메라 권한 설정

참고: [Xcode에서 Mac의 내장 카메라 사용 방법](https://magnae2016.net/2)

1. 새 파일 - Resource - Property List: `Info.plist`
2. Privacy - Camera Usage Description 추가
   - Type: String
   - Value: 카메라 권한을 허용한다
3. Project - Target File - Identity - Choose Info.plist File
4. Project - Target File - Build Phases - Copy Files
   - Destination: Products Directory
   - Subpath: Nothing
   - Copy only when installing 비활성화
   - Info.plist 추가