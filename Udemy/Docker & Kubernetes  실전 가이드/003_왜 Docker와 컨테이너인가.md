---
created: 2024-07-16 23:23
course-name: Docker & Kubernetes 실전 가이드
instructor: Maximilian Schwarzmüller
section: "섹션 1: 시작하기"
presentation: 
tags:
  - lecture-note
  - "#udemy"
---
## 강의 노트
#### Why containers?
- **왜** 우리는 소프트웨어 개발에서 **독립적인 표준화된 애플리케이션 패키지**를 원하는 것인가
##### 예시 1
- NodeJS 애플리케이션을 생성했다고 가정
	- NodeJS 버전 14.3에서 코드를 작성했음
		- 이 코드를 성공적으로 실행하려면 NodeJS 14.3 이상이 필요함 
			- 우리 컴퓨터에서 로컬로 작동하던 코드가 원격 시스템에서는 작동하지 않을 수 있음
- 도커와 컨테이너는 **제품 생산에서 가지고 있는 것과 똑같은 개발 환경을 갖도록 도와준다.** 
	- **특정 NodeJS 버전을 도커 컨테이너에 Lock** 할 수 있으므로
		- 코드가 정확한 버전으로 실행하도록 할 수 있음

##### 예시 2
- 팀이나 회사 내 각각의 개발 환경에서
	- 큰 팀에 속해 있을 때, 동일한 NodeJS 애플리케이션 프로젝트 작업을 하고 있음
		- 나는 한동안 Node로 작업하지 않거나, 업데이트할 필요가 없어서 여전히 이전 버전임
			- 다른 팀원은 최신버전을 가지고 최상위 수준의 await로 코드를 작성했을 때
				- 이 코드를 공유하면 나는 작동하지 않을 것임
- NodeJS 업데이트는 큰 문제가 되지 않을 것임
	- 하지만, 실제로는 관리하고 설치해야 하는 더 복잡한 종속성이 있는 프로젝트일 것이다
- 컨테이너에 코드가 필요로 하는 모든 것을 포함하는 환경을 보유하는 것은 상당한 가치가 있다.

##### 예시 3
- 혼자 작업하는 경우에도 도커와 컨테이너가 매우 유용
- **작업 중인 프로젝트가 여러 개**인 경우
	- 충돌하는 버전이 있을 수 있음
	- 어떤 프로젝트는 Python 버전 2를 사용하고, 다른 프로젝트에서는 최신 버전의 Python을 사용한다고 가정하면
		- 프로젝트를 전환할 때마다 잘못된 버전을 제거하고 올바른 버전을 설치해야 함을 의미
			- 매우 번거로우며 귀찮아 짐
- 도커는 이를 도와줄 것이다
	- 각 버전을 컨테이너에 보유하고 각 프로젝트에는 그들만의 컨테이너가 존재하도록 함 
		- 따라서 프로젝트를 전환하면 그대로 작동 
		- 매번 제거하고 설치할 필요가 없음 
---
## 다음 강의 노트 [[005_가상머신 vs Docker 컨테이너]]
