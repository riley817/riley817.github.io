# 텔레그램 봇 설정하기


> 할때마다 까먹는다.

## 텔레그램 봇 만들기

#### 1. 봇아빠 채팅방에 접속
- [https://t.me/BotFather](https://t.me/BotFather) 혹은 `@BotFather` 검색하여 채팅방 접속

#### 2. BotFather 채팅방에서 봇 생성 명령어를 입력한다.
- 채팅창에 `/newbot`을 입력한다.
  ![telegram-1](/posts/images/etc/20210813105155.png)
  
#### 3. 텔레그램 봇 이름을 입력한다.
- 이름은 `_bot`으로 끝나야 한다.
    ![telegram-2](/posts/images/etc/20210813105538.png)

#### 4. 다시 입력
- 한번 더 bot 이름을 입력하면 봇 주소와 API 토큰 값을 얻을 수 있다.
    ![telegram-3](/posts/images/etc/20210813105916.png)
  
#### 5. 봇 채팅방 접속 후 시작 버튼 클릭
{{< figure src="/posts/images/etc/20210813110808.png#center" width="50%">}}

![telegram-4](/posts/images/etc/20210813110808.png)

#### 6. 봇 채팅방 chat id 얻기
- 봇 채팅방에서 아무 메세지나 입력한다.
  ![telegram-5](/posts/images/etc/20210813012213.png)

- 브라우저에서 https://api.telegram.org/bot`봇토큰`/getUpdates 으로 접속하면 JSON 데이터에서 chat 아이디를 확인할 수 있다.
- chat 객체에 id 값 9자리가 봇챗방 아이디 이다.
```json
{
"ok": true,
"result": [
    {
      "update_id": 556815269,
      "message": {
        "message_id": 3,
        "from": {
            "id": 1719000000, ...
        },
        "chat": {
            "id": 1719000000, ...
        },
        "date": 1628829234,
        "text": "/start",
        "entities": [
            {
                "offset": 0,
                "length": 6,
                "type": "bot_command"
            }
          ]
        }
      }
  ]
}
```

## 2. Private 채팅방 텔레그램 채널 ID 알아내기
- 새로운 private 채널을 생성한다.
  
    ![telegram-5](/posts/images/etc/20210813023637.png)

- 채널의 초대링크를 복사한다.
  ![telegram-5](/posts/images/etc/20210813025156.png)

- [@username_to_id_bot](https://t.me/username_to_id_bot) 채팅방에 접속하여 내 초대 링크를 채팅창에 입력한다.
- IDBot계정이 나의 private 채팅방의 챗방 ID를 리턴한다.
  ![telegram-5](/posts/images/etc/20210813025400.png)

- 메세지 보내기 API 테스트!
    ```shell
    https://api.telegram.org/bot<봇토큰>/sendmessage?chat_id=-1001183952507&text=에쑤빠는나야두리될수없숴
    ```
- 성공쓰
    ![telegram-5](/posts/images/etc/20210813030239.png)
  
## 참고링크
* [How to obtain the chat_id of a private Telegram channel?](https://stackoverflow.com/questions/33858927/how-to-obtain-the-chat-id-of-a-private-telegram-channel)
