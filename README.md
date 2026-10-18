# 서울 고교 지도 만들기 — 진로교사 연수 안내

진로진학상담교사 연수에서 쓰는 실습 안내 페이지입니다. 학교알리미 공시자료를 Leaflet + OpenStreetMap 지도 위에 올린 「서울 고교 지도」 웹앱을 만드는 과정을 네 단계로 나누어 담았습니다. 지도에는 API 키나 카드 등록이 필요 없고, 참가자가 넣는 키는 `SCHOOLINFO_API_KEY` 하나입니다.

## 구성

1. Google AI 스튜디오의 Build 기능 열기
2. 준비된 프롬프트를 그대로 붙여넣기 (프롬프트 원문 포함, 줄 수는 페이지가 원문에서 직접 셈, 복사 버튼 제공)
3. 학교알리미 API 키 발급과 Secrets 저장
4. GitHub에 저장하고 Netlify로 배포하기 (AI 스튜디오 GitHub 연동 → Netlify 저장소 연결, 서버 함수와 `SCHOOLINFO_API_KEY` 환경 변수 포함)

각 단계는 체크하며 따라갈 수 있고, 인쇄하면 단계별로 한 장씩 나옵니다.

## 파일

- `index.html` — 안내 페이지 전체. 2단계 복사용 프롬프트 원문도 이 파일의 `#promptText` 안에 들어 있습니다(별도 원본 파일 없음). 외부 의존성은 글꼴(Pretendard) 하나뿐입니다.
- `images/` — 학교알리미 OpenAPI 화면 캡처(`schoolinfo-*`), AI 스튜디오 시작·프로젝트 화면(`aistudio-01`~`02`), AI 스튜디오 GitHub 연동 화면(`github-01`~`08`), Netlify 배포 화면(`netlify-01`~`09`). AI 스튜디오·GitHub·Netlify 캡처는 지진 지도 연수(earthquake.labbitory.com)의 실습 화면을 그대로 가져왔습니다.
- `data/seoul-gu.geojson` — 서울 25개 자치구 경계(`kind: "district"`, `sggCode`·`eduOffice` 포함), 11개 교육지원청 외곽선(`kind: "eduOffice"`, 안쪽 경계 제거), 서울 외곽선(`kind: "seoulOutline"`). 생성될 앱이 자기 저장소의 `public/data/`에서 먼저 찾고, 없으면 이 사이트의 파일을 불러옵니다. 안내 페이지 4단계(선택 항목)에서 내려받을 수 있습니다.
- `_headers` — Cloudflare Pages(또는 Netlify)에서 `/data/*`에 CORS 허용 헤더를 붙입니다. 생성된 앱이 자기 경계 파일을 못 찾을 때 이 사이트의 파일을 대신 불러오기 위해서입니다.

## 경계 데이터 출처

- 원본: [southkorea/seoul-maps](https://github.com/southkorea/seoul-maps) `kostat/2013/json/seoul_municipalities_geo_simple.json`
- 원자료: 통계청(KOSTAT) 센서스용 행정구역경계 2013, 단순화본
- 라이선스: Apache License 2.0 (원본 저장소 README 표기)
- 가공: 좌표를 소수점 6자리로 반올림하고, 인접 구가 공유하는 변을 지워 교육지원청·서울 외곽선을 만들었습니다. 좌표는 GeoJSON 표준인 `[경도, 위도]` 순서입니다. 화면 표시용 근사선입니다.

## 배포

정적 페이지이므로 별도 빌드 과정이 없습니다. Cloudflare Pages나 Netlify에 이 저장소를 연결하면, 빌드 명령은 비워 두고 출력 폴더를 저장소 최상위(`/`)로 지정하면 됩니다.
