# wargame.kr

## QR CODE PUZZLE (300p)

![Image]()

`QR CODE PUZZLE` 문제의 설명을 보면 `javascript`를 이용한 퍼즐 문제인 것을 알 수 있다.

![Image]()

메인 페이지의 모습이다. 문제의 제목과 같이 QR CODE가 분할되어 퍼즐처럼 순서가 뒤바뀌어 있다.

이 문제를 해결하기 위해서는 QR CODE의 모습을 되돌려 놓아야할 것이다.

### ~~1. 노가다~~

![Image]()

그냥 손수 퍼즐을 맞추어 QR CODE를 복구시키는 방법이 있다. ~~재미있다~~

정말 할 짓이 없지 않다면, 별로 추천하고 싶지 않다.

### 2. 소스 코드 참고

![Image]()

페이지 소스보기를 해보면 위 사진과 같이 `unescape('.%2f%69%6d%67%2f%71%72%2e%70%6e%67')`라는 코드가 작성되어 있다.

**javascript**에서 unescape는 escape를 통해서 인코딩된 문자열을 디코딩하는 함수이기 때문에 unescape 함수 내에 들어있는 값이 힌트가 될 것이다.

* [unescape](https://opentutorials.org/course/50/199)

![Image]()

`unescape('.%2f%69%6d%67%2f%71%72%2e%70%6e%67`를 클립보드에 복사한 후 위와 같이 개발자 도구의 Console 창에 붙여넣은 후 실행시키면 `./img/qr.png` 라는 값을 얻을 수 있다.

![Image]()

위에서 얻은 경로를 URL에 추가하여 페이지를 이동해보자.

![Image]()

알아낸 경로로 이동해보니 위와 같이 QR CODE 이미지 원본 파일이 첨부되어 있는 것을 볼 수 있다.

이를 파일로 저장하거나 이미지 경로를 복사한 후 QR CODE를 인식하면 된다.

PC에서 QR CODE를 인식하기 위해서, `http://www.onlinebarcodereader.com/` 라는 바코드 인식 사이트에 접속하였다.

![Image]()

다음과 같이 URL을 삽입하거나 이미지 파일을 삽입한 후 Start 버튼을 누르면

![Image]()

다음과 같은 URL을 얻을 수 있다. 이를 클릭해서 페이지 이동을 해보면 플래그를 볼 수 있다.

![Image]()

**FLAG : 082d1ef7e140b5a4dd8a22465c9f6e5027ac77b0**