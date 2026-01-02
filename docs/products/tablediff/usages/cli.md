# CLI 실행 방법

TableDiff는 CLI 기반으로 실행되며, 비교 결과는 표준 출력(stdout)으로 출력됩니다.

---

## 기본 실행 형식

```bash
java -jar TableDiff_0.4.jar [options]
```

### 주요 옵션
- `-c, --conf <file>`: 설정 파일 경로 (필수)
- `-o, --out <format>`: `compare` 결과 출력 형식 (json | bin)
- `-i, --in <format>`: `applyTo` 입력 형식 (json | bin)
- `-f, --from <file>`: `applyTo` 입력 파일 (미지정 시 stdin)
> `-o` 와 `-i` 옵션은 동시에 사용할 수 없습니다.

### Compare 실행
```bash
java -jar TableDiff_0.4.jar \
    -c conf/tablediff.conf \
    -o json > compare.ndjson
```
> NDJson 형식으로 한 줄당 하나의 결과가 출력됩니다. 파일 저장은 shell redirect 사용을 권장합니다.

### 샘플 모드 실행
개발/테스트용 내장 샘플 데이터를 사용할 수 있습니다.
```bash
java -jar TableDiff_0.4.jar -c _sample -o json
```
