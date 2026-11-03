# 개발 워크스테이션 구축 미션

## 1. 프로젝트 개요

이 저장소는 터미널, Docker, Git/GitHub를 사용하여 개발 워크스테이션을 구축하고 검증한 과정을 기록한 프로젝트입니다.

주요 목표:
- 터미널 기본 명령어 실습
- 파일 및 디렉토리 권한 실습
- Docker 설치 및 기본 명령어 점검
- Dockerfile 기반 웹 서버 컨테이너 실행
- 포트 매핑, 바인드 마운트, Docker 볼륨 검증
- Git/GitHub를 통한 버전 관리 및 제출

---

## 2. 실행 환경
- OS: Microsoft Windows 11 Home
- Shell: PowerShell 5.1
- Terminal: Windows PowerShell
- Docker: Docker version 29.9.2, build a82e528
- Git: git version 2.56.0.windows.4

---

## 3. 프로젝트 디렉토리 구조

```text
dev-workstation/
├── README.md
├── .gitignore
├── app/
│   ├── Dockerfile
│   └── index.html
├── docs/
├── logs/
├── screenshots/
└── bind-test/
    └── index.html
```

| 경로 | 설명 |
|---|---|
| `README.md` | 미션 수행 과정과 실행 결과를 정리한 문서 |
| `.gitignore` | Git에 포함하지 않을 파일을 지정하는 설정 파일 |
| `app/` | Dockerfile과 웹 서버용 index.html 파일을 저장한 폴더 |
| `docs/` | 실습 문서 또는 샘플 파일을 저장하는 폴더 |
| `logs/` | 명령어 실행 결과나 증거 로그를 저장하는 폴더 |
| `screenshots/` | 실행 화면 캡처 이미지를 저장하는 폴더 |
| `bind-test/` | 바인드 마운트 실습에 사용한 호스트 폴더 |
-----

## 4. 수행 체크리스트

- [x] 터미널 기본 조작
- [x] 파일/디렉토리 생성, 복사, 이동, 삭제
- [x] 파일 권한 변경 실습
- [x] 디렉토리 권한 변경 실습
- [x] Docker 버전 확인
- [x] Docker 데몬 동작 확인
- [x] hello-world 컨테이너 실행
- [x] ubuntu 컨테이너 실행
- [x] Dockerfile 작성
- [x] 커스텀 이미지 빌드
- [x] 포트 매핑 접속 확인
- [x] 바인드 마운트 검증
- [x] Docker 볼륨 영속성 검증
- [x] Git 설정
- [x] GitHub 저장소 연동
- [x] 트러블슈팅 2건 이상 기록

---

## 5. 터미널 기본 조작 로그

### 현재 위치 확인

명령어:

```powershell
pwd
```

결과:

```text
Path
----
C:\Users\user\codyssey\dev-workstation
```

---

### 폴더 목록 확인

명령어:

```powershell

ls

```

결과: 

```text

디렉터리: C:\Users\user\codyssey\dev-workstation


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-10-30   오후 1:13                app
d-----      2026-10-30   오후 1:13                docs
d-----      2026-10-30   오후 1:13                logs
d-----      2026-10-30   오후 1:13                screenshots
-a----      2026-10-30   오후 1:18            184 .gitignore
-a----      2026-10-30   오후 3:41           2877 README.md
```

------

## hello-world 컨테이너 실행

```
PS C:\Users\user\codyssey\dev-workstation> Write-Output "=== hello-world 컨테이너 실행 ==="
=== hello-world 컨테이너 실행 ===
PS C:\Users\user\codyssey\dev-workstation> docker run --rm hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 ```
---

## ubuntu 컨테이너 실행

Ubuntu 이미지를 사용하여 간단한 명령을 실행하고 컨테이너가 정상적으로 동작하는지 확인했다.

```powershell
docker run --rm ubuntu echo "ubuntu container test"
```

예상 결과:

```text
ubuntu container test
```

`--rm` 옵션을 사용했기 때문에 명령 실행이 끝난 뒤 컨테이너는 자동으로 삭제된다.

## 6. 권한 실습 로그

Docker의 Alpine Linux 컨테이너에서 `ls -l`, `chmod` 명령을 실행하여 파일 권한 변경을 확인했다.

