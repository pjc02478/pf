coco
====

* __사용 언어__ : PHP
* __설명__ : 게임 서버 제작을 위한 프레임워크입니다.
* __기능__
  * request/response 파라미터 사용에 대한 유연성을 제공
    * request에서 데이터를 가져올 때 get인지 post인지, 혹은 post로 json이 넘어오는지에 대해서 로직 코드의 변경 없이 유연하게 처리할 수 있는 방법을 제공합니다. 또한 response를 보낼 때에도 간단하게 json, xml, 혹은 커스텀 포멧을 사용하여 내보낼 수 있도록 도와줍니다.
  * 파라미터와 php value의 1:1맵핑
    * 파라미터와 php내의 변수와 1:1맵핑이 제공됩니다. request에서 넘겨받은 파라미터를 바로 php 변수로 사용할 수 있고, 반대로 php의 변수가 바로 response 데이터에 반영됩니다.
  * 편리한 값 검증
    * 넘겨받은 파라미터가 존재하는지, 길이가 어떻게 되는지, 범위가 맞는지 혹은 정규식에 매칭되는지 검사할 수 있는 기능을 제공합니다.
  * orm 지원 
    * 매번 db 연결을 맺고, 끊고, 직접 쿼리 코드를 작성해야 하는 불편함을 없에기 위해 ORM이 포함되어 있습니다. (이 부분은 idiorm으로 지원되며, 직접 작성한 부분이 아닙니다.)
* __개발 인원__ : 1인

Example
----
* __로그인하여 유저 정보를 돌려주는 코드__
```php
<?php
  coco::in([
    "id" => "required pk[accounts] as[account] va-length[8,16]",
    "password" => "required va-length[8, 32]");
  coco::out([
    "nickname" => "required from[account->nickname]",
    "level" => "required from[account->level]"]);
  
  if( $account->password != $password )
    coco::abort( INVALID_PASSWORD );
?>
```
* __닉네임을 변경하는 코드__
```php
<?php
  coco::in([
    "session" => "session_key",
    "nickname" => "required va-length[8,16] va-rex[/[a-zA-Z0-9]+/] as[new_nickname]",
    "account" => "required session"]);
  coco::out([
  "timestamp" => "timestamp"]);
  
  /* 변경된 값은 response이후에 자동으로 커밋된다. */
  $account->nickname = $new_nickname;
?>
```

IN attributes
----
* __required__ : 항목이 필수임을 명시.
* __optional__ : 항목이 필수가 아님을 명시. (기본값)
* __as[name]__ : 이 항목의 값을 name 이름의 PHP변수로 가져온다.
* __pk[table,(column_name)]__ : 이 항목의 값이 table에서의 column_name에 대한 PK임을 명시하고, 해당 row를 가져온다. 
* __disable-autocommit__ : pk로 가져온 row가 autocommit 되는것을 방지.
* __va-rex[regex]__ : 항목의 값을 regex(정규식)으로 검증한다.
* __va-length[min,max]__ : 항목의 길이가 min~max 범위의 값인지 검증한다.
* __va-length[len]__ : 항목의 길이가 len인지 검증한다.
* __va-range[min,max]__ : 항목이 numeric값이고, min~max 범위의 값인지 검증한다.
* __session__ : 이 항목의 값은 세션으로부터 가져온다.
* __session_key__ : 이 항목이 session의 key가 됨을 명시.

OUT attributes
----
* __required__ : 항목이 필수임을 명시.
* __optional__ : 항목이 필수가 아님을 명시. (기본값)
* __timestamp__ : 타임스탬프 값을 대입.
* __session_key__ : session의 key값을 대입
* __from[name]__ : 이 항목의 값을 name이름을 가진 PHP 변수로부터 가져온다.
