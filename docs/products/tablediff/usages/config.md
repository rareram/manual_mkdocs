# 기능 테스트용 설정 파일 작성

TableDiff는 **HOCON 형식의 설정 파일**을 사용하여 비교 대상 DB, SQL, 비교 조건을 정의합니다.

---

## 설정 파일 기본 구조

설정 파일은 다음 네 영역으로 구성됩니다.

* `TableA`: DB_A 접속 정보 및 SQL
* `TableB`: DB_B 접속 정보 및 SQL
* `compare.sortKey`: 정렬키 조건
* `compare.compCols`: 값 비교 대상 컬럼

---

## TableA / TableB 설정 예시

아래는 기능 테스트에 사용할 수 있는 최소 설정 예시입니다.

```hocon
TableA {
    driver  = "oracle.jdbc.OracleDriver"
    jdbcUrl = "jdbc:oracle:thin:@//192.168.0.78:1521/freepdb1"
    username = "cdctest"
    password = "cdctest"

    sql = """
        SELECT
            S_KEY1,
            S_KEY2,
            S_KEY3,
            C_VAL1,
            C_VAL2
        FROM TB_DIFF_SAMPLE
        ORDER BY
            S_KEY1 DESC NULLS FIRST,
            S_KEY2 ASC  NULLS FIRST,
            S_KEY3 DESC
    """
}

TableB {
    driver  = "oracle.jdbc.OracleDriver"
    jdbcUrl = "jdbc:oracle:thin:@//192.168.0.78:1522/freepdb1"
    username = "cdctest"
    password = "cdctest"

    sql = """
        SELECT
            S_KEY1,
            S_KEY2,
            S_KEY3,
            C_VAL1,
            C_VAL2
        FROM TB_DIFF_SAMPLE
        ORDER BY
            S_KEY1 DESC NULLS FIRST,
            S_KEY2 ASC  NULLS FIRST,
            S_KEY3 DESC
    """
}
```

## compare 설정

`SortKey`는 `ORDER BY`에 사용된 컬럼과 완전히 동일한 순서와 조건을 가져야 합니다.

```hocon
compare {
    sortKey = [
        { colA: 1, colB: 1, ascending: false, nullAsSmallest: true  },
        { colA: 2, colB: 2, ascending: true,  nullAsSmallest: true  },
        { colA: 3, colB: 3, ascending: false, nullAsSmallest: false }
    ]

    compCols = [
        { colA: 4, colB: 4 },
        { colA: 5, colB: 5, tolerance: { delta: 0.01 } }
    ]
}
```

* `colA` / `colB`: `SELECT` 결과에서의 컬럼 위치 (1부터 시작)
* `ascending`: 정렬 방향
* `nullAsSmallest`: `NULL` 정렬 우선순위
* `tolerance`: 실수형, 시각형 컬럼에만 적용됩니다.
