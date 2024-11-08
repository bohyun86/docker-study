### 도커파일 설정

- 도커파일은 도커 이미지를 만들기 위한 설정 파일이다.
- 도커파일은 기본적으로 `FROM`, `RUN`, `CMD` 명령어로 구성된다.
- 도커파일은 `Dockerfile`이라는 이름으로 저장한다.
- 도커파일은 다음과 같은 명령어로 이미지를 빌드한다.

```bash
docker build -t my_custom_nginx .
```

- `my_custom_nginx`라는 이름의 이미지를 빌드한다.
- `.`은 현재 디렉토리에 있는 `Dockerfile`을 사용한다.
- `Dockerfile`은 다음과 같이 작성한다.

```Dockerfile
# 베이스 이미지
FROM nginx:latest

# 작업 디렉토리 설정
WORKDIR /usr/share/nginx/html

# 파일 복사
COPY index.html index.html

# 컨테이너 실행 시 실행할 명령어
CMD ["nginx", "-g", "daemon off;"]
```

- `FROM`: 베이스 이미지를 지정한다.
- `WORKDIR`: 작업 디렉토리를 설정한다.
- `COPY`: 파일을 복사한다.
- `CMD`: 컨테이너 실행 시 실행할 명령어를 설정한다.
- `index.html` 파일은 다음과 같이 작성한다.

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Custom Nginx</title>
</head>
<body>
    <h1>Hello, Nginx!</h1>
