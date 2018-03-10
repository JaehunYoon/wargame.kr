# Wargame.kr

## already got (200p)

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/01%20already%20got/01%20Title.PNG)

**already got** 문제를 클릭해보면 다음과 같이 문제 설명이 되어있다.

`can you see HTTP Response header?` 라는 문구를 보아하니 아마 이 문제는 웹 페이지의 HTTP Response Header를 보면 될 것 같다.

Start를 누르면 본격적인 문제 풀이를 위한 페이지로 이동하게 된다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/01%20already%20got/02%20index$20page.PNG)

접속된 페이지에는 `you've already got key! :p` 라는 문구를 출력하고 있다. 문제 설명에서 보았던 HTTP 헤더를 찾아보자.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/01%20already%20got/03%20network.PNG)

브라우저의 개발자 도구를 통해 `already_got/` 이라는 HTTP 헤더를 찾을 수 있었다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/01%20already%20got/04%20flag.PNG)

문제 설명에서 말한 대로 HTTP Response header를 살펴보니 FLAG가 제시되어 있었다.

**FLAG : c74b3ece3990cf1b80c8b806a3a83a168e0ad12e**