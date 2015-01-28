EventPie
====

* __작성 언어__ : C++
* __설명__ : C++로 작성된 node-js스타일의 이벤트 드리븐 구현체입니다.

Example
----
* echo server
```C++
server = new TCPServer(9916,
  [=](int err, TCPSocket *client){
    if(err == eNoError){
      client->onReceive(
        [=](void *data, int len){
          /* echo-back */
          client->write( data, len );
        });
      client->onUnbind(
        [=](){
          printf("disconnected\n");
        });
    }
  }
```

Features
----
* __Timer__
타이머는 지정된 주기마다 반복적으로 작업할 수 있게 해줍니다.
```C++
int life = 3;
new Timer(
  [&life, =](){
    printf("0.15초마다 실행되는 타이머\n");
    
    life --;
    if( life == 0) delete timer;
  }, 150);
```
* __Task__
Task는 EventPie에서 처리하는 작업의 단위입니다.<br>
defer함수를 이용해 태스크를 뒤로 미룰 수 있습니다. (node의 nextTick과 같은 역할)
```C++
defer(
  [](){ printf("hello\n"); });
```
deferAsync함수를 통해서 태스크를 비동기적으로 실행할 수 있습니다.<br>
이 함수를 통해서 blocking로직을 non-blocking로직으로 변환할 수 있습니다.<br>
작업은 EventPie의 thread-pool에서 실행됩니다.
```C++
deferAsync(
  [](){
    sleep(1000);
    
    defer(
      [](){
        printf("task completed\n");
      })
  });
```
