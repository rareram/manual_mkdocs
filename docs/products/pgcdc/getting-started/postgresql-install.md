# PostgreSQL 설치

개발 환경에 PostgreSQL을 설치합니다. 사용 중인 운영체제에 맞는 탭을 선택하여 진행하세요.

=== "macOS (Homebrew)"

    Homebrew 패키지 관리자를 사용하여 PostgreSQL을 설치하고 실행합니다.

    ```bash
    # PostgreSQL 설치
    brew install postgresql

    # PostgreSQL 서비스 시작
    brew services start postgresql
    ```

    설치가 완료되면 다음 명령어로 PostgreSQL에 접속할 수 있습니다.
    ```bash
    # 'your_username' 부분은 실제 macOS 사용자 이름으로 변경하세요.
    psql -U your_username -d postgres
    ```

=== "Windows"

    Windows용 PostgreSQL은 공식 웹사이트에서 다운로드하여 설치하는 것을 권장합니다.

    1.  [PostgreSQL 공식 다운로드 페이지](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)로 이동하여 Windows 버전 설치 관리자를 다운로드합니다.
    2.  설치 관리자를 실행하고, 화면의 안내에 따라 설치를 진행합니다. 설치 과정에서 `superuser` (`postgres`)의 비밀번호를 설정하게 됩니다.
    3.  설치 시 **Stack Builder** 옵션은 선택 해제해도 무방합니다.
    4.  설치가 완료되면, 시작 메뉴에서 `SQL Shell (psql)`을 실행하여 PostgreSQL에 접속할 수 있습니다.

    !!! note "pgAdmin 4"
        Windows 설치 관리자는 `pgAdmin`이라는 GUI 도구를 함께 설치합니다. 이 도구를 사용하면 그래픽 환경에서 데이터베이스를 편리하게 관리하고 데이터를 시각적으로 확인할 수 있습니다.

=== "Ubuntu"

    `apt` 패키지 관리자를 사용하여 PostgreSQL을 설치합니다.

    ```bash
    # 패키지 목록 업데이트
    sudo apt update

    # PostgreSQL 및 관련 유틸리티 설치
    sudo apt install postgresql postgresql-contrib
    ```

    설치 후에는 `postgres` 사용자로 자동 생성된 계정을 통해 다음과 같이 접속할 수 있습니다.

    ```bash
    sudo -i -u postgres
    psql
    ```