</body>
</html>
```

- `index.html` 파일을 `Dockerfile`과 같은 디렉토리에 저장한다.
- `Dockerfile`과 `index.html` 파일이 같은 디렉토리에 있어야 한다.
- `docker build` 명령어로 이미지를 빌드하고 실행한다.

```bash
docker build -t my_custom_nginx .
docker run -d -p 8080:80 --name my_custom_nginx my_custom_nginx
```

- `my_custom_nginx` 이미지를 빌드하고 실행한다.
- `-p 8080:80` 옵션으로 호스트의 8080 포트를 컨테이너의 80 포트로 연결한다.
- `--name my_custom_nginx` 옵션으로 컨테이너의 이름을 지정한다.
- `my_custom_nginx` 이미지를 실행하면 `index.html` 파일이 브라우저에 출력된다.
- `docker ps` 명령어로 컨테이너가 실행 중인지 확인한다.

```bash
docker ps
```

- `docker exec` 명령어로 컨테이너 내부로 접속하여 명령어를 실행할 수 있다.

```bash
docker exec -it my_custom_nginx /bin/bash
```

- `my_custom_nginx` 컨테이너 내부로 접속하여 Bash 쉘을 실행한다.
- `exit` 명령어로 컨테이너에서 빠져나온다.
- `docker stop` 명령어로 컨테이너를 중지하고 `docker rm` 명령어로 컨테이너를 삭제한다.

```bash
docker stop my_custom_nginx
docker rm my_custom_nginx
```

- `docker images` 명령어로 이미지 목록을 확인한다.

```bash
docker images
```

- `docker rmi` 명령어로 이미지를 삭제한다.

```bash
docker rmi my_custom_nginx
```

- `docker rmi $(docker images -q -f dangling=true)` 명령어로 사용되지 않는(dangling) 이미지를 삭제한다.

```bash
docker rmi $(docker images -q -f dangling=true)
```

### Dockerfile 명령어

1. **베이스 이미지 지정 (`FROM`)**
   ```Dockerfile
   FROM nginx:latest
   ```
    - `nginx:latest` 이미지를 베이스 이미지로 사용한다.
    - **추가 명령어**
        - `FROM ubuntu:18.04` : `ubuntu:18.04` 이미지를 베이스 이미지로 사용한다.
        - `FROM node:14` : `node:14` 이미지를 베이스 이미지로 사용한다.
        - `FROM python:3.8` : `python:3.8` 이미지를 베이스 이미지로 사용한다.
        - `FROM alpine:3.12` : `alpine:3.12` 이미지를 베이스 이미지로 사용한다.
        - `FROM scratch` : 빈 이미지로 사용자 정의 이미지를 생성한다.
        - `FROM my_custom_image` : `my_custom_image` 이미지를 베이스 이미지로 사용한다.
        - `FROM my_custom_image:latest` : `my_custom_image` 이미지의 `latest` 태그를 사용한다.
        - `FROM my_custom_image:1.0` : `my_custom_image` 이미지의 `1.0` 태그를 사용한다.
        - `FROM my_custom_image@sha256:123456` : `my_custom_image` 이미지의 `sha256:123456` 해시를 사용한다.
        - `FROM my_custom_image AS builder` : `my_custom_image` 이미지를 `builder` 스테이지로 사용한다.
        - `FROM my_custom_image:latest AS builder` : `my_custom_image` 이미지의 `latest` 태그를 `builder` 스테이지로 사용한다.

2. **작업 디렉토리 설정 (`WORKDIR`)**
    ```Dockerfile
    WORKDIR /usr/share/nginx/html
    ```
     - `/usr/share/nginx/html` 디렉토리를 작업 디렉토리로 설정한다.
     - **추가 명령어**
          - `WORKDIR /app` : `/app` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /var/www/html` : `/var/www/html` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /home/user` : `/home/user` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /root` : `/root` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /data` : `/data` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /var/log` : `/var/log` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /tmp` : `/tmp` 디렉토리를 작업 디렉토리로 설정한다.
          - `WORKDIR /mnt` : `/mnt` 디렉토리를 작업 디렉토리로 설정한다.

3. **파일 복사 (`COPY`)**
    ```Dockerfile
    COPY index.html index.html
    ```
     - `index.html` 파일을 현재 디렉토리의 `index.html` 파일로 복사한다.
     - **추가 명령어**
          - `COPY index.html /usr/share/nginx/html/index.html` : `index.html` 파일을 `/usr/share/nginx/html/index.html` 파일로 복사한다.
          - `COPY index.html /app/index.html` : `index.html` 파일을 `/app/index.html` 파일로 복사한다.
          - `COPY index.html /var/www/html/index.html` : `index.html` 파일을 `/var/www/html/index.html` 파일로 복사한다.
          - `COPY index.html /home/user/index.html` : `index.html` 파일을 `/home/user/index.html` 파일로 복사한다.
          - `COPY index.html /root/index.html` : `index.html` 파일을 `/root/index.html` 파일로 복사한다.
          - `COPY index.html /data/index.html` : `index.html` 파일을 `/data/index.html` 파일로 복사한다.
          - `COPY index.html /var/log/index.html` : `index.html` 파일을 `/var/log/index.html` 파일로 복사한다.
          - `COPY index.html /tmp/index.html` : `index.html` 파일을 `/tmp/index.html` 파일로 복사한다.
          - `COPY index.html /mnt/index.html` : `index.html` 파일을 `/mnt/index.html` 파일로 복사한다.

4. **파일 추가 (`ADD`)**
    ```Dockerfile
    ADD index.html index.html
    ```
     - `index.html` 파일을 현재 디렉토리의 `index.html` 파일로 추가한다.
     - **추가 명령어**
          - `ADD index.html /usr/share/nginx/html/index.html` : `index.html` 파일을 `/usr/share/nginx/html/index.html` 파일로 추가한다.
          - `ADD index.html /app/index.html` : `index.html` 파일을 `/app/index.html` 파일로 추가한다.
          - `ADD index.html /var/www/html/index.html` : `index.html` 파일을 `/var/www/html/index.html` 파일로 추가한다.
          - `ADD index.html /home/user/index.html` : `index.html` 파일을 `/home/user/index.html` 파일로 추가한다.
          - `ADD index.html /root/index.html` : `index.html` 파일을 `/root/index.html` 파일로 추가한다.
          - `ADD index.html /data/index.html` : `index.html` 파일을 `/data/index.html` 파일로 추가한다.
          - `ADD index.html /var/log/index.html` : `index.html` 파일을 `/var/log/index.html` 파일로 추가한다.
          - `ADD index.html /tmp/index.html` : `index.html` 파일을 `/tmp/index.html` 파일로 추가한다.
          - `ADD index.html /mnt/index.html` : `index.html` 파일을 `/mnt/index.html` 파일로 추가한다.

5. **환경 변수 설정 (`ENV`)**
    ```Dockerfile
    ENV MY_NAME="Docker"
    ```
     - `MY_NAME` 환경 변수를 `Docker`로 설정한다.
     - **추가 명령어**
          - `ENV MY_NAME="Docker"` : `MY_NAME` 환경 변수를 `Docker`로 설정한다.
          - `ENV MY_NAME="Docker" MY_VERSION="1.0"` : `MY_NAME` 환경 변수를 `Docker`, `MY_VERSION` 환경 변수를 `1.0`으로 설정한다.
          - `ENV MY_NAME="Docker" MY_VERSION="1.0" MY_PORT="8080"` : `MY_NAME` 환경 변수를 `Docker`, `MY_VERSION` 환경 변수를 `1.0`, `MY_PORT` 환경 변수를 `8080`으로 설정한다.
          - `ENV MY_NAME="Docker" \ MY_VERSION="1.0" \ MY_PORT="8080"` : `MY_NAME` 환경 변수를 `Docker`, `MY_VERSION` 환경 변수를 `1.0`, `MY_PORT` 환경 변수를 `8080`으로 설정한다.

6. **명령어 실행 (`RUN`)**
    ```Dockerfile
    RUN apt-get update
    ```
     - `apt-get update` 명령어를 실행한다.
     - **추가 명령어**
          - `RUN apt-get update` : `apt-get update` 명령어를 실행한다.
          - `RUN apt-get install -y nginx` : `apt-get install -y nginx` 명령어를 실행한다.
          - `RUN apt-get install -y python3` : `apt-get install -y python3` 명령어를 실행한다.
          - `RUN apt-get install -y nodejs` : `apt-get install -y nodejs` 명령어를 실행한다.
          - `RUN apt-get install -y git` : `apt-get install -y git` 명령어를 실행한다.
          - `RUN apt-get install -y curl` : `apt-get install -y curl` 명령어를 실행한다.
          - `RUN apt-get install -y wget` : `apt-get install -y wget` 명령어를 실행한다.
          - `RUN apt-get install -y vim` : `apt-get install -y vim` 명령어를 실행한다.
          - `RUN apt-get install -y nano` : `apt-get install -y nano` 명령어를 실행한다.
          - `RUN apt-get install -y net-tools` : `apt-get install -y net-tools` 명령어를 실행한다.
          - `RUN apt-get install -y iputils-ping` : `apt-get install -y iputils-ping` 명령어를 실행한다.
          - `RUN apt-get install -y iproute2` : `apt-get install -y iproute2` 명령어를 실행한다.
          - `RUN apt-get install -y iperf3` : `apt-get install -y iperf3` 명령어를 실행한다.
          - `RUN apt-get install -y iptraf-ng` : `apt-get install -y iptraf-ng` 명령어를 실행한다.
          - `RUN apt-get install -y nmap` : `apt-get install -y nmap` 명령어를 실행
          - 
7. **명령어 실행 및 쉘 실행 (`CMD`)**
    ```Dockerfile
    CMD ["nginx", "-g", "daemon off;"]
    ```
     - `nginx -g "daemon off;"` 명령어를 실행한다.
     - **추가 명령어**
          - `CMD ["nginx", "-g", "daemon off;"]` : `nginx -g "daemon off;"` 명령어를 실행한다.
          - `CMD ["python3", "app.py"]` : `python3 app.py` 명령어를 실행한다.
          - `CMD ["node", "app.js"]` : `node app.js` 명령어를 실행한다.
          - `CMD ["npm", "start"]` : `npm start` 명령어를 실행한다.
          - `CMD ["git", "--version"]` : `git --version` 명령어를 실행한다.
     - 
8. **명령어 실행 및 쉘 실행 (`ENTRYPOINT`)**
    ```Dockerfile
    ENTRYPOINT ["nginx", "-g", "daemon off;"]
    ```
     - `nginx -g "daemon off;"` 명령어를 실행한다.
     - **추가 명령어**
          - `ENTRYPOINT ["nginx", "-g", "daemon off;"]` : `nginx -g "daemon off;"` 명령어를 실행한다.
          - `ENTRYPOINT ["python3", "app.py"]` : `python3 app.py` 명령어를 실행한다.
          - `ENTRYPOINT ["node", "app.js"]` : `node app.js` 명령어를 실행한다.
          - `ENTRYPOINT ["npm", "start"]` : `npm start` 명령어를 실행한다.
          - `ENTRYPOINT ["git", "--version"]` : `git --version` 명령어를 실행한다.
-
9. **포트 연결 (`EXPOSE`)**
    ```Dockerfile
    EXPOSE 80
    ```
     - 80 포트를 외부에 노출한다.

10. **사용자 설정 (`USER`)**
    ```Dockerfile
    USER root
    ```
     - `root` 사용자로 설정한다.

11. **볼륨 마운트 (`VOLUME`)**
    ```Dockerfile
    VOLUME /usr/share/nginx/html
    ```
     - `/usr/share/nginx/html` 디렉토리를 볼륨으로 설정한다.

12. **라벨 설정 (`LABEL`)**
    ```Dockerfile
    LABEL version="1.0"
    ```
     - `version` 라벨을 `1.0`으로 설정한다.

13. **도커 빌드 인자 설정 (`ARG`)**
    ```Dockerfile
    ARG MY_NAME="Docker"
    ```
     - `MY_NAME` 도커 빌드 인자를 `Docker`로 설정한다.

## Add와 Copy의 차이점
- `ADD` 명령어는 파일과 디렉토리를 추가할 때 사용한다.
- `COPY` 명령어는 파일과 디렉토리를 복사할 때 사용한다.

## RUN, CMD, ENTRYPOINT의 차이점

`RUN`, `CMD`, `ENTRYPOINT`의 차이점과 역할을 구체적으로 살펴보겠습니다. 각각의 명령어는 Dockerfile에서 이미지와 컨테이너 동작에 중요한 역할을 합니다.

### 1. `RUN`
- **목적**: 이미지 생성 중에 실행할 명령어를 지정합니다.
- **실행 시점**: 이미지 빌드 단계에서 실행되며, 빌드 과정이 완료된 후에는 적용된 상태로 새로운 레이어를 생성합니다.
- **사용 예시**: 주로 패키지 설치, 파일 복사, 환경 구성 등의 작업을 위해 사용됩니다.
- **예시**
  ```Dockerfile
  RUN apt-get update && apt-get install -y python3
  ```
  위 명령어는 Python3를 이미지에 설치하며, 결과가 이미지에 영구적으로 저장됩니다. 컨테이너가 시작되면 이 설치가 끝난 상태에서 실행됩니다.

### 2. `CMD`
- **목적**: 컨테이너가 시작될 때 기본으로 실행할 명령어 또는 인수를 설정합니다. `CMD`는 하나만 사용할 수 있으며, 여러 개의 `CMD`가 있을 경우 마지막 `CMD`가 적용됩니다.
- **실행 시점**: 컨테이너가 시작될 때 실행되며, 런타임 중에 변경 가능하고 `docker run` 명령어에 인수를 전달하면 `CMD`는 무시됩니다.
- **ENTRYPOINT와의 관계**: `ENTRYPOINT`가 설정된 경우 `CMD`는 인수로 사용됩니다. 이를 통해 `ENTRYPOINT`가 지정된 명령어에 대한 기본 인수를 지정할 수 있습니다.
- **예시**
  ```Dockerfile
  CMD ["python3", "app.py"]
  ```
  이 경우 `docker run`으로 컨테이너를 시작하면 `python3 app.py`가 기본적으로 실행됩니다. 만약 `docker run <이미지> python3 다른_파일.py`로 실행하면 `app.py` 대신 `다른_파일.py`가 실행됩니다.

### 3. `ENTRYPOINT`
- **목적**: 컨테이너가 시작될 때 실행할 명령어를 설정합니다. `ENTRYPOINT`는 주로 특정 애플리케이션을 컨테이너에서 고정적으로 실행해야 할 때 사용합니다.
- **실행 시점**: 컨테이너가 시작될 때 실행되며, `CMD`와 달리 런타임에서 대체하기 어렵습니다. 다만 `--entrypoint` 플래그로 실행 시점을 변경할 수 있습니다.
- **CMD와의 관계**: `CMD`가 `ENTRYPOINT`에 대한 인수 역할을 할 수 있습니다. 이를 통해 `ENTRYPOINT` 명령어가 항상 실행되며, `CMD`로 기본 인수 설정이나 유연한 인수 변경이 가능합니다.
- **예시**
  ```Dockerfile
  ENTRYPOINT ["python3"]
  CMD ["app.py"]
  ```
  여기서 컨테이너가 시작되면 기본적으로 `python3 app.py`가 실행됩니다. `docker run <이미지> 다른_파일.py`로 실행하면 `python3 다른_파일.py`가 실행됩니다.

### 차이점 요약
| 명령어         | 실행 시점          | 용도                                                                                  | 런타임 변경 가능 여부               |
|----------------|--------------------|---------------------------------------------------------------------------------------|-------------------------------------|
| `RUN`          | 이미지 빌드 중     | 이미지 생성에 필요한 설정, 설치 등 수행                                               | ❌                                    |
| `CMD`          | 컨테이너 시작 시   | 컨테이너 시작 시 기본 실행 명령어 설정                                                | ⭕ (명령어나 인수를 덮어쓸 수 있음)   |
| `ENTRYPOINT`   | 컨테이너 시작 시   | 컨테이너 시작 시 고정적으로 실행할 명령어 설정 (변경하기 어려움)                       | ⭕ (`--entrypoint`로 실행 시 변경 가능) |

이렇게 `RUN`, `CMD`, `ENTRYPOINT`는 각자의 시점과 용도가 다르기 때문에 Dockerfile을 작성할 때 이미지 빌드와 컨테이너 실행 시점을 분리하고, 애플리케이션의 특성에 따라 가장 적절한 방식으로 사용하는 것이 중요합니다.

예를 들어, 아래와 같은 `Dockerfile`이 있다고 가정해 보겠습니다:

```Dockerfile
# 베이스 이미지
FROM python:3.9

