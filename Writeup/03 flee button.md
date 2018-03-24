# wargame.kr

## flee button (450p)

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/01%20Title.PNG)

`flee button` 문제의 설명을 보면, 

```
click the button!
i can't catch it!
```
라는 문구가 남겨져 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/02%20index%20page.PNG)

메인 화면으로 들어가면 나오는 사진이다. 사진을 보면 마우스 커서는 보이지 않게 나오기 때문에 이해가 가지 않겠지만, `click me!` 버튼과 마우스 커서에는 거리를 두고 있으며 마우스 커서를 움직이면 버튼도 함께 움직이게 되어 클릭을 할 수 없게 구성되어 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/03%20page%20source.PNG)

페이지 소스 보기를 해보면, 많은 양의 인코딩된 문자열들이 `unescape_blue14` 함수 내에 존재한다. `unescape`는 이전 문제에서도 다루었던 함수이기 때문에 이를 디코딩시키면 될 것이다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/04%20we%20should%20decode%20this%20code.PNG)

다음과 같이 필요한 부분을 복사한 후 개발자 도구를 이용하여 함수를 실행할 준비를 한다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/05%20console%20unescape.PNG)

붙여넣기.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/06%20unescape%20(1).PNG)

**Console**에서 함수를 직접 실행시켜보면 다음과 같은 코드가 디코딩되어 나오는 것을 볼 수 있다. 디코딩한 코드에는 계속해서 `unescape`으로 감싸진 인코딩 구문이 나오기 때문에 다시 한 번 이를 디코딩해야한다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/07%20we%20shoud%20decode%20again%20this%20code.PNG)

다음과 같이 필요한 부분을 복사한 후 함수를 실행시킨다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/08%20find%20hidden%20html%20code.PNG)

실행 결과, 완전히 복호화된 html 코드를 찾게 되었다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/09%20key.PNG)

찾아낸 코드 내에 `onclick` 이라는 이벤트를 유심히 살펴보자. `onclick`은 클릭 시 발생하는 이벤트인데 우리가 클릭해야 할 버튼을 클릭하면 발생할 이벤트가 명시되어있다.

```javascript
onclick="window.location='?key=d39f';"
```

버튼을 클릭하게 되면 `?key=d39f` 라는 URL을 추가한 페이지로 이동하게 된다는 것을 알 수 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/10%20add%20url.PNG)

임의로 `window.location`에 들어간 값을 URL에 대입해보자.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/03%20flee%20button/11%20flag.PNG)

다음과 같이 플래그를 볼 수 있는 페이지로 이동하게 되었다.

