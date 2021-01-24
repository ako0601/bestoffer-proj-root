# bestoffer-project

로컬 개발환경 구성을 위한 프로젝트 루트

## Build Setup

### 공통 요구사항

1. Node.js
2. Adonis.js CLI [바로가기](https://adonisjs.com/docs/4.1/installation)
3. (Mac OS, Windows) Docker Desktop 설치 (v20.10 이상) [바로가기] (https://www.docker.com/products/docker-desktop)
4. (Mac OS) VMware (도커 설치시)
5. (Windows) WSL2 + Ubuntu (도커 설치시)

### :bulb: 로컬 개발환경 구축 방법

- 구성요소: 컨테이너 네트워크, API 서버(Adonis.js), 프론트엔드 서버(Nuxt.js), Nginx Proxy 서버
- 구성방법
```bash
# 이 저장소 클론하기
git clone https://github.com/ako0601/bestoffer-proj-root.git bestoffer

# 저장소로 이동
cd bestoffer

# 이 저장소에 프론트, 서버 레포를 각각 client, server 폴더로 클론
git clone https://github.com/ako0601/bestoffer-client.git client
git clone https://github.com/ako0601/bestoffer-server.git server

# 각각의 서비스 (client, server) 에서 최신의 develop 브랜치로 변경
# 클라이언트로 이동
cd client
git checkout -t origin/develop
cd ..
# 서버로 이동
cd server
git checkout -t origin/server
cd ..

# 시크릿 구성하기

# 프록시 구성을 위한 호스트파일 수정: (Mac OS, Linux) /ect/hosts (Windows) C:\Windows\System32\drivers\etc\hosts

```
hosts 파일 추가 항목: 해당 파일 안에는 ip 주소와 DNS 이름이 매핑되어있는 형태로 저장이 되어있는데, 브라우저에 localhost 로 접속하면 127.0.0.1 로 접속되는 이유는 hosts 파일에서 localhost 라는 이름을 127.0.0.1 로 매핑해주고 있기 때문.

```
# 이 줄을 찾아서 아래 다음과 같이 추가함
127.0.0.1 localhost

# 추가할 항목
127.0.0.1 local-f.bestoffer.com
127.0.0.1 local-b.bestoffer.com
```

프로젝트 루트 (bestoffer) 디렉토리로 이동해서 docker-compose 실행하기

기존 이미지가 없을경우 자동으로 빌드되고, 기존 이미지가 있을 때 신규 이미지를 빌드할 경우 빌드 옵션으로 실행해야 함.

1. 기존 이미지로 구성하면서 해당 컴포저에 구성되어있는 모든 컨테이너의 로그 관찰
    ```bash
    docker-compose up
    # 종료시에는 ctrl + c 로 로깅 종료 후
    docker-compose down
    ```
2. 기존 이미지로 구성하면서 데몬으로 실행시키는 법 (빌드 완료 후 로그 생략)
    ```bash
    docker-compose up -d
    # 종료시에는
    docker-compose down
    ```
3. 신규 이미지를 구성하면서 (업데이트) 컴포저를 구성하는 법
    
    ##### 이 방법은 처음 실행할때가 아니라, 서버나 클라이언트 쪽 npm 페키지 정보가 업데이트 되거나 새로운 env를 적용할 때 해당 됨.
    ```bash
    # 데몬 플레그를 추가할 수도 있음, 택 1
    docker-compose up --build
    docker-compose up --build -d

    # 종료시 데몬 플레그를 추가하지 않고 실시간 로깅을 활성화 시킨 경우 ctrl + c
    docker-compose down
    ```

이후 브라우저에서 http://local-f.bestoffer.com:3000 에서 프론트 페이지를 볼 수 있고, http://local-b.bestoffer.com:3333 에서 백엔드 웰컴 메세지 api 를 확인할 수 있음.

---
### TEAM OAKS 콜라보레이션 [Notion 바로가기](https://www.notion.so/Collaboration-22c1188040124b10a4edeed2557a731f)