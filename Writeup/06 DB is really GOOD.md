# wargame.kr

## DB is really good (500p)

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/01%20Title.PNG)

문제 설명을 잘 보면, **어떤 종류의 데이터베이스일까요? 사용자 이름과 데이터베이스 간의 상관 관계를 찾아야 합니다.** 라는 문구로 설명이 되어 있다.

문제명도 그렇듯이 아마 이번 문제는 DB를 직접 조회하거나 건드려보는 작업을 필요로 하는 문제라고 예상되었다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/02%20index%20page.PNG)

메인 페이지의 모습이다. `BLUEMEMO SYSTEM` 라는 문구와 함께 `USER`를 입력받은 후 로그인을 하는 방식으로 구성되어 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/03%20add%20any%20string.PNG)

다음은 `USER`에 아무 값이나 대입해본 사진이다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/04%20memo_php.PNG)

로그인 버튼을 누르니 다음과 같이 메모를 작성할 수 있는 기능이 구현되어 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/05%20write%20note.PNG)

다음은 메모장에 아무 내용이나 작성한 후 등록을 한 모습이다. 평범하게 글이 등록되는 것을 볼 수 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/06%20add%20admin.PNG)

너무 평범한 메모 시스템이여서 로그인 창에서 `USER`를 `admin`으로 지정한 후 접속을 해보기로 하였다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/07%20access%20denied.PNG)

결과는 다음과 같이, `dont access with 'admin'` 이라는 문구와 함께 접속이 제한되었다. 

이 후에는 도저히 어떻게 해야 DB를 조회할 수 있을지 구상하다가 의미없는 `SQL Injection` 공격과 특정 문자열들을 삽입해보았다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/08%20add%20slashes.PNG)

수십 분 가량 경과된 후로, 다음과 같이 `/`를 삽입하게 되었다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/09%20error%20print.PNG)

놀랍게도 해당 페이지에서는 `/`로 인해 오류를 뿜게 되었고 에러 정보를 출력하여 실마리를 잡게 되었다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/10%20add%20h4lo.PNG)

오류문을 통해서 얻을 수 있는 정보가 무엇인지 알아보기 위해 `/` 뒤에 `h4lo`라는 문자열을 삽입하게 되었다. `/h4lo`가 붙은 경로를 찾다보면 `admin`의 db 정보를 열람할 수 있는 방법을 얻을 수 있을 것이다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/11%20here.PNG)

다음은 `/h4lo`를 통해 얻은 경로를 나타낸 사진이다.

`./db/wkrm_` 뒤에 전송한 `USER`명이 덧붙여 나오는 것을 알 수 있었다. 이를 이용하여 우리는 `admin`의 db 정보가 담긴 db 파일을 다운로드할 수 있을 것이다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/12%20download%20databases.PNG)

다음과 같이 URL에 `/db/wkrm_admin.db` 라는 경로를 추가하여 접속을 하면 `wkrm_admin.db` 라는 데이터베이스 파일을 다운로드 받을 수 있다.

필자는 `DB Browsers for SQLite`라는 툴을 이용하여 db 파일을 열람하였다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/14%20open%20file.PNG)

다음과 같이 다운로드 받은 db 파일을 열면 `wkrm_admin`의 db 정보를 확인할 수 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/15%20flag%20hint.PNG)

'데이터 보기' 라는 메뉴로 들어가 `memo` 테이블의 정보를 확인하면 다음과 같이 문제 풀이에 성공하여 URL을 제공받을 수 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/16%20url.PNG)

다음과 같이 db 열람을 통해 알아낸 주소를 추가하여 접속을 해보자.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/06%20DB%20is%20really%20GOOD/17%20flag.PNG)

플래그 값을 획득하여 문제 풀이에 성공했다.

**FLAG : 1ff61a3b5d6648fd6849b8a2977ad99f9dcc895d**