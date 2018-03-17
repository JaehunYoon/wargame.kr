# wargame.kr

## login filtering (450p)

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/01%20Title.PNG)

`login filtering` 문제의 설명을 보면 필터링을 우회하여 로그인에 성공해야 한다고 제시되어 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/02%20index%20page.PNG)

메인 페이지에 접속하게 되면 다음과 같은 로그인 창을 볼 수 있다.

로그인 폼 하단에는 `get source` 라는 문구의 링크가 존재한다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/03%20view-source.PNG)

링크를 타게되면 해당 문제의 php 소스를 열람할 수 있게 된다.

```php
<?php

if (isset($_GET['view-source'])) {
    show_source(__FILE__);
    exit();
}

/*
create table user(
 idx int auto_increment primary key,
 id char(32),
 ps char(32)
);
*/

 if(isset($_POST['id']) && isset($_POST['ps'])){
  include("../lib.php"); # include for auth_code function.

  mysql_connect("localhost","login_filtering","login_filtering_pz");
  mysql_select_db ("login_filtering");
  mysql_query("set names utf8");

  $key = auth_code("login filtering");

  $id = mysql_real_escape_string(trim($_POST['id']));
  $ps = mysql_real_escape_string(trim($_POST['ps']));

  $row=mysql_fetch_array(mysql_query("select * from user where id='$id' and ps=md5('$ps')"));

  if(isset($row['id'])){
   if($id=='guest' || $id=='blueh4g'){
    echo "your account is blocked";
   }else{
    echo "login ok"."<br />";
    echo "Password : ".$key;
   }
  }else{
   echo "wrong..";
  }
 }
?>
```

```php
$row=mysql_fetch_array(mysql_query("select * from user where id='$id' and ps=md5('$ps')"));
```

위의 코드를 보면 POST 방식으로 `id`와 `ps` 값을 받아 query 문에 들어가게 된다.

`id` 값은 전송받은 문자열 그대로 삽입되지만, `ps` 값은 `md5`로 암호화 되어 삽입된다.

문제를 푸는 데에 필요한 값은 `$key` 값이다.

```php
if(isset($row['id']))
{
    if($id=='guest' || $id=='blueh4g')
    {
        echo "your account is blocked";
    }
    else
    {
        echo "login ok"."<br />";
        echo "Password : ".$key;
    }
}
else
{
  echo "wrong..";
}
```

해당 문제의 if절을 보면, `id`에 값이 db에 존재할 때, 그 값이 `guest`이거나 `blueh4g`와 같으면 `your account is blocked`라는 문구와 함께 `key`값을 얻지 못하게 된다.

하지만 `id` 값이 필터링되는 값이 아닌 경우에는 `login ok`를 출력한 후 `key` 값을 얻게 되기 때문에, `id` 필터링을 우회하여 로그인에 성공해야 한다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/04%20account%20infomation.PNG)

다음은 해당 문제의 php 소스의 맨 하단 부분에 주어진 계정 정보이다.

```html
<!--

you have blocked accounts.

guest / guest
blueh4g / blueh4g1234ps

-->
```

`id`와 `ps`값이 `guest`인 계정과 `id`가 `blueh4g`, `ps`값이 `blueh4g1234ps`인 계정에 대한 정보가 주석 처리 되어있다.

db 내에 존재하는 계정으로 추정하고, 이를 이용하여 필터링 우회를 해보자.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/05%20filtering%20worked.PNG)

`id`와 `ps`에 평범하게 `guest`를 삽입했을 경우에는 당연히도 다음과 같이 블록 당하게 된다.

이는 mysql에서 대소문자를 구분하지 않는 특징을 이용하여 문제를 풀 수 있다.

php 내 에서는 확실하게 대소문자를 구분하기 때문에 `if` 절에서 `guest`를 필터링할 때 `Guest`, `gUest`, `GUEST` 등과 같이 최소 1개의 문자만이라도 대문자로 바꾸어 대입해주면 이를 다른 문자열로 취급하여 필터링을 피할 수 있다.

하지만 query문에 삽입되는 `id` 값은 대문자가 섞여있어도 정상적으로 sql에서 검색이 가능하기 때문에, `if(isset($row['id'])` 에서 `id` 값을 찾은 것으로 간주하게 되어 정상적으로 `$key` 값을 얻게 된다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/06%20guest%20login%20ok.PNG)

다음과 같이 `id` 값을 우회하여 삽입하면 `flag` 값으로 추정되는 `password` 값을 얻을 수 있다.

`blueh4g` 도 위와 같이 문제를 해결할 수 있다. 얻어낸 `password` 값이 `flag` 인지 확실히 확인하기 위해서 다른 계정으로 로그인에 성공해도 같은 `password` 값을 출력하는지 확인해보자.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/07%20filtering%20worked%202.PNG)

기존에 제공했던 계정 값을 전송하면 블록이 된 것을 확인할 수 있다.

`if(isset($row['id'])` 를 충족했기 때문에 db 내에 존재하는 계정인 것을 알 수 있다.

![Image](https://github.com/JaehunYoon/wargame.kr/blob/master/Image/04%20login%20filtering/08%20blueh4g%20login%20ok.PNG)

다음과 같이 우회를 하여 전송했더니 `guest`로 로그인 했을 때와 같은 `password` 값을 출력하였다.

`flag` 값으로 간주한다.

**FLAG : c393f5f686dfd735fc56126db5932492365d6570**