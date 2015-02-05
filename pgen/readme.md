pgen 패킷 빌더
====

* __작성 언어__ : Ruby
* __설명__ : 패킷을 정의하고 빌드하여 C/C++에서 작성된 서버/클라이언트에서 사용할 수 있도록 해주는 툴입니다.
* __개발 인원__ : 1인

Example
----
* 스크립트에 스키마를 작성
```ruby
# 패킷의 정의
class LoginRequest < Packet
  required #required된 패킷만 빌드에 참여
  string :user_id, 32
  string :user_pw, 32
end
# 열거형의 정의
class CharacterType < Enum
  required
  keys [
    :CH_NONE,
    :CH_PLAYER,
    :CH_MONSTER ]
end
```
* pgen.rb 파일 실행
  * -s : 소스 파일 경로
  * -d : 출력 경로 (리스트)
  * -p : stdout으로 결과 프리뷰 출력
```
ruby pgen.rb -s packet_def.rb -d 출력 경로1,출력 경로2,...
```


TestModule
----
* 작성된 스키마로 서버를 테스트 해 볼 수 있도록 테스트 모듈이 제공됩니다.
```ruby
class LoginTest < RocketTestUnit
  def query
    p = LoginRequest.new
    p.user_id = "pjc0247"
    p.user_pw = "some_password"
    return p
  end
  def should
    p = LoginResponse.new
    p.result = 1
    return p
  end
end
```
```ruby
RocketTestPool.execute \
  [LoginTest, EnterRoomTest, LeaveRoomTest]
```

* 테스트 실행
```
Passed LoginTest - 0.0001sec
Failed SomeTest - foo is 'hello' (expected 'world')
```

관련 프로젝트
----
* [CornFlakes](https://github.com/pjc0247/CornFlakes)
