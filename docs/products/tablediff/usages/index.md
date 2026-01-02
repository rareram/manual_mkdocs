# TableDiff 기능 테스트 개요

이 문서는 **TableDiff(ADV)를 이용해 실제 데이터 비교 기능을 검증**하기 위한 기능 테스트 가이드입니다.

TableDiff는 단순히 “테이블 A/B 비교 도구”가 아니라,
SQL을 통해 생성된 두 개의 ResultSet을 **정렬된 상태로 스트리밍 비교**하여
다음 네 가지 결과로 분류합니다.

- `Same`
- `Change`
- `OnlyInA`
- `OnlyInB`

본 문서는 다음 범위를 다룹니다.

- 기능 테스트를 위한 설정 파일(HOCON) 작성
- CLI 기반 비교 실행 방법
- 비교 결과 확인 방법
- ApplyTo(후처리) 테스트 방법

아래 항목은 **별도 문서를 참고**합니다.

- [JVM 설치 및 옵션](../../../install-guide/jvm/installation.md)
- [Oracle DB 설치](../../../install-guide/oracle/index.md)
- 테스트용 데이터 생성 SQL

---

## 테스트 전 체크리스트

기능 테스트를 시작하기 전에 아래 항목을 반드시 확인하십시오.

- [JVM이 설치되어 있고](../../../install-guide/jvm/installation.md) `java` 명령이 실행 가능해야 합니다.
- [비교 대상 DB 2개(DB_A / DB_B)](../../../install-guide/oracle/index.md)에 접속 가능해야 합니다.
- 두 SQL의 SELECT 컬럼 개수와 순서가 동일해야 합니다.
- 두 SQL의 ORDER BY 조건이 완전히 동일해야 합니다.
- SortKey는 ResultSet 내에서 유일해야 합니다.

---

## 문서 구성

기능 테스트 문서는 다음 네 부분으로 구성됩니다.

1.  기능 테스트 개요
2.  설정 파일 작성
3.  CLI 실행 방법
4.  결과 확인 및 ApplyTo 테스트

