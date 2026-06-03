# PE_weather (노라죠) 🌤️

체육 교사를 위한 날씨 대시보드 앱입니다. 오늘의 날씨, 주간 예보, 미세먼지, 자외선 지수, 야외활동 권고 정보를 한눈에 확인할 수 있습니다.

---

## 주요 기능

| 기능 | 설명 |
|------|------|
| 🌡️ 오늘의 날씨 | 현재 기온, 체감온도, 강수확률, 하늘 상태 |
| 📅 주간 예보 | 7일간 최고/최저기온, 날씨 아이콘, 강수확률 |
| 💨 미세먼지 | PM2.5 / PM10 등급 및 수치 (에어코리아) |
| ☀️ 생활기상지수 | 자외선, 불쾌지수, 체감온도 등 |
| 🏃 야외활동 권고 | 날씨 종합 판단 — 활동 권고/주의/금지 |
| 📚 환경교육 자료 | 날씨 관련 체육 수업 활용 정보 |
| 📍 위치 선택 | 자동 GPS 감지 + 전국 지역 수동 검색 |

---

## 사용 API

| API | 출처 | 용도 |
|-----|------|------|
| 기상청 단기예보 (v2.0) | 공공데이터포털 | 3일 날씨 예보 |
| 기상청 중기예보 | 공공데이터포털 | 4~7일 날씨 예보 |
| 기상청 생활기상지수 (v4) | 공공데이터포털 | 자외선·불쾌지수 등 |
| 에어코리아 대기오염정보 | 공공데이터포털 | 미세먼지 PM2.5/PM10 |
| 카카오 Local API | Kakao Developers | 주소 검색 및 좌표 변환 |

### API 키 발급 방법

#### 1. 공공데이터포털 (기상청 + 에어코리아)
1. [data.go.kr](https://www.data.go.kr) 회원가입 및 로그인
2. 아래 API를 각각 검색하여 활용 신청:
   - `기상청_단기예보 ((구)_동네예보) 조회서비스`
   - `기상청_중기예보 조회서비스`
   - `기상청_생활기상지수 조회서비스`
   - `한국환경공단_에어코리아_대기오염정보`
3. 마이페이지 → 오픈API → 인증키 확인 (KMA/AIR 동일 키 사용 가능)

#### 2. 카카오 Developers (지역 검색)
1. [developers.kakao.com](https://developers.kakao.com) 접속 → 로그인
2. 내 애플리케이션 → 애플리케이션 추가
3. **앱 키** 탭에서 **REST API 키** 복사
4. 플랫폼 → Web → 사이트 도메인 등록 (로컬 테스트: `http://localhost:8000`)

---

## 설치 및 실행

### 로컬 실행

```bash
# 저장소 클론
git clone https://github.com/youseungoh/PE_weather.git
cd PE_weather

# 로컬 서버 실행 (file:// 프로토콜은 CORS 오류 발생)
python -m http.server 8000
# 또는 VS Code Live Server 사용
```

브라우저에서 `http://localhost:8000` 접속

### API 키 설정

`index.html` 파일 내 아래 상수를 본인 키로 교체:

```javascript
const API_KEYS = {
  kma:   '공공데이터포털_인증키',
  air:   '공공데이터포털_인증키',
  kakao: '카카오_REST_API_키',
};
```

---

## GitHub Pages 배포

```bash
git add .
git commit -m "deploy PE_weather"
git push origin main
```

GitHub 저장소 → Settings → Pages → Source: `main` 브랜치 선택 → Save

배포 주소: `https://<username>.github.io/PE_weather`

> **주의**: GitHub Pages 도메인을 카카오 Developers 플랫폼 → Web → 사이트 도메인에 추가해야 카카오 API가 정상 작동합니다.

---

## 파일 구조

```
PE_weather/
└── index.html   # 단일 파일 앱 (HTML + CSS + JavaScript)
```

모든 로직, 스타일, 마크업이 `index.html` 하나에 포함되어 있습니다.

---

## 기술 스택

- **언어**: Vanilla JavaScript (프레임워크 없음)
- **UI**: Glassmorphism 다크 테마, CSS Grid/Flexbox
- **아이콘**: [Lucide Icons](https://lucide.dev)
- **폰트**: Google Fonts — Noto Sans KR, DM Sans
- **CORS 프록시**: [api.allorigins.win](https://api.allorigins.win) (공공데이터포털 우회)
- **좌표 변환**: Lambert Conformal Conic (WGS84 → 기상청 NX/NY 격자)

---

## 주요 함수

| 함수 | 역할 |
|------|------|
| `fetchRealData()` | 전체 API 병렬 호출 (Promise.allSettled) |
| `parseKmaForecast()` | 단기예보 파싱 — TMX/TMN, 주간 최악 날씨 |
| `fetchMidForecast()` | 중기예보 파싱 (4~7일) |
| `renderAll(data)` | 전체 UI 렌더링 |
| `renderWeekly(data)` | 주간 예보 카드 렌더링 |
| `calcVerdict(data)` | 야외활동 권고 판단 로직 |
| `openLocationPicker()` | 위치 선택 모달 열기 |
| `doKakaoSearch()` | 카카오 API로 지역 검색 |
| `lcc()` | 위경도 → 기상청 격자 좌표 변환 |

---

## 크레딧

© Produced by You Seungoh · [yso21@naver.com](mailto:yso21@naver.com)  
BGM: [youtube.com/@pianocanvas](https://youtube.com/@pianocanvas) | Cafe Music
