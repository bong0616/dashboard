# 아이샵케어 · 설치 운영 대시보드

Google Sheet(26년 미설치 리스트) 데이터를 집계해 보여주는 사내 열람용 대시보드입니다.
Vercel 정적 호스팅 + Google Apps Script(GAS) 웹앱 JSON API 구조.

## 구조

```
Google Sheet (로우데이터/콜리스트/실적로그)
   └─ GAS 웹앱  대시보드API.js  doGet(?key=코드)  → 집계 JSON (JSONP)
        └─ index.html (Vercel)  게이트 통과 후 fetch → Chart.js 렌더
```

- **운영 대시보드**: 콜리스트 기준 현재 잔여 업무량(미설치/설치예정/부재/오늘 컨택가능/미설치 사유)
- **성과 대시보드**: 설치완료율·설치소요일·5일이내 설치율·자가/상담원 설치율·재컨택주기↔속도·7일+지연사유·일/월 처리량
- **출고 코호트**: 출고월·주별 설치 진행률, 채널 구성, 미설치 잔여 경과일, 출고월별 현재 상태

## 접근 코드(passcode)

- 소스에는 저장하지 않습니다. 사용자가 입력한 코드를 GAS의 `DASH_PASSCODE` 와 대조합니다.
- **코드 변경**: 앱스크립트 프로젝트의 `대시보드API.js` 안 `DASH_PASSCODE` 한 곳만 수정 → `clasp push` 후 `clasp deploy -i <배포ID>` 로 재배포.

## 데이터/API 갱신

- 데이터는 GAS가 매 요청 시 시트를 읽어 집계하므로 **항상 최신**입니다(대시보드 우측하단 "새로고침").
- API 로직 수정 시: `대시보드API.js` 수정 → `clasp push` → `clasp deploy -i <배포ID>`.
- API 엔드포인트는 `index.html` 상단 `API_BASE` 에 있습니다.

## 배포

- 이 폴더를 GitHub `dashboard` 레포에 두고 Vercel에 연결하면, `main` push 시 자동 배포됩니다.
- 정적 파일 하나(`index.html`)라 별도 빌드 설정이 필요 없습니다(Vercel Zero-config).