```
PS C:\Users\user\codyssey\dev-workstation> docker run --rm alpine sh -c "mkdir -p /tmp/perm && echo hello > /tmp/perm/file.txt && ls -l /tmp/perm/file.txt && chmod 600 /tmp/perm/file.txt && ls -l /tmp/perm/file.txt && chmod 755 /tmp/perm && ls -ld /tmp/perm"

Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
7b229517ed22: Pull complete
3ec36f3670df: Download complete
3da1513d2124: Download complete
Digest: sha256:be2656061b536c1883c4034790e21d0a09c5aaaf51968b89368a3c0371ea2c9a
Status: Downloaded newer image for alpine:latest
-rw-r--r--    1 root     root             6 Nov  1 02:30 /tmp/perm/file.txt
-rw-------    1 root     root             6 Nov  1 02:30 /tmp/perm/file.txt
drwxr-xr-x    2 root     root          4096 Nov  1 02:30 /tmp/perm
```
---
Linux 권한 표기는 `rwx`로 표현된다.

- `r`: read, 읽기 권한
- `w`: write, 쓰기 권한
- `x`: execute, 실행 권한
- `600`: 소유자는 읽기/쓰기 가능, 그룹과 기타 사용자는 권한 없음
- `755`: 소유자는 읽기/쓰기/실행 가능, 그룹과 기타 사용자는 읽기/실행 가능

`chmod 600`은 파일 소유자에게만 읽기/쓰기 권한을 주고,  
`chmod 755`는 소유자에게 읽기/쓰기/실행 권한을, 그룹과 기타 사용자에게 읽기/실행 권한을 부여한다.

이번 실습에서 `chmod 600`을 적용한 이유는 파일을 소유자만 읽고 쓸 수 있도록 제한하기 위해서이다.  
이 설정은 설정 파일이나 민감한 데이터처럼 다른 사용자가 접근하면 안 되는 파일에 사용할 수 있다.

반대로 디렉토리에 `chmod 755`를 적용하면 소유자는 내용을 수정할 수 있고, 다른 사용자는 디렉토리에 접근하고 내용을 조회할 수 있다.  
권한을 너무 넓게 부여하면 보안 위험이 생길 수 있으므로 필요한 범위에서만 권한을 부여해야 한다.


## 7. Docker 설치 및 기본 점검
```
PS C:\Users\user\codyssey\dev-workstation> docker version
Client:
 Version:           29.9.2
 API version:       1.56
 Go version:        go1.26.10
 Git commit:        a82e528
 Built:             Tue Oct 20 19:44:07 2026
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.97.0 (244584)
 Engine:
  Version:          29.9.2
  API version:      1.56 (minimum version 1.40)
  Go version:       go1.26.10
  Git commit:       0367cbe
  Built:            Tue Oct 20 18:45:02 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.4.2
  GitCommit:        b7a25261e029d68978bbf290df0a62fc819b3c51
 runc:
  Version:          1.5.2
  GitCommit:        v1.5.2-0-g29dd3dc2
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
---
```
PS C:\Users\user\codyssey\dev-workstation> docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     C:\Users\user\.docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Users\user\.docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     C:\Users\user\.docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.1
    Path:     C:\Users\user\.docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     C:\Users\user\.docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.97.0
    Path:     C:\Users\user\.docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.9
    Path:     C:\Users\user\.docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     C:\Users\user\.docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Users\user\.docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.2
    Path:     C:\Users\user\.docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     C:\Users\user\.docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.21
    Path:     C:\Users\user\.docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     C:\Users\user\.docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Users\user\.docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.27.0
    Path:     C:\Users\user\.docker\cli-plugins\docker-scout.exe

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 29.9.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: local
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Discovered Devices:
  cdi: docker.com/gpu=webgpu
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2 nvidia
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b7a25261e029d68978bbf290df0a62fc819b3c51
 runc version: v1.5.2-0-g29dd3dc2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.18.54.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 3.67GiB
 Name: docker-desktop
 ID: 2dbcdef2-b322-48dd-89d4-1e404df38a7f
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
 ```
------------
```
PS C:\Users\user\codyssey\dev-workstation> Write-Output "=== 컨테이너 목록 확인 ==="
=== 컨테이너 목록 확인 ===
PS C:\Users\user\codyssey\dev-workstation> docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED      STATUS                  PORTS     NAMES
29ee7c68f455   hello-world   "/hello"   2 days ago   Exited (0) 2 days ago             vigilant_tesla
```
----------
```
PS C:\Users\user\codyssey\dev-workstation> Write-Output "=== 이미지 목록 확인 ==="
=== 이미지 목록 확인 ===
PS C:\Users\user\codyssey\dev-workstation> docker images
                                                                                                   i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
