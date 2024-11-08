물론이죠! 요청하신 내용을 보완하여 Docker 기본 명령어들을 정리했습니다. 추가 옵션과 설명을 포함하여 최대한 상세하게 작성하였으니 참고하시기 바랍니다.

### 1. Docker 이미지 관련 명령어

1. **이미지 검색 (`docker search`)**
   ```bash
   docker search nginx
   ```
    - Docker Hub에서 `nginx` 관련 이미지를 검색합니다.

2. **이미지 다운로드 (`docker pull`)**
   ```bash
   docker pull nginx
   ```
    - `nginx` 이미지를 로컬에 다운로드합니다.
    - 이미지를 미리 다운로드하지 않더라도 컨테이너를 실행할 때 자동으로 다운로드됩니다.
    - **추가 명령어**
        - `docker pull nginx:latest` : `latest` 태그가 붙은 `nginx` 이미지를 다운로드합니다.

3. **이미지 목록 확인 (`docker images`)**
   ```bash
   docker images
   ```
    - 로컬에 저장된 Docker 이미지 목록을 확인합니다.
    - **추가 명령어**
        - `docker images -a` : 모든 이미지를 확인합니다 (중간 단계 이미지 포함).
        - `docker images -q` : 이미지 ID만 확인합니다.
        - `docker images -f dangling=true` : 사용되지 않는, 불완전한 (dangling) 이미지만 확인합니다.
        - `docker images --no-trunc` : 이미지 ID를 전체로 출력합니다.
        - `docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"` : 특정 포맷으로 이미지 목록을 확인합니다.

4. **이미지 삭제 (`docker rmi`)**
   ```bash
   docker rmi nginx
   ```
    - `nginx` 이미지를 로컬에서 삭제합니다.
    - **추가 명령어**
        - `docker rmi $(docker images -q)` : 모든 이미지를 삭제합니다.
        - `docker rmi $(docker images -q -f dangling=true)` : 사용되지 않는(dangling) 이미지를 삭제합니다.

### 2. Docker 컨테이너 관련 명령어

1. **컨테이너 실행 (`docker run`)**
   ```bash
   docker run -d -p 8080:80 --name my_nginx nginx
   ```
    - `nginx` 이미지를 기반으로 컨테이너를 백그라운드에서 실행합니다 (`-d`).
    - 호스트의 `8080` 포트를 컨테이너의 `80` 포트와 연결하며, 컨테이너 이름을 `my_nginx`로 설정합니다.
    - **추가 명령어**
        - `docker run -it --name my_ubuntu ubuntu` : `ubuntu` 이미지를 인터랙티브 모드로 실행하여 Bash 쉘에 접속합니다.
        - `docker run -v /path/to/html:/usr/share/nginx/html nginx` : 호스트의 `/path/to/html`을 컨테이너 내부의 `/usr/share/nginx/html`에 마운트합니다.
        - `docker run --network my_network --name my_nginx nginx` : 사용자 정의 네트워크 `my_network`에 연결하여 실행합니다.

2. **실행 중인 컨테이너 확인 (`docker ps`)**
   ```bash
   docker ps
   ```
    - 현재 실행 중인 컨테이너 목록을 확인합니다.
    - **추가 명령어**
        - `docker ps -a` : 실행 중이지 않은 컨테이너를 포함하여 모든 컨테이너를 확인합니다.
        - `docker ps -q` : 컨테이너 ID만 출력합니다.
        - `docker ps --filter "status=exited"` : 종료된 컨테이너만 확인합니다.

3. **컨테이너 정지 (`docker stop`)**
   ```bash
   docker stop my_nginx
   ```
    - `my_nginx` 컨테이너를 정지합니다.
    - **추가 명령어**
        - `docker stop $(docker ps -q)` : 모든 실행 중인 컨테이너를 정지합니다.

4. **컨테이너 삭제 (`docker rm`)**
   ```bash
   docker rm my_nginx
   ```
    - 정지된 `my_nginx` 컨테이너를 삭제합니다.
    - **추가 명령어**
        - `docker rm $(docker ps -a -q)` : 모든 정지된 컨테이너를 삭제합니다.
        - `docker rm -f my_nginx` : 실행 중인 컨테이너를 강제로 삭제합니다.

