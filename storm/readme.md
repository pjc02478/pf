STORM
====

* __작성 언어__ : C++
* __설명__ : C++를 위한 MYSQL ORM입니다. 내부적으로 connection-pool을 가지고 있습니다. 4대 쿼리와 트랜젝션을 지원합니다. join의 경우에는 직접 로우 쿼리로 조인부분을 작성해야 하는데, 이 문제를 해결하기 위해서 관계형 데이터 모델을 지원하는 두번째 버전을 제작 중에 있습니다. [arc](https://github.com/pjc0247/arc)
* __개발 인원__ : 1인

Example
----
* __SELECT 쿼리__
```C++
/* 싱글 레코드 검색 */
auto result = ORM::from("accounts")
  ->where("id", "pjc")
  ->where("nickname", "anz")
  ->find_one();
cout << result->get("level");

/* 다수 레코드 검색 */
auto results = ORM::from("accounts")
  ->where_raw("level = 1")
  ->limit(5)
  ->find_many();
for(auto account : results)
  cout<< account->get("nickname");
```
* __INSERT 쿼리__
```C++
auto row = ORM::from("test")->create();

row->set("hello", "world");
(*row)["foo"] = "bar";

row->save();
```
* __UPDATE 쿼리__
```C++
auto row = ORM::from("test")
  ->where("id", "1")
  ->find_one();
  
row->set("nickname", "anz41");

row->save();
```
* __DELETE 쿼리__
```C++
/* 싱글 레코드 지우기 */
auto row = ORM::from("test")
  ->where("id", "1")
  ->find_one();
row->remove();

/* 조건에 맞는 다수의 레코드 지우기 */
ORM::from("test")
  ->where("level", "1")
  ->remove();
```
* __TRANSACTION__
```C++
ORM::begin();
  // db 작업
ORM::commit();
ORM::rollback();
```