alpine:latest        be2656061b53         13MB         3.94MB
hello-world:latest   c567b231f656       25.9kB         9.49kB    U
```
---------
```
PS C:\Users\user\codyssey\dev-workstation> Write-Output "=== Dockerfile 내용 ==="
=== Dockerfile 내용 ===
PS C:\Users\user\codyssey\dev-workstation> Get-Content .\app\Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```
---------
```
PS C:\Users\user\codyssey\dev-workstation> Write-Output "=== Docker 이미지 빌드 ==="
=== Docker 이미지 빌드 ===
PS C:\Users\user\codyssey\dev-workstation> docker build -t dev-workstation-web:1.0 .\app
[+] Building 8.4s (8/8) FINISHED
```
-----------

Docker 이미지는 컨테이너를 만들기 위한 실행 템플릿이다.  
컨테이너는 이미지를 기반으로 실제 실행 중인 프로세스이다.  
예를 들어 `dev-workstation-web:1.0`은 이미지이고, `dev-web`은 그 이미지로 실행한 컨테이너이다.
--------

## 8. Docker 기본 운영 명령

```
PS C:\Users\user\codyssey\dev-workstation> @"
>> <!doctype html>
>> <html>
>> <head>
>>   <meta charset="utf-8">
>>   <title>Dev Workstaion</title>
>> </had>
>> <body>
>>   <h1>Dec Workstation Docker Test<h1>
>>   <p>Dockerfile build and port mapping test.</p>
>> </body>
>> </html>
>> "@ | Set-Content -Encoding UTF8 .\app\index.html
```
---
```
PS C:\Users\user\codyssey\dev-workstation> @"
>> FROM nginx:alpine
>> COPY index.html /usr/share/nginx/html/index.html
>> EXPOSE 80
>> "@ | Set-Content -Encoding UTF8 .\app\Dockerfile
```
-------
```
S C:\Users\user\codyssey\dev-workstation> Write-Output "=== Dockerfile 내용 ==="
=== Dockerfile 내용 ===
PS C:\Users\user\codyssey\dev-workstation> Get-Content .\app\Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
PS C:\Users\user\codyssey\dev-workstation> Write-Output "=== Docker 이미지 빌드 ==="
=== Docker 이미지 빌드 ===
PS C:\Users\user\codyssey\dev-workstation> docker build -t dev-workstation-web:1.0 .\app
[+] Building 8.4s (8/8) FINISHED 
```

## 9. Dockerfile 기반 웹 서버
```
PS C:\Users\user\codyssey\dev-workstation> curl http://localhost:8080

보안 경고: 스크립트 실행 위험
Invoke-WebRequest는 웹 페이지의 내용을 구문 분석합니다. 페이지를 구문 분석할 때 웹 페이지 내 스크립트 코드가 실행될 수
있습니다.
      권장 조치:
      -UseBasicParsing 스위치를 사용하여 스크립트 코드 실행을 방지합니다.

      계속하시겠어요?

[Y] 예(Y)  [A] 모두 예(A)  [N] 아니요(N)  [L] 모두 아니요(L)  [S] 일시 중단(S)  [?] 도움말 (기본값은 "N"): y


StatusCode        : 200
StatusDescription : OK
Content           : ï»¿<!doctype html>
                    <html>
                    <head>
                      <meta charset="utf-8">
                      <title>Dev Workstaion</title>
                    </had>
                    <body>
                      <h1>Dec Workstation Docker Test<h1>
                      <p>Dockerfile build and port mapping test.</p>
                    </body>
                    <...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 208
                    Content-Type: text/html
                    Date: Sun, 01 Nov 2026 03:05:10 GMT
                    ETag: "6ae6a8ce-d0"
                    Last-Modified: Sun, 01 Nov 2026 0...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 208], [Content-Type, text/html]
                    ...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 208