5. **컨테이너 로그 확인 (`docker logs`)**
   ```bash
   docker logs my_nginx
   ```
    - `my_nginx` 컨테이너의 로그를 확인합니다.
    - **추가 명령어**
        - `docker logs -f my_nginx` : 로그를 실시간으로 스트리밍합니다.
        - `docker logs --tail 10 my_nginx` : 마지막 10줄의 로그를 확인합니다.

6. **실행 중인 컨테이너에 들어가기 (`docker exec`)**
   ```bash
   docker exec -it my_nginx /bin/bash
   ```
    - `my_nginx` 컨테이너 내부로 접속하여 Bash 쉘을 실행합니다.

### 3. Docker 이미지 빌드 및 네트워크 관련 명령어

1. **Dockerfile로 이미지 빌드 (`docker build`)**
   ```bash
   docker build -t my_custom_nginx .
   ```
    - 현재 디렉토리에 있는 `Dockerfile`을 이용하여 `my_custom_nginx`라는 이름의 이미지를 빌드합니다.

2. **네트워크 목록 확인 (`docker network ls`)**
   ```bash
   docker network ls
   ```
    - Docker 네트워크 목록을 확인합니다.

3. **새로운 네트워크 생성 (`docker network create`)**
   ```bash
   docker network create my_network
   ```
    - 이름이 `my_network`인 사용자 정의 네트워크를 생성합니다.

4. **네트워크에 컨테이너 연결 (`docker network connect`)**
   ```bash
   docker network connect my_network my_nginx
   ```
    - `my_nginx` 컨테이너를 `my_network` 네트워크에 연결합니다.

### 4. Docker 볼륨 관련 명령어

1. **볼륨 생성 (`docker volume create`)**
   ```bash
   docker volume create my_volume
   ```
    - `my_volume`이라는 이름의 볼륨을 생성합니다.

2. **컨테이너에 볼륨 마운트 (`docker run`에서 `-v` 옵션 사용)**
   ```bash
   docker run -d -p 8080:80 --name my_nginx -v my_volume:/usr/share/nginx/html nginx
   ```
    - `my_volume` 볼륨을 컨테이너 내부의 `/usr/share/nginx/html` 경로에 마운트하여 실행합니다.

3. **볼륨 목록 확인 (`docker volume ls`)**
   ```bash
   docker volume ls
   ```
    - 현재 Docker 볼륨 목록을 확인합니다.

4. **볼륨 삭제 (`docker volume rm`)**
   ```bash
   docker volume rm my_volume
   ```
    - `my_volume`이라는 볼륨을 삭제합니다. 사용 중인 볼륨은 삭제할 수 없습니다.

### 5. Docker Compose 관련 명령어

1. **Compose로 서비스 실행 (`docker compose up`)**
   ```bash
   docker-compose up -d
   ```
    - 현재 디렉토리의 `docker-compose.yml` 파일을 사용하여 서비스를 백그라운드에서 실행합니다.
    - **추가 명령어**
        - `docker compose up --build` : 이미지를 새로 빌드하고 서비스를 실행합니다.

2. **Compose로 서비스 중지 (`docker compose down`)**
   ```bash
   docker compose down
   ```
    - 실행 중인 모든 컨테이너를 종료하고 네트워크 및 관련 리소스를 정리합니다.
    - **추가 명령어**
        - `docker-compose down --volumes` : 종료 후 사용된 볼륨도 삭제합니다.

3. **특정 서비스만 실행 (`docker compose up <service_name>`)**
   ```bash
   docker compose up -d web
   ```
    - `docker-compose.yml`에서 `web` 서비스만 실행합니다.

4. **Compose 로그 확인 (`docker compose logs`)**
   ```bash
   docker compose logs
   ```
    - 모든 서비스의 로그를 확인합니다.
    - **추가 명령어**
        - `docker compose logs -f web` : `web` 서비스의 로그를 실시간으로 스트리밍합니다.

5. **Compose 서비스 재시작 (`docker compose restart`)**
    ```bash
    docker compose restart
    ```
     - 모든 서비스를 재시작합니다.
     - **추가 명령어**
          - `docker compose restart web` : `web` 서비스만 재시작합니다.
    ```
   
5. **Compose 삭제 (`docker compose rm`)**
   ```bash
   docker compose rm
   ```
    - 모든 서비스를 삭제합니다.
    - **추가 명령어**
        - `docker compose rm -f` : 실행 중인 컨테이너를 강제로 삭제합니다.



