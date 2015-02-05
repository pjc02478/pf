choco
====

* __사용 언어__ : C++
* __설명__ : C++로 작성된 게임 서버 프레임워크
  * IOCP기반으로 제작되었습니다.
  * 마이크로쓰레딩을 지원합니다.
* __개발 인원__ : 1인
* __모듈__
  * 서버 베이스
  * 마이크로스레드
  * 패킷 핸들러
  * 세션/ 세션 풀
  * ORM
  * 메모리 풀
  * http
  * 로거
  * 컨피그 (외부 config파일을 통한 서버 환경설정)
  * 패러렐 (스레드 풀)
  * 유틸리티 (hash, uniqid ...)

Example
----
* __간단한 로그인 서버__
```C++
using namespace choco;

class login_test : public intf::handler{
  login_test(){
    _ROUTE_ASYNC(login_test, login_request);
  }
  
  virtual void on_connected(
    session::conn *client){}
  virtual void on_disconnected(
    session::conn *client){}
  
  _async void on_login_request(
    session::conn *client,
    login_request *pkt){
    
    auto account = orm::from("accounts")
      ->where("id", pkt->id)
      ->where("pw", pkt->pw)
      ->find_one_async();
    
    if(account == nullptr)
      send_login_request(
        client, false, "");
    else{
      send_login_request(
        client,
        true, account->get("nickname").c_str());
    }
  }
  
  void send_login_response(
    intf::sendable *to,
    INT(result), CSTRING(nickname)){
    
    login_response pkt;
    pkt.result = result;
    strcpy_s(pkt.nickname, nickname);
    
    to->send(&pkt);
  }
};
```
* __sleep__
```C++
printf("hello ");

/* 4초간 대기합니다. 
 *   대기하는 동안 스레드가 중지하는 것이 아니라 다른 요청을 처리합니다.
 *   4초가 지나면 현재 요청으로 복귀하여 아래의 코드를 수행합니다. */
mt::sleep(4000);

printf("world\n");
```
* __http 클라이언트__
```C++
  /* http요청을 수행하는동안 스레드가 블럭되지 않습니다. */
  _async void foo(){
    string doc;
    errorno err = 
      http::open_uri("www.google.com", doc);
      
    cout<< doc;
  }
```
* __스케쥴러 사용하기__
```C++
/* 3초마다 실행하기 */
parallel::schedule(func, 3000);

/* 1초후 한 번만 실행하기 */
parallel::schedule_once(func, 1000);
```
