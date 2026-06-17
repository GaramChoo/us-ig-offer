# US IG Offer Axe — Daily Bond Database

> Citi US Investment Grade Offer Axe 데이터를 날짜별로 누적·조회하는 브라우저 기반 도구

## 개요

매일 수신하는 Citi US IG Offer Axe 데이터를 복사+붙여넣기로 적재하고,
발행자·등급·만기·스프레드 기준으로 조회할 수 있는 단일 HTML 앱입니다.
별도 서버나 설치 없이 브라우저만으로 동작하며, 데이터는 브라우저 localStorage에 저장됩니다.

## 주요 기능

### 데이터 입력
- 메일 원문을 그대로 복사+붙여넣기 (헤더 포함/미포함 모두 허용)
- 파싱 미리보기 후 저장 (날짜 중복 시 덮어쓰기)
- Security 항목 자동 파싱: 발행자명 / 쿠폰(소수·분수 모두 인식) / 만기일 / Float 여부

### 전체 조회
| 필터 | 설명 |
|------|------|
| 텍스트 검색 | 종목명·발행자·ISIN·업종 |
| Industry | 섹터별 필터 |
| Rank | Sr Unsecured / Sr Preferred / Sr Non Preferred |
| Moody's / S&P | 체크박스 방식, 등급 순서 정렬, "이상 선택" 기능 |
| Fixed / Float | 고정금리·변동금리 구분 |
| 잔존만기 | 최소~최대 년수 범위 지정 |

- 특정 날짜 선택 시: 해당일 종목 수 / 발행자 수 / 평균 YTC / 평균 Spread 요약 표시
- 신용등급 정렬: Moody's(Aaa→C), S&P(AAA→D) 순서 적용
- JSON 내보내기: 전체 DB를 `.json`으로 저장 (Claude 등 AI 분석용)

### 발행자 추이
- 발행자별 등장 일수·ISIN 수·평균 YTC/Spread 집계
- "꾸준히 거래되는 종목" 파악에 활용

## 데이터 구조

입력 형식 (탭 구분, 9열):

```
Size(K)  Security              Spread  YTC   Moody's  S&P  ISIN          Industry  Rank
11950    BMO 5.266 12/11/26    20      3.91  A2       A-   US06368LC537  Banks     Sr Unsecured
```

저장 형식 (localStorage, key: `us_ig_axe_db_v1`):
```json
[
  {
    "date": "2026-06-05",
    "src": "Citi",
    "sec": "BMO 5.266 12/11/26",
    "issuer": "BMO",
    "coupon": 5.266,
    "isFloat": false,
    "maturityStr": "2026-12-11",
    "size": 11950,
    "spread": 20,
    "ytc": 3.91,
    "moodys": "A2",
    "moodysRank": 5,
    "sp": "A-",
    "spRank": 6,
    "isin": "US06368LC537",
    "industry": "Banks",
    "rank": "Sr Unsecured"
  }
]
```

## 사용 방법

### 매일 데이터 입력 (약 30초)
1. 메일에서 데이터 전체 선택·복사
2. **데이터 입력** 탭 → 날짜 확인 → 붙여넣기
3. **미리보기** → 내용 확인 → **저장**

### 데이터 분석
1. **전체 조회** 탭에서 필터 조합
2. **JSON 내보내기** → Claude에 업로드하여 심층 분석 가능

## 기술 스택

- Vanilla HTML / CSS / JavaScript (외부 라이브러리 없음)
- 브라우저 localStorage (서버 불필요)
- GitHub Pages 정적 호스팅

## 주의사항

- 데이터는 **해당 브라우저에만** 저장됩니다 (다른 브라우저·기기 간 공유 불가)
- 브라우저 캐시 전체 삭제 시 데이터가 삭제될 수 있습니다
- 정기적으로 JSON 내보내기를 통해 백업을 권장합니다