```
---

## 10. 포트 매핑 접속 증거
```
PS C:\Users\user\codyssey\dev-workstation> docker rm -f dev-web 2>$null
PS C:\Users\user\codyssey\dev-workstation> docker run -d --name dev-web -p 8080:80 dev-workstation-web:1.0
338c8b529713cba8b6ce92f147b303795d1e7abccfa8dcf990cff09ce2151335
PS C:\Users\user\codyssey\dev-workstation> docker ps
CONTAINER ID   IMAGE                     COMMAND                   CREATED          STATUS          PORTS                                     NAMES
338c8b529713   dev-workstation-web:1.0   "/docker-entrypoint.…"   13 seconds ago   Up 12 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   dev-web
```

## 10-1. 포트 매핑과 네트워크 네임스페이스 설명

Docker 컨테이너는 기본적으로 호스트와 분리된 네트워크 환경에서 실행된다.  
이처럼 프로세스별로 독립된 네트워크 환경을 제공하는 기능을 네트워크 네임스페이스라고 한다.

컨테이너 내부의 nginx 웹 서버는 80번 포트에서 실행되지만, 호스트 PC에서 바로 컨테이너 내부 포트에 접근할 수는 없다.  
따라서 `-p 8080:80` 옵션을 사용하여 호스트의 8080 포트를 컨테이너의 80 포트와 연결했다.

```powershell
docker run -d --name dev-web -p 8080:80 dev-workstation-web:1.0
```

| 구분 | 의미 |
|---|---|
| `8080` | 호스트 Windows에서 접근하는 포트 |
| `80` | 컨테이너 내부 nginx가 사용하는 포트 |
| `-p 8080:80` | 호스트 8080 포트를 컨테이너 80 포트로 전달 |

이 설정 덕분에 브라우저 또는 PowerShell에서 다음 주소로 접속할 수 있다.

```text
http://localhost:8080
```

## 10-2. 포트 충돌 진단 및 해결 방법

컨테이너 실행 시 호스트의 8080 포트를 이미 다른 프로그램이나 컨테이너가 사용 중이면 포트 충돌이 발생할 수 있다.

### 포트 사용 여부 확인

PowerShell에서 다음 명령어로 8080 포트를 사용하는 프로세스가 있는지 확인할 수 있다.

```powershell
netstat -ano | findstr :8080
```

출력이 있다면 8080 포트가 이미 사용 중이라는 의미이다.

### 실행 중인 Docker 컨테이너 확인

```powershell
docker ps
```

기존 컨테이너가 8080 포트를 사용하고 있다면 다음 명령어로 중지하거나 삭제할 수 있다.

```powershell
docker stop dev-web
docker rm dev-web
```

또는 강제로 삭제할 수 있다.

```powershell
docker rm -f dev-web
```

### 다른 포트로 실행

8080 포트를 사용할 수 없다면 호스트 포트를 8081로 변경하여 실행할 수 있다.

```powershell
docker run -d --name dev-web-8081 -p 8081:80 dev-workstation-web:1.0
```

이 경우 접속 주소는 다음과 같다.

```text
http://localhost:8081
```

## 11. 바인드 마운트 검증

호스트의 `bind-test` 폴더를 nginx 컨테이너의 `/usr/share/nginx/html` 경로에 연결

처음 접속 결과 :
```
PS C:\Users\user\codyssey\dev-workstation>> $bindPath = (Resolve-Path .\bind-test).Path
PS C:\Users\user\codyssey\dev-workstation>> docker run -d --name bind-web -p 8081:80 -v "${bindPath}:/usr/share/nginx/html:ro" nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
Digest: sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed0606
Status: Downloaded newer image for nginx:alpine
b81b681ce4c3cef3faa86dcd2f80e5dabf9e4ccdb1c9abea39196a59b96c1344
PS C:\Users\user\codyssey\dev-workstation>> curl http://localhost:8081

tatusCode        : 200
StatusDescription : OK
Content           : ï»¿<!doctype html>
                    <html>
                    <body>
                      <h1>Bind Mount Test v1</h1>
                    </body>
                    </html>
