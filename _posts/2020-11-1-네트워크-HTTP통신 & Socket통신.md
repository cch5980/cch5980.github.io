---
title: "HTTP 통신 vs Socket 통신"
category: Web, Network
---



# HTTP 통신

- <u>Client의 요청(Request)이 있을때만 서버가 응답(Response)</u>하여 해당 정보를 전송하고 곧바로 연결을 종료하는 방식
- Client가 요청을 보내는 경우에만 Server가 응답하는 **<span style="color:red">단방향적 통신</span>**(Server가 Client로 요청을 보낼 수 없다.)

![http통신](https://user-images.githubusercontent.com/23491962/97810319-5627b300-1cb6-11eb-9d59-45517f37cb3d.JPG)


특징

- Client가 요청을 보내는 경우에만 Server가 응답하는 단방향 통신이다.
- Server로부터 응답을 받은 후에는 연결이 바로 종료된다.
- 실시간 연결이 아니고, 필요한 경우에만 Server로 요청을 보내는 상황에 유용하다.
- 요청을 보내 Server의 응답을 기다리는 어플리케이션(Adroid, ios)의 개발에 주로 사용된다.










# Socket 통신

- <u>Server와 Client가 특정 port를 통해 연결을 성립하고 있어 실시간</u>으로 <span style="color:red">**양방향 통신**</span>(실시간 통신이 필요한 경우에 자주 사용)
  - ex) 실시간 동영상 스트리밍 서비스를 HTTP 통신으로 구현했다고 가정하면 사용자가 서버로 동영상을 요청하기 위해 동영상이 종료되는 순간까지 계속해서 HTTP 통신을 보내야 하고, 계속 연결 요청을 하기 때문에 부화가 걸리게 된다.

![socket통신](https://user-images.githubusercontent.com/23491962/97810320-56c04980-1cb6-11eb-8216-0dcff9db7f97.JPG)


특징

- Server와 Client가 계속 연결을 유지하는 양방향 통신
- Server와 Client가 실시간으로 데이터를 주고받는 상황이 필요한 경우에 사용
- 실시간 동영상 스트리밍이나 온라인 게임 등에 사용

