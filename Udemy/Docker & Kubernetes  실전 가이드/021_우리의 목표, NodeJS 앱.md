---
created: 2025-02-05 18:40
course-name: Docker & Kubernetes 실전 가이드
instructor: Maximilian Schwarzmüller
section: "섹션 2: Docker 이미지 & 컨테이너: 코어 빌딩 블록"
presentation: 
tags:
  - lecture-note
  - "#udemy"
last_modified: 2025-02-05T18:52:00
---
> - 이미지를 다운받지 않고, 자신만의 이미지를 구축하여 노드JS 코드를 실행할 수 있다.
> - 일반적으로 공식 베이스 이미지를 가져와서 그 위에 코드를 추가하여 그 이미지로 코드를 실행한다.
---
> (강의 자료 다운로드 필요)
- 기본 노드 애플리케이션 코드가 포함된 `server.js`
	- 노드JS로 웹 서버를 실행하고, 포트 80번에서 수신 대기하여 `/` URL로 들어오는 요청을 처리. (노드에 대한 이해 필요 X)

- 이 프로젝트를 도커없이 로컬로 실행하려면 노드JS를 설치해야 한다.
	- 그리고 터미널을 열고 `npm install`로 모든 종속성을 다운하고,
	- `node server.js` 를 입력하여 서버를 시작하고
	- `localhost:80`에 방문하여 작동을 확인한다.
---
## 다음 강의 노트 : [[]]