# 애플리케이션 파일 복사
COPY app.py /app/app.py
COPY another_script.py /app/another_script.py

# 워킹 디렉토리 설정
WORKDIR /app

# Python 패키지 설치
RUN pip install flask

# 기본 실행 명령어 설정
ENTRYPOINT ["python3"]
CMD ["app.py"]
```

### 1. 기본 동작
이 Dockerfile을 기반으로 이미지를 빌드하고, 컨테이너를 실행했을 때의 기본 동작을 보겠습니다.

```bash
# Docker 이미지 빌드
docker build -t my-python-app .

# 컨테이너 실행 (기본 동작)
docker run my-python-app
```

위의 경우, `ENTRYPOINT ["python3"]`와 `CMD ["app.py"]`가 결합되어 `python3 app.py`가 실행됩니다.

### 2. CMD 덮어쓰기
`CMD` 명령어를 덮어쓰고 싶다면, `docker run` 명령어에 추가 인수를 전달하여 `CMD`를 대체할 수 있습니다.

```bash
# 다른 스크립트를 실행하고 싶을 때
docker run my-python-app another_script.py
```

위 명령을 실행하면, `ENTRYPOINT`는 여전히 `python3`이므로, `another_script.py`가 `python3`의 인수로 전달되어 `python3 another_script.py`가 실행됩니다.

### 3. ENTRYPOINT 덮어쓰기
`ENTRYPOINT`를 변경하고 싶다면, `docker run` 명령어에서 `--entrypoint` 옵션을 사용할 수 있습니다.

```bash
# Bash 쉘로 컨테이너를 시작하고 싶을 때
docker run --entrypoint /bin/bash my-python-app
```

이 경우, `ENTRYPOINT`가 `bash`로 대체되므로, `CMD ["app.py"]`는 인수로 전달되어 `bash app.py`가 됩니다. 이 명령은 쉘을 열고 컨테이너 안에서 작업할 수 있게 해 줍니다.

### 결과 요약
이와 같은 방식으로 `ENTRYPOINT`와 `CMD`를 조합하여 기본 동작을 설정하고, 필요할 때 이를 변경할 수 있습니다.

## VOLUME, LABEL, ARG 명령어

Dockerfile에서 `VOLUME`, `LABEL`, `ARG`는 각각 다른 목적을 가지고 있으며, 이미지를 빌드하고 관리하는 데 중요한 역할을 합니다. 아래에서 각 명령어의 역할을 자세히 설명하겠습니다.

---

### 1. `VOLUME`

- **목적**: Docker 컨테이너에서 특정 디렉토리를 호스트와 공유하거나, 컨테이너 간에 공유하도록 합니다. `VOLUME`은 영구 데이터를 유지하거나 컨테이너 삭제 시 데이터가 손실되지 않도록 하는 데 사용됩니다.
- **사용 시점**: 주로 데이터가 컨테이너 수명과 별개로 보존되어야 할 때 사용합니다. 예를 들어, 데이터베이스 파일, 로그 파일, 사용자 데이터 등을 저장할 때 유용합니다.
- **특징**:
    - `VOLUME`에 지정된 디렉토리는 호스트 시스템에 저장되어 컨테이너 삭제 후에도 데이터를 유지할 수 있습니다.
    - 다른 컨테이너와 데이터를 쉽게 공유할 수 있습니다.
    - 권한 문제를 피하기 위해 주의가 필요합니다.
- **예시**:
  ```Dockerfile
  # 컨테이너 내부의 /data 디렉토리를 호스트와 공유
  VOLUME /data
  ```
  위와 같이 `VOLUME` 명령을 Dockerfile에 지정하면, `/data` 디렉토리가 호스트의 특정 디렉토리와 연결됩니다. 예를 들어, 다음과 같이 `docker run` 명령어로 직접 호스트 디렉토리를 매핑할 수도 있습니다:

  ```bash
  docker run -v /host/data:/data my-container
  ```
  이렇게 실행하면 호스트의 `/host/data` 디렉토리가 컨테이너의 `/data`와 연결됩니다.

---

### 2. `LABEL`

- **목적**: Docker 이미지를 설명하거나 관리하기 위한 메타데이터를 지정하는 데 사용됩니다. 예를 들어, 이미지 버전, 작성자, 설명 등 이미지를 구분할 수 있는 정보를 라벨로 설정할 수 있습니다.
- **사용 시점**: 이미지의 정보를 체계적으로 관리할 때 사용합니다. 특히 여러 이미지 간의 정보 구분, 검색, 필터링에 유리합니다.
- **특징**:
    - `LABEL`은 키-값 쌍으로 메타데이터를 설정합니다.
    - 하나의 이미지에 여러 `LABEL`을 설정할 수 있습니다.
    - 이미지 검색과 관리에 유용합니다.
- **예시**:
  ```Dockerfile
  # 작성자와 버전 정보를 포함한 LABEL
  LABEL maintainer="yourname@example.com"
  LABEL version="1.0"
  LABEL description="This is a sample container for demonstration purposes"
  ```
  이후, `docker inspect` 명령어로 이미지의 메타데이터를 조회할 수 있습니다:

  ```bash
  docker inspect my-container
  ```
  `LABEL`로 설정한 정보는 JSON 형식으로 조회됩니다.

---

### 3. `ARG` (빌드 인자)

- **목적**: 이미지 빌드 시 외부에서 인수를 전달받아 Dockerfile에서 동적으로 사용할 수 있게 합니다. 예를 들어, 환경에 따라 다른 패키지 버전을 설치하거나 특정 설정을 적용할 수 있습니다.
- **사용 시점**: 빌드 환경에 따라 다른 값을 사용하고자 할 때 사용합니다. 예를 들어, 테스트, 스테이징, 프로덕션 등 환경마다 다르게 설정해야 하는 값을 지정할 때 유용합니다.
- **특징**:
    - `ARG`로 선언한 변수는 빌드 시점에만 유효하며, 런타임에는 사용할 수 없습니다.
    - 기본값을 지정할 수 있으며, `docker build` 명령어에서 `--build-arg`로 값을 전달할 수 있습니다.
- **예시**:
  ```Dockerfile
  # 빌드 인자 선언 및 기본값 설정
  ARG VERSION=latest

  # ARG 사용
  RUN echo "Building version $VERSION"
  ```
  Docker 이미지를 빌드할 때 다음과 같이 `--build-arg` 옵션으로 값을 지정할 수 있습니다:

  ```bash
  docker build --build-arg VERSION=1.2 -t my-container .
  ```

  이 경우 `VERSION` 인자는 `1.2`로 설정되어 `RUN echo "Building version $VERSION"`에서 `Building version 1.2`가 출력됩니다.

---

### 요약

| 명령어 | 목적 | 사용 시점 | 특징 |
|--------|------|-----------|------|
| `VOLUME` | 데이터 공유 및 영구 저장소 설정 | 영구적으로 보존해야 할 데이터를 저장할 때 사용 | 호스트와 데이터 공유 가능, 컨테이너 삭제 시 데이터 유지 |
| `LABEL` | 이미지 메타데이터 관리 | 이미지 설명 및 관리가 필요할 때 사용 | 키-값 쌍으로 메타데이터 제공, `docker inspect`로 조회 가능 |
| `ARG` | 빌드 시 인자 전달 | 빌드 환경에 따라 동적으로 값을 설정할 때 사용 | 빌드 시점에만 유효하며 런타임에는 사용할 수 없음 |

이렇게 `VOLUME`, `LABEL`, `ARG`는 Dockerfile의 빌드와 실행 시점에서 각각 중요한 역할을 담당하며, 이를 활용해 Docker 이미지를 효율적으로 관리할 수 있습니다.