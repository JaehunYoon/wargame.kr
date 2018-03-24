# wargame.kr

## md5_compare (500p)

![Image]()

문제 설명을 보면 **다른 값을 이용하여 그냥 일치시켜라** 라고 제시되어 있다.

서로 다른 값으로 일치시켜야 하는 `md5_compare` 문제라고 생각하니, 예상으로는 `Magic Hash`를 이용한 문제라고 예상되었다.

![Image]()

문제의 메인 페이지에 접속을 하면 2개의 `value`를 입력받는 폼이 존재하였고, `view-source`를 통해 문제의 php 소스를 열람할 수 있도록 한 링크가 있었다.

![Image]()

문제의 php 코드이다.

```php
<?php
    if (isset($_GET['view-source'])) {
         show_source(__FILE__);
         exit();
    }

    if (isset($_GET['v1']) && isset($_GET['v2'])) {
        sleep(3); // anti brute force

        $chk = true;
        $v1 = $_GET['v1'];
        $v2 = $_GET['v2'];

        if (!ctype_alpha($v1)) {$chk = false;}
        if (!is_numeric($v2) ) {$chk = false;}
        if (md5($v1) != md5($v2)) {$chk = false;}

        if ($chk){
            include("../lib.php");
            echo "Congratulations! FLAG is : ".auth_code("md5_compare");
        } else {
            echo "Wrong...";
        }
    }
?>
```

```php
if (isset($_GET['v1']) && isset($_GET['v2']))
```

`GET` 방식으로 입력받는 `v1`, `v2`의 값이 존재하면 조건을 만족하게 된다.

```php
sleep(3); // anti brute force
```

`sleep` 함수를 넣어서 브루트포스 공격에 많은 시간이 소요되도록 설계하였다.

```php
$chk = true;
$v1 = $_GET['v1'];
$v2 = $_GET['v2'];
```

`$chk` 라는 `bool` 변수가 존재하고, `v1`, `v2`의 값을 `GET` 방식으로 입력받는다.

```php
if (!ctype_alpha($v1)) {$chk = false;}
```

`ctype_alpha()`라는 함수가 제시되어 있다.

이 함수는 어떤 기능을 하는 함수일까?

#### bool ctype_alpha (string $text)

`ctype_alpha` 함수는 인자값으로 들어간 `$text` 문자열이 모두 알파벳으로 구성되어 있는지 확인하고 만약 그렇다면 **True** 값을 반환(1 반환)하고 그렇지 않다면 **False**를 반환(Null 반환)하게 된다.

위의 함수 설명을 참고하여 코드를 해석해보면, **만약 `$v1`의 문자열이 알파벳만으로 구성되어 있지 않다면 `$chk`를 `false`로 반환**하게 된다.

```php
if (!is_numeric($v2) ) {$chk = false;}
```

이번에는 `is_numeric()`이라는 함수가 제시되어 있다.

이 함수는 어떤 기능을 하는 함수일까?

#### bool is_numeric (string $text)

`is_numeric` 함수는 인자값으로 들어간 `$text` 문자열이 모두 숫자로 구성되어 있는지 확인하고 만약 그렇다면 **True** 값을 반환(1 반환)하고 그렇지 않다면 **False**를 반환(Null 반환)하게 된다.

`ctype_alpha` 함수와 비슷한 개념이지만 숫자 또는 알파벳으로만 구성된 문자열인지 확인하는 부분에서는 확연히 차이나기 때문에 이를 주의 깊게 살펴보아야 한다. 즉, 위의 설명을 참고해보면, **만약 `$v2`의 문자열이 숫자로만으로 구성되어 있지 않다면 `$chk`를 `false`로 반환**하게 된다.

결론적으로, `v1`에는 알파벳만으로 구성된 문자열, `v2`에는 숫자로만 구성된 문자열이 들어가야 한다.

```php
if (md5($v1) != md5($v2)) {$chk = false;}
```

문제의 핵심 조건절이다.<br/>
`md5`로 암호화시킨 `v1`, `v2`의 값이 일치하지 않으면 `$chk`가 `false`가 되어 버린다.

`md5` 비교 연산을 일치시키면서 `ctype_alpha`와 `is_numeric` 함수의 조건문을 무사히 통과하려면 '알파벳으로만 구성된 매직해시 값' 과 '숫자로만 구성된 매직해시 값'을 이용하여 `Magic Hash` 우회를 통해 문제를 풀어나가야 한다.

여기서 `Magic Hash`란, `md5`로 인코딩을 하였을 때 `0e`로 시작하는 `md5` 암호문은 지수 연산으로 인식되어 결국 0이라는 값으로 연산되어 나오게 되는 점을 이용하여 `md5` 비교 연산을 우회하는 방법이다.

***

`v1`에는 알파벳으로만 구성된 매직 해쉬 값을 넣어야 하기 때문에, `QNKCDZO` 라는 값을 이용하기로 하였다.

`QNKCDZO`는 `md5`로 암호화 하였을 때, `0e830400451993494058024219903391` 라는 값이 나오기 때문에, 알파벳만으로 구성된 문자열이면서 `Magic Hash` 우회가 가능하다.

`v2`에는 숫자로만 구성된 매직 해쉬 값을 넣어야 하기 때문에, `240610708` 라는 값을 이용하기로 하였다.

`240610708`는 `md5`로 암호화 하였을 때, `0e462097431906509019562988736854` 라는 값이 나오기 때문에, 숫자만으로 구성된 문자열이면서 `Magic Hash` 우회가 가능하다.

![Image]()

`v1`과 `v2`에는 위에서 언급한 값을 대입하였다. 

```php
if ($chk){
            include("../lib.php");
            echo "Congratulations! FLAG is : ".auth_code("md5_compare");
        }
```

문제의 서버로 `submit`을 보내면, 우회에 성공하여 다음 조건절에 만족하기 때문에 `FLAG` 값을 얻을 수 있을 것이다.

![Image]()

`Magic Hash`를 이용한 `md5` 비교 우회에 성공하여 `FLAG`를 획득하였다.

**FLAG : 43ee903041bef8224c635e385aa0acb8bc31804b**