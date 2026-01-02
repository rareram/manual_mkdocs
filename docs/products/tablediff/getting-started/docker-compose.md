# Docker Compose를 이용한 환경 구성

TableDiff의 기능 테스트를 위해서는 데이터 소스(Source)와 목적지(Destination) 역할을 하는 2개의 Oracle 데이터베이스가 필요합니다. 이 문서에서는 Docker Compose를 사용하여 Oracle 19c, 21c, 23ai 등 다양한 버전의 테스트 환경을 한 번에 구성하는 방법을 안내합니다.

이 Docker Compose 설정은 다음과 같은 특징을 가집니다.

* **다중 버전 지원**: 21c와 23ai 버전을 동시에 운영하여 버전 간 비교 테스트를 수행할 수 있습니다.
* **데이터 영속성**: Docker 볼륨을 사용하여 컨테이너가 삭제되어도 데이터가 유지됩니다.
* **자동 초기화**: 컨테이너 시작 시 `initdb.d` 폴더의 스크립트를 실행하여 사용자와 테이블을 자동으로 생성합니다.

---

## 빠른 시작

1.  **Docker Compose 파일 준비**
    아래 `compose.yml` 내용을 파일로 저장합니다.

2.  **디렉토리 구조 생성**
    `compose.yml` 파일이 있는 곳에 아래와 같은 디렉토리를 생성합니다. 초기화 스크립트나 CSV 파일이 있다면 각 위치에 배치합니다.
    ```
    .
    ├── compose.yml
    ├── 21c/
    │   ├── src-init/
    │   └── dst-init/
    ├── 23ai/
    │   ├── src-init/
    │   └── dst-init/
    └── volumes/
    ```

3.  **컨테이너 실행**
    터미널에서 아래 명령어를 실행하여 모든 데이터베이스 컨테이너를 시작합니다.
    ```bash
    docker-compose up -d
    ```
    데이터베이스가 완전히 시작되기까지 약 2~5분 정도 소요될 수 있습니다.

---

## compose.yml

아래 `compose.yml` 내용을 그대로 복사해서 사용합니다.

```yaml
# compose.yml
services:
  # ============ 21c (19c 대체) - SOURCE ============
  orcl_21c_src:
    image: gvenzl/oracle-xe:21-slim
    container_name: orcl_21c_src
    hostname: orcl21src
    restart: unless-stopped
    environment:
      ORACLE_PASSWORD: cdctest
      APP_USER: cdctest
      APP_USER_PASSWORD: cdctest
    ports:
      - "1921:1521" # 21c 소스 DB 포트
    volumes:
      - ./21c/src-init:/container-entrypoint-initdb.d
      - ./volumes/21c-src-data:/opt/oracle/oradata
    healthcheck:
      test: ["CMD", "healthcheck.sh"]
      interval: 30s
      timeout: 10s
      retries: 10
      start_period: 120s
    networks: [oracle-net]

  # ============ 21c (19c 대체) - DESTINATION ============
  orcl_21c_dst:
    image: gvenzl/oracle-xe:21-slim
    container_name: orcl_21c_dst
    hostname: orcl21dst
    restart: unless-stopped
    environment:
      ORACLE_PASSWORD: cdctest
      APP_USER: cdctest
      APP_USER_PASSWORD: cdctest
    ports:
      - "1922:1521" # 21c 목적지 DB 포트
    volumes:
      - ./21c/dst-init:/container-entrypoint-initdb.d
      - ./volumes/21c-dst-data:/opt/oracle/oradata
    healthcheck:
      test: ["CMD", "healthcheck.sh"]
      interval: 30s
      timeout: 10s
      retries: 10
      start_period: 120s
    networks: [oracle-net]

  # ============ 23ai Free - SOURCE ============
  orcl_23ai_src:
    image: gvenzl/oracle-free:latest
    container_name: orcl_23ai_src
    hostname: orcl23src
    restart: unless-stopped
    environment:
      ORACLE_PASSWORD: cdctest
      APP_USER: cdctest
      APP_USER_PASSWORD: cdctest
    ports:
      - "2321:1521" # 23ai 소스 DB 포트
    volumes:
      - ./23ai/src-init:/container-entrypoint-initdb.d
      - ./volumes/23ai-src-data:/opt/oracle/oradata
    healthcheck:
      test: ["CMD", "healthcheck.sh"]
      interval: 30s
      timeout: 10s
      retries: 10
      start_period: 120s
    networks: [oracle-net]

  # ============ 23ai Free - DESTINATION ============
  orcl_23ai_dst:
    image: gvenzl/oracle-free:latest
    container_name: orcl_23ai_dst
    hostname: orcl23dst
    restart: unless-stopped
    environment:
      ORACLE_PASSWORD: cdctest
      APP_USER: cdctest
      APP_USER_PASSWORD: cdctest
    ports:
      - "2322:1521" # 23ai 목적지 DB 포트
    volumes:
      - ./23ai/dst-init:/container-entrypoint-initdb.d
      - ./volumes/23ai-dst-data:/opt/oracle/oradata
    healthcheck:
      test: ["CMD", "healthcheck.sh"]
      interval: 30s
      timeout: 10s
      retries: 10
      start_period: 120s
    networks: [oracle-net]

networks:
  oracle-net:
    driver: bridge
```

---

## 연결 정보

`TableDiff`의 설정 파일(`tablediff.conf`) 작성 시 아래 정보를 참고하여 `jdbcUrl`을 구성합니다.

| DB 버전 | 구분 | 호스트 포트 | 컨테이너 이름 | PDB 이름 | JDBC URL (예시) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **21c** | 소스 | `1921` | `orcl_21c_src` | `XEPDB1` | `jdbc:oracle:thin:@//localhost:1921/XEPDB1` |
| **21c** | 목적지 | `1922` | `orcl_21c_dst` | `XEPDB1` | `jdbc:oracle:thin:@//localhost:1922/XEPDB1` |
| **23ai** | 소스 | `2321` | `orcl_23ai_src` | `FREEPDB1` | `jdbc:oracle:thin:@//localhost:2321/FREEPDB1` |
| **23ai**| 목적지 | `2322` | `orcl_23ai_dst` | `FREEPDB1` | `jdbc:oracle:thin:@//localhost:2322/FREEPDB1` |

- **사용자 계정**: `cdctest`
- **비밀번호**: `cdctest`

이 정보는 `compose.yml` 파일의 `environment` 섹션에 정의되어 있습니다.
