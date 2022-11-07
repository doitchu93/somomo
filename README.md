# :peach:**SOMOMO (소소한 모임 모여라)**

> 가벼운 소모임부터 주제를 정한 그룹 모임까지 다 모아 놓은 SNS 웹사이트<br>
데모 링크

<br>

## 목차

[1. 제작 기간](#1-제작-기간)<br>
[2. 참여 인원](#2-참여-인원)<br>
[3. 사용 기술](#3-사용-기술)<br>
[4. ERD](#4-erd)<br>
[5. 담당 기능](#5-담당-기능)<br>
[6. 문제 해결](#6-문제-해결)

<br>

## 1. 제작 기간

- 2022.07.25 ~ 2022.08.26
- 기획, 설계 2주 / 개발 : 3주

<br>

## 2. 참여 인원

<table>
    <tr align=center>
        <td width=200px>팀원 1</td>
        <td width=200px>팀원 2</td>
        <td width=200px><b>유형두</b></td>
        <td width=200px>팀원 3</td>
        <td width=200px>팀원 4</td>
        <td width=200px>팀원 5</td>
    </tr>
    <tr align=center>
        <td>
            로그인<br>
            회원가입<br>
            마이 페이지
        </td>
        <td>
            메인 페이지<br>
            카카오 로그인
        </td>
        <td>
            <b>소모임 채팅방</b>
        </td>
        <td colspan=3>
            그룹모임방 목록 페이지<br>
            그룹모임방 상세 페이지
        </td>
    </tr>
</table>

<br>

## 3. 사용 기술

### Back-End

- Java 8
- Spring 5.3.14
- Spring Security 5.5.2
- Maven
- MyBatis 3.5.7
- ORACLE 11G

### Front-End

- JavaScript
- HTML5 / CSS3

<br>

## 4. ERD

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199242755-da9bbf3d-e2b6-470a-91b3-244c14689eb8.png" width=80%/></p>

<br>

## 5. 담당 기능

### 소모임 채팅방

- 메인 페이지에서 모임 모집 글 작성 시 연동되는 소모임 채팅방이 생성됩니다.
- 모임 모집 글을 통해 다른 사용자들이 소모임 채팅방에 입장할 수 있습니다.
- 사용자들은 소모임 채팅방에서 실시간으로 소통할 수 있습니다.
- 채팅 페이지에서 참여한 소모임 채팅방을 관리할 수 있습니다.

<br>

<details><summary><b>5.1. 채팅방 구조 펼치기</b></summary><div markdown="1">

### 5.1. 채팅방 구조

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199678058-727c6f4a-015d-43a9-aeb5-04f98072777f.png" width=80%/></p>

#### 5.1.1. 채팅방 목록

- 채팅방 목록은 사용자가 참여한 채팅방의 마지막 메시지 시간을 기준으로 내림차순으로 정렬해서 표시합니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/0f79e617bf6ee8838ceb4415ea6be279669d43cc/somomo/src/main/resources/mappers/chat-mapper.xml#L138-L175)

- 각각의 채팅방에는 마지막 메시지 내용과 시간이 표시되고, 읽지 않은 메시지 수를 알림 배지로 보여줍니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/9f75de21a344c7e56cf2bea4f01bd5ffd91be90b/somomo/src/main/webapp/WEB-INF/views/chat/chatMain.jsp#L538-L581)

- 실시간으로 변경되는 것을 확인할 수 있도록 setInterval을 이용해 지정한 시간마다 변경된 내용을 불러와 줍니다.

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199704471-488dd5bc-279e-43a1-84ad-d15dec55f785.gif"/></p>

- 사용자가 채팅방에 참여한 시간보다 이전에 다른 사용자가 보낸 마지막 메시지가 있다면, 해당 사용자의 채팅방 목록에는 마지막 메시지와 시간이 표시되지 않습니다.<br>
[:pushpin:코드 보기 1](https://github.com/doitchu93/somomo/blob/06ca4e094b02917e20e0eac874ddbbed2c6ef017/somomo/src/main/java/com/kh/somomo/chat/controller/ChatController.java#L148-L162)<br>
[:pushpin:코드 보기 2](https://github.com/doitchu93/somomo/blob/06ca4e094b02917e20e0eac874ddbbed2c6ef017/somomo/src/main/webapp/WEB-INF/views/chat/chatMain.jsp#L552-L557)

#### 5.1.2. 채팅 영역

- 채팅방 선택 시 채팅 영역에는 유저 목록, 메시지 영역, 메시지 입력 영역이 표시됩니다.

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199717487-f12b9678-4cea-4f3d-9c73-22a44e152d6f.png" width=80%/></p>

- 유저 목록의 화살표를 클릭하면 채팅방에 참여한 사용자를 확인할 수 있습니다.

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199720220-298e57c0-65b1-45ed-838d-c563fc32165a.gif"/></p>

- 선택한 채팅방의 메시지를 AJAX를 이용해 DB에서 조회 후, 반복문을 이용해 HTML로 표시해줍니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/b3ef888b9b5af57e6ad565e9a51152274454b0f2/somomo/src/main/webapp/WEB-INF/views/chat/chatMain.jsp#L677-L729)

- 가져온 메시지의 날짜를 비교해서 만약 날짜 표시가 없거나 다르다면 가져온 메시지의 날짜를 표시합니다.

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199725162-f80f918d-2b1e-4bd2-a10c-85befdc631b0.gif"/></p>

- 이후 웹 소켓을 이용해서 수신받은 메시지도 위와 같은 과정을 거쳐서, DB에서 가져온 HTML 내용에 append 해줍니다.

<p align="center"><img src="https://user-images.githubusercontent.com/102894238/199729182-db559a76-796d-417f-a69b-12ea80078df8.png" width=60%/></p>

</div></details>

<br>

<details><summary><b>5.2. 실시간 채팅 펼치기</b></summary><div markdown="1">

### 5.2. 실시간 채팅

- 채팅 내용 전달 시 메시지 내용뿐만 아니라 작성자 정보와 채팅방 번호를 같이 보내기 위해 JSON을 사용했습니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/edb6408e87765d2be91a284be2d7abc6f6275115/somomo/src/main/webapp/WEB-INF/views/chat/chatMain.jsp#L227-L246)

- 웹 소켓 서버에서 수신한 JSON Data를 Java 객체로 변환하기 위해 Jackson 라이브러리의 ObjectMapper를 사용했습니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/edb6408e87765d2be91a284be2d7abc6f6275115/somomo/src/main/java/com/kh/somomo/chat/controller/ChatWebSocketServer.java#L62-L66)

- 채팅방을 선택하면 특정 메시지를 발송하여 웹 소켓 서버 내에서 채팅방을 구분할 수 있도록 Map을 이용해 담아줍니다.
- 만약 사용자가 웹 소켓 서버의 채팅방을 들어가려 할 때 웹 소켓 서버 내에 아직 채팅방이 생성되지 않았다면 채팅방을 생성하고 입장하도록 합니다.
- 다른 사용자가 같은 채팅방에 입장할 때는 채팅방 생성 없이 바로 입장할 수 있습니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/edb6408e87765d2be91a284be2d7abc6f6275115/somomo/src/main/java/com/kh/somomo/chat/controller/ChatWebSocketServer.java#L73-L105)

- 웹 소켓 내의 채팅방에 들어간 뒤 채팅 메시지를 수 / 발신할 수 있습니다.
- 전달받은 메시지는 채팅 DB에 저장되고 저장된 DB를 다시 불러와 채팅방에 채팅방 DB에 저장된 시간을 기준으로 표시될 수 있도록 처리했습니다.<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/edb6408e87765d2be91a284be2d7abc6f6275115/somomo/src/main/java/com/kh/somomo/chat/controller/ChatWebSocketServer.java#L107-L138)

</div></details>

<br>

## 6. 문제 해결

<details><summary><b>6.1. 채팅 발송 불가 문제 펼치기</b></summary><div markdown="1">

### 6.1. 채팅 발송 불가 문제

- 여러 채팅방을 선택해서 채팅 내용을 확인 후 다른 페이지를 갔다 돌아오면 일부 채팅방에서 채팅이 발송되지 않는 문제 발생

#### 원인

- 사용자가 웹 소켓 서버를 통해 여러 채팅방에 연결된 상태에서 웹 소켓 서버 종료 시 사용자 session이 전부 제거되지 않아서 이후 재접속하면 session이 제거되지 않은 채팅방에서는 웹 소켓 통신을 할 수 없음

    <details><summary>기존 코드 펼치기</summary><div markdown="1">

    ```java
        @Override
        public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
            connectedUsers--;
            System.out.println("[" + session.getId() + "] [웹소켓 서버 연결 종료] | 총 접속 인원 : " + connectedUsers + "명");

            // sessionList에 session이 있다면
            if(sessionList.get(session) != null) {

                // 해당 session의 채팅방 번호를 가져와서, 채팅방을 찾고, 그 채팅방의 ArrayList<session>에서 해당 session을 제거
                chatRoomList.get(sessionList.get(session)).remove(session);

                // sessionList에서 session 제거
                sessionList.remove(session);
            }
        }
    ```

    </div></details>

#### 해결

- foreach문으로 chatRoomList 안의 session을 전부 찾아서 제거할 수 있게 수정<br>
[:pushpin:코드 보기](https://github.com/doitchu93/somomo/blob/cc9e938d9082b4a6881485a22f00db12c939b4e3/somomo/src/main/java/com/kh/somomo/chat/controller/ChatWebSocketServer.java#L41-L57)

</div></details>

<br>