---
```

호스트의 `bind-test/index.html` 파일을 수정한 뒤 다시 접속한 결과:
```
PS C:\Users\user\codyssey\dev-workstation>> @"
>> <!doctype html>
>> <html>
>> <body>
>>   <h1>Bind Mount Test v2</h1>
>>   <p>Updated from host folder.</p>
>> </body>
>> </html>
>> "@ | Set-Content -Encoding UTF8 .\bind-test\index.html
PS C:\Users\user\codyssey\dev-workstation>> curl http://localhost:8081

StatusCode        : 200
StatusDescription : OK
Content           : ï»¿<!doctype html>
                    <html>
                    <body>
                      <h1>Bind Mount Test v2</h1>
                      <p>Updated from host folder.</p>
                    </body>
                    </html>
```                    
-------

## 12. Docker 볼륨 영속성 검증

Docker 볼륨 생성 및 저장
```
PS C:\Users\user\codyssey\dev-workstation>> docker volume create dev-volume-test
dev-volume-test
PS C:\Users\user\codyssey\dev-workstation>> docker volume ls
DRIVER    VOLUME NAME
local     dev-volume-test
PS C:\Users\user\codyssey\dev-workstation>> docker run --rm -v dev-volume-test:/data nginx:alpine sh -c "echo Docker volume persistence test > /data/message.txt"
```

새 컨테이너에서 같은 볼륨을 연결해서 데이터 확인 :
```
PS C:\Users\user\codyssey\dev-workstation>> docker run --rm -v dev-volume-test:/data nginx:alpine cat /data/message.txt
Docker volume persistence test
```

`--rm` 옵션으로 컨테이너는 삭제되었지만, Docker 볼륨에 저장된 `message.txt` 데이터는 유지되었다.  
이를 통해 Docker 볼륨은 컨테이너 생명주기와 독립적으로 데이터를 보관한다는 것을 확인했다.

---

## 13. Git/GitHub 설정 및 연동

### Git 저장소 초기화

명령어:
```powershell
git init
git branch -M main
git remote add origin https://github.com/jjj4676-ctrl/ia-codyssey
git push -u origin main
```

Github 저장소에 `README.md`와 기본 폴더 구조를 업로드했다.

---

## 14. 트러블슈팅

### 문제 1: Docker API 연결 실패

- 문제: Ubuntu 컨테이너 실행 명령을 실행했지만 Docker API에 연결하지 못하는 오류 발생

```powershell
docker run --rm ubuntu echo "ubuntu container test"
failed to connect to the Docker API at npipe:////./pipe/dockerDesktopLinuxEngine
The system cannot find the file specified.
```

- 원인 가설: Docker Desktop이 실행 중이 아니거나, Docker Desktop의 Linux 엔진이 아직 준비되지 않은 상태로 판단

- 확인: Docker 서버가 정상적으로 동작하는지 확인하기 위해 다음 명령어를 사용

```docker version```
```docker info```

- 해결: Docker Desktop을 실행하고 엔진이 준비될 때까지 기다린 뒤 다시 명령어를 실행

```powershell
PS C:\Users\user\codyssey\dev-workstation> docker run --rm ubuntu echo "ubuntu container test"
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
edafa32f5d9e: Pull complete
f3f25b84a7d7: Pull complete
21594acc2d8b: Download complete
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
ubuntu container test
```



### 문제 2 : PowerShell curl 실행 시 보안 경고 발생

- 문제: `curl http://localhost:8080` 명령을 실행했을 때 PowerShell에서 스크립트 실행 관련 보안 경고 표시
```text
보안 경고: 스크립트 실행 위험
Invoke-WebRequest는 웹 페이지의 내용을 구문 분석합니다.
```

- 원인 가설: Windows PowerShell에서 `curl`은 실제 curl 프로그램이 아니라 `Invoke-WebRequest`의 별칭으로 동작한다. 이 때문에 HTML 응답을 파싱하는 과정에서 보안 경고가 출력된 것으로 판단

- 확인: 경고에 `Invoke-WebRequest`라는 문구가 표시되었고, 사용자가 `y`를 입력한 뒤 HTTP 응답 상태 코드 `200` `OK`가 출력

- 해결: 응답 확인 목적이라면 다음처럼 `-UseBasicParsing` 옵션을 사용할 수 있다.
```Invoke-WebRequest http://localhost:8080 -UseBasicParsing```
또는
```curl.exe http://localhost:8080```
---

## 15. 배운 점


아직 작성 전입니다.

