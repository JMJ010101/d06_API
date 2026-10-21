# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 이 폴더는 무엇인가

원티드 AI 강의 6일차(D6) 실습 폴더입니다. 빌드·린트·테스트 도구가 없고 git 저장소도 아닙니다(`package.json` 없음). 결과물은 **API를 부르는 단일 HTML 파일**이고, 확인은 브라우저에서 더블클릭으로 엽니다.

- `d6-api/` — 실습 결과물: `weather.html`(A), `rate.html`(B), `city.html`(C), `morning.html`(D)
- `D06_1021.html` — 수업 워크북(참고용, 수정 대상 아님)
- `.claude/rules/api.md` — **API 화면 규칙. HTML을 만들거나 고치기 전에 읽는다.** 규칙을 여기서 반복하지 말고 이 파일을 갱신한다. "실수에서 배운 규칙"과 "추가" 절에 AI가 자주 하는 실수가 쌓인다.
- `.claude/skills/api-check` — `/api-check @d6-api\파일.html`. 고치지 않고 규칙 준수 여부만 표로 검사한다.

## 작업 방식

- 새 화면은 `d6-api/` 아래에 HTML 한 파일(CSS·JS 인라인, 외부 빌드 없음)로 만든다.
- 커밋·푸시는 하지 않고 커밋 메시지만 제안한다. 형식: `"[직군] D6: <A~D> <제목>"`.
- 키가 필요한 API는 만들지 않고 "이 API는 키가 필요해서 D8 방식이 필요해요"라고 알린다.

## 기존 화면의 공통 구조

네 파일이 같은 뼈대를 따르므로 새 화면도 맞춘다.

- `const API = {…}` 한 곳에 주소·좌표·통화를 모으고, 화면 맨 아래 "🔌 이 화면이 쓰는 API" 상자가 **실제로 부른 URL**을 그대로 보여 준다(주소를 바꾸면 결과가 바뀌는 걸 확인하려는 용도).
- 카드마다 따로 `fetch` 하고 각자 `try/catch`로 실패 화면을 그린다. `morning.html`은 `Promise.allSettled`로 한 카드 안의 두 호출도 분리한다.
- 외부 응답은 `textContent`/DOM으로 넣는다(`city.html`의 `innerHTML` 사용은 검사에서 지적됨).
- `city.html`은 호출이 **연쇄**다: 지오코딩 응답의 `results[0].latitude/longitude`가 예보·대기질 주소에 들어간다.

## 사용하는 API와 주의점

- Open-Meteo 예보/대기질/지오코딩 — 출처 "Weather data by Open-Meteo.com" 표기 필수. 미세먼지 등급은 환경부 기준(PM10 30/80/150, PM2.5 15/35/75), 모델 예측값이라는 안내를 붙인다.
- Frankfurter(`api.frankfurter.dev/v1/latest`) — 유럽중앙은행 기준 환율이며 하루 한 번 발표. 기준 날짜를 보여 주고 "실시간"이라고 쓰지 않는다.
- Nager.Date `NextPublicHolidays/KR` — 공휴일 이름은 한국어 `localName`을 먼저 보여 준다.
- Dog CEO `breeds/image/random` — 응답의 `message`가 이미지 URL.
