# 한미 증시 + 거시경제 대시보드

실시간 한미 증시 지수, 10년물 국채 수익률, 환율, 유가, 두바이유 동향을 한눈에 보여주는 단일 페이지 대시보드입니다. 모바일에서도 작동합니다.

## 데이터 출처

- **Yahoo Finance v8 chart API** (CORS 프록시 경유): 주가/환율/유가/미국 10년물 yield 시계열
- **정적 메모**: 6개국 10년물 yield 현재값 (Trading Economics 2026-05-01), 미·한 기준금리, 두바이유 12개월 평균
- 차트 토글: 1M / 1Y / 5Y / 10Y

## GitHub Pages 배포 절차 (5분)

### 1. GitHub 저장소 생성

1. [github.com](https://github.com) 로그인
2. 우상단 `+` → `New repository`
3. 저장소 이름: `stock-dashboard` (자유)
4. Public 선택, `Add a README file` **체크 안 함**
5. `Create repository` 클릭

### 2. index.html 업로드

1. 빈 저장소 페이지에서 `uploading an existing file` 링크 클릭
2. 이 폴더(`Cowork_EconomicStatus`)의 `index.html`을 드래그
3. 하단에 `Commit changes` 버튼 클릭

### 3. Pages 활성화

1. 저장소 상단 `Settings` 탭
2. 좌측 메뉴 `Pages`
3. `Source`를 `Deploy from a branch`로 선택
4. `Branch`를 `main` / `(root)`로 선택 → `Save`
5. 1~2분 후 페이지 새로고침하면 상단에 URL 표시:
   `https://<사용자명>.github.io/stock-dashboard/`

### 4. 휴대폰 즐겨찾기

- iOS Safari: 공유 버튼 → "홈 화면에 추가"
- Android Chrome: 메뉴 → "홈 화면에 추가"

## 작동 방식

페이지를 열면 자동으로 Yahoo Finance API에 다음 순서로 요청합니다:

1. `corsproxy.io` 시도
2. 실패 시 `api.allorigins.win`
3. 실패 시 `api.codetabs.com`
4. 모두 실패하면 `localStorage`에 저장된 마지막 데이터 표시

차트 데이터는 자동으로 `localStorage`에 캐시되어, 다음 방문 시 빠르게 로드되고 네트워크 오류 시에도 화면이 비지 않습니다.

## 갱신 주기

- 미국 시장 데이터: Yahoo Finance가 약 15분 지연된 데이터 제공
- 한국 시장 데이터: 마찬가지로 약 15~20분 지연
- 환율: 거의 실시간
- 유가: 약 10분 지연
- 두바이유: 월간 평균 (코드 내 인라인 데이터)

## 업데이트 시 주의

`index.html`을 수정해서 다시 업로드하려면:
- GitHub 저장소에서 `index.html` 클릭 → 연필 아이콘 → 내용 교체 → `Commit`
- 또는 새 파일을 드래그해서 덮어쓰기

푸시 후 1~2분이면 사이트에 반영됩니다.

## 보안/프라이버시

- 공개 URL입니다 (URL을 아는 사람은 누구나 접근 가능). 검색엔진에는 노출되지 않도록 `robots.txt`를 추가하려면 저장소에 `robots.txt` 파일을 만들어 다음을 넣으면 됩니다:
  ```
  User-agent: *
  Disallow: /
  ```
- CORS 프록시는 제3자 서비스이므로 시세 데이터(공개 정보)만 통과시키고 개인정보는 전송하지 않음.

## 문제 해결

- **차트가 비어있음**: 브라우저 콘솔(F12)에서 `[hist]` 경고 확인. 모든 프록시가 다운된 경우라 잠시 후 새로고침.
- **첫 로드가 느림**: CORS 프록시 응답 대기 중. 9초 timeout 후 다음 프록시 시도.
- **5Y / 10Y 데이터 없음**: 일부 ticker(특히 KOSDAQ)는 Yahoo가 10Y 데이터를 제공하지 않을 수 있음. 그 경우 5Y로 토글.

## 출처

- Yahoo Finance — query1.finance.yahoo.com/v8/finance/chart
- Trading Economics — tradingeconomics.com/bonds
- Federal Reserve — federalreserve.gov/monetarypolicy/openmarket.htm
- Bank of Korea — bok.or.kr
- FRED POILDUBUSDM — fred.stlouisfed.org

---

본 대시보드는 자동화 도구로 생성된 시장 동향 정리이며 투자 자문이 아닙니다.
