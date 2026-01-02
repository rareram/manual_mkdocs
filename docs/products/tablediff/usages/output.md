# 결과 확인 및 ApplyTo 테스트

이 문서는 Compare 결과를 확인하고, ApplyTo 기능을 이용해 후처리를 테스트하는 방법을 설명합니다.

---

## 결과 타입

Compare 결과는 다음 타입 중 하나로 출력됩니다.
- `Same`
- `Change`
- `OnlyInA`
- `OnlyInB`
- `Ext` (대용량 컬럼 분리 출력)

---

## 결과 건수 확인

```bash
grep '"type":"Same"'    compare.ndjson | wc -l
grep '"type":"Change"' compare.ndjson | wc -l
grep '"type":"OnlyInA"' compare.ndjson | wc -l
grep '"type":"OnlyInB"' compare.ndjson | wc -l
```

### Change 결과만 확인
```bash
grep '"type":"Change"' compare.ndjson | head
```

---

## ApplyTo 테스트 (Mock 모드 권장)

실제 DB 반영 전, 반드시 `mock` 모드로 테스트하십시오.

```hocon
Change = {
    use.db = "mock"
    action = "update"
    batch = 10

    cols = {
        C_VAL1 = "rowA.cols.4"
        C_VAL2 = "rowA.cols.5"
    }

    where = {
        S_KEY1 = "rowB.keys.1"
    }
}
```

### ApplyTo 실행 (파일 입력)
```bash
java -jar TableDiff_0.4.jar \
    -c conf/tablediff.conf \
    -i json \
    -f compare.ndjson
```

### ApplyTo 실행 (파이프 입력)
```bash
cat compare.ndjson | \
java -jar TableDiff_0.4.jar -c conf/tablediff.conf -i json
```
> `mock` 모드에서는 실제 DB 변경 없이 실행 로그만 출력됩니다.
