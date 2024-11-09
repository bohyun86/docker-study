### 도커 컴포즈 작성법

Docker Compose는 여러 개의 Docker 컨테이너로 구성된 애플리케이션을 정의하고, 실행 및 관리할 수 있도록 도와주는 도구입니다. 단일 Compose 파일에서 모든 서비스(컨테이너)의 설정을 관리할 수 있어, 애플리케이션 전체를 쉽게 배포하고 중지할 수 있습니다. 이 Docker Compose 파일은 YAML 형식으로 작성되며, `docker-compose.yaml` 파일에서 각 서비스의 이미지, 네트워크, 볼륨 등을 설정할 수 있습니다.

---

### Docker Compose의 주요 기능

- **멀티 컨테이너 애플리케이션 배포**: 여러 개의 컨테이너를 함께 정의하고, 단일 명령어로 실행하거나 중지할 수 있습니다.
- **서비스 간 네트워크 설정**: 서비스 간 네트워크를 설정해 컨테이너가 서로 쉽게 통신할 수 있도록 지원합니다.
- **볼륨 및 데이터 공유**: 볼륨을 사용해 데이터를 공유하거나 영구 저장할 수 있습니다.
- **환경 설정 파일(.env)**: 설정을 외부 파일에 저장해 코드와 설정을 분리할 수 있습니다.

---

### Docker Compose 파일 구조와 작성 방법

Compose 파일은 YAML 형식으로 작성되며, 주로 다음과 같은 주요 상위 키로 구성됩니다:

1. **version**: Compose 파일의 버전을 정의합니다. 예를 들어 `version: '3'`와 같이 설정합니다.
2. **services**: 애플리케이션의 각 서비스를 정의하는 섹션입니다. 각 서비스는 하나의 컨테이너로 실행됩니다.
3. **networks**: 서비스들이 연결될 네트워크를 정의합니다.
4. **volumes**: 컨테이너 간 공유되거나 영구적으로 저장될 데이터를 위한 볼륨을 정의합니다.

---

### Docker Compose 파일 작성 예시

아래는 `docker-compose.yaml` 파일에서 기본적인 설정 예시입니다.

```yaml
version: '3'

services:
  web:
    build: .
    command: python app.py
    ports:
      - "5000:5000"
    networks:
      - app-network
    volumes:
      - app-volume:/app/data
    environment:
      - APP_ENV=production
      - DATABASE_URI=mysql://user:password@db:3306/mydb
  
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - db-volume:/var/lib/mysql
    networks:
      - app-network

networks:
  app-network:

volumes:
  app-volume:
  db-volume:
```

---

### 호스트 경로 직접 지정하는 방법

```yaml
services:
web:
volumes:
- /path/on/host:/app/data  # 호스트의 특정 경로를 컨테이너의 경로에 마운트
```
---

### 각 항목의 상세 설명

#### 1. `version`
- Docker Compose 파일의 버전을 지정합니다.
- 최신 버전의 Docker Compose에서는 `version: '3'`이 일반적입니다.

#### 2. `services`
- 각 서비스는 독립적인 컨테이너로 실행됩니다.
- **서비스 정의 요소**:

    - **image**: 컨테이너에 사용할 Docker 이미지를 지정합니다. 이미지를 빌드하지 않고 직접 가져오는 경우 사용합니다.
    - **build**: Dockerfile을 사용해 이미지를 빌드할 경로를 지정합니다.
    - **command**: 컨테이너 시작 시 실행할 명령어를 지정합니다.
    - **ports**: 호스트와 컨테이너 간의 포트 매핑을 설정합니다. `호스트 포트:컨테이너 포트` 형식으로 지정합니다.
    - **networks**: 이 서비스가 연결될 네트워크를 지정합니다.
    - **volumes**: 호스트 또는 다른 컨테이너와 데이터를 공유하기 위한 볼륨을 설정합니다. `볼륨 이름:컨테이너 내 경로` 형식으로 지정합니다.
    - **environment**: 환경 변수를 설정합니다. 여기서는 `APP_ENV`와 `DATABASE_URI`와 같은 설정을 컨테이너에 전달합니다.

#### 3. `networks`
- 여러 서비스가 연결될 네트워크를 정의합니다.
- 네트워크 이름을 설정하여 서비스 간에 격리된 네트워크를 생성할 수 있습니다. 위 예시에서는 `app-network`라는 네트워크를 생성하여 `web`과 `db` 서비스가 서로 통신할 수 있도록 설정합니다.

#### 4. `volumes`
- 데이터를 영구적으로 저장하거나, 컨테이너 간 데이터를 공유할 수 있는 볼륨을 정의합니다.
- 예시에서는 `app-volume`과 `db-volume`을 정의하여 각각 `web`과 `db` 서비스에 마운트했습니다. 이렇게 설정된 볼륨은 컨테이너가 삭제되어도 데이터가 유지됩니다.

---

### Docker Compose 파일의 주요 명령어

1. **docker compose up**: Compose 파일을 읽어 모든 서비스를 실행합니다.
   ```bash
   docker compose up -d
   ```
   `-d` 옵션을 추가하면 백그라운드에서 실행됩니다.

2. **docker-compose down**: 실행 중인 모든 서비스를 중지하고, 생성된 네트워크와 컨테이너를 삭제합니다.
   ```bash
   docker-compose down
   ```

3. **docker-compose ps**: 현재 실행 중인 모든 서비스 상태를 확인합니다.
   ```bash
   docker-compose ps
   ```

4. **docker-compose logs**: 각 서비스의 로그를 확인할 수 있습니다.
   ```bash
   docker-compose logs
   ```

5. **docker-compose restart**: 실행 중인 서비스를 재시작합니다.
   ```bash
   docker-compose restart
   ```

---

이렇게 Docker Compose 파일을 통해 멀티 컨테이너 애플리케이션을 효율적으로 관리할 수 있으며, 각 서비스 간의 네트워크 및 데이터 공유 설정을 쉽게 구성할 수 있습니다. 이는 개발 환경에서 특히 유용하며, 코드 수정, 테스트 및 배포 작업을 보다 간편하게 만들어줍니다.