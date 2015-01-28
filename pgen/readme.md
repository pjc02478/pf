pgen 패킷 빌더
====

* 작성 언어 : Ruby

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
