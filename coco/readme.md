coco
====

* __사용 언어__ : PHP
* __설명__ : 게임 서버 제작을 위한 프레임워크입니다.

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
