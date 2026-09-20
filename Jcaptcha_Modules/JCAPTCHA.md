# 🛡️ jCAPTCHA V2 / V3

이 저장소는 [GitHub Pages](https://untitled-hub.github.io/choimin/)로 배포되어 있고, `index.html`이 실제 라이브 데모입니다. jCAPTCHA는 두 가지 버전으로 나뉘어 있습니다.

- **V2**: 클릭 없이 자동으로 진행되는 "클라우드플레어 스타일" 위젯. 실제로 봇을 탐지하진 않고, **접속 시간대만 확인**해서 지정한 시간대면 통과시키지 않습니다.
- **V3**: 문제 풀이형 jCAPTCHA (14개 문제 중 3개 무작위 출제, 함정 답 피하기). **현재 `index.html` 데모에서 사용 중인 버전이며, `jcaptcha.min.js`가 곧 V3입니다.**

두 버전은 서로 다른 전역 이름(`data-jcaptcha-v2` / `data-jcaptcha-v3`, `JCaptchaV2` / `JCaptchaV3`, 콜백 이름도 `jcaptchaV2Onxxx` / `jcaptchaV3Onxxx`)을 쓰기 때문에 **한 페이지에 같이 넣어도 서로 충돌하지 않습니다**.

## 📦 파일 구성

| 파일 | 용도 |
|---|---|
| `jcaptcha.min.js` | V3 배포용 스크립트 (문제 풀이형). `index.html`이 실제로 불러다 쓰는 파일. |
| `jcaptcha-v2.js` | V2 소스 (자동 시간 검사). 필요한 곳에 별도로 추가. |
| `index.html` | 라이브 데모 겸 실제 배포 페이지 (jCAPTCHA 탭에서 V3 데모 확인 가능) |
| `gwitricks.mp3` | V3 문제 10번(노래 제목 맞추기)에 쓰이는 음원 |
| `mc1.mp3` | V3 문제 14번(가사 채우기)에 쓰이는 음원 — **아직 저장소에 없음**, 추가되면 다른 파일들과 같은 폴더(저장소 루트)에 올려야 함 |

---

## 🌐 배포 방식 (안정성 관련 변경 사항)

기존에는 `https://cdn.jsdelivr.net/gh/UNTITLED-HUB/choimin/latest/jcaptcha.min.js` 처럼 jsDelivr의 `/latest/` 경로를 사용하고 있었는데, 이 경로는 **실제로 존재하지 않는 경로**라 스크립트가 전혀 로드되지 않는 상태였습니다 (jsDelivr가 "Couldn't find the requested file" 오류를 반환).

이번에 저장소에 이미 켜져 있던 **GitHub Pages**를 통해 불러오도록 바꿨습니다. GitHub Pages는 저장소 `main` 브랜치가 바뀌면 자동으로 재배포되고, 캐시 문제 없이 항상 최신 파일을 서빙합니다.

```html
<!-- 같은 저장소(GitHub Pages) 안에서: 파일명만 -->
<div data-jcaptcha-v3></div>
<script src="jcaptcha.min.js"></script>

<!-- 다른 사이트에서 불러올 때: 고정 URL -->
<script src="https://untitled-hub.github.io/choimin/jcaptcha.min.js"></script>
```

> jsDelivr을 다시 쓰고 싶다면 GitHub에서 **릴리스(태그)**를 만들고 `@` 버전 태그로 지정해야 합니다 (`.../gh/UNTITLED-HUB/choimin@태그명/jcaptcha.min.js`). `/latest/`라는 경로 자체는 jsDelivr 문법이 아니므로 그대로 쓰면 다시 깨집니다.

---

## V2 — 접속 시간 자동 검사

### 동작 방식

1. `data-jcaptcha-v2` 요소가 있으면 페이지 로드 시 자동으로 렌더링되고, 사람이 아무것도 누르지 않아도 곧바로 "검사"가 시작됩니다.
2. 검사 중에는 스피너와 함께 다음 문구가 보입니다:
   > 이 사이트는 악의적인 사용자로부터 보호합니다. 확인 후 연결됩니다.
3. 약 1초 뒤, **현재 접속하는 브라우저의 로컬 시간**을 기준으로 요일/시간을 확인합니다.
   - 월, 화, 목, 금: **08:40 ~ 14:30** 접속 시 차단
   - 수: **08:40 ~ 13:50** 접속 시 차단
   - 그 외 시간 / 토·일요일: 통과
4. **차단된 경우** — 배지가 빨간 ✗로 바뀌고 아래 문구가 표시됩니다:
   > 접속이 제한되었습니다 / 이 사이트는 현재 시간대에 접속할 수 없습니다. 잠시 후 다시 시도해주세요.
5. **통과된 경우** — 배지가 초록 ✓로 자동 체크되며 아래 문구가 표시되고 토큰이 발급됩니다:
   > 확인 완료.. 연결중

### 사용법

```html
<div data-jcaptcha-v2></div>
<script src="jcaptcha-v2.js"></script>
```

### 콜백 등록

```html
<script>
  window.jcaptchaV2OnSuccess = function (data) {
    console.log("통과, 토큰:", data.token);
    // 여기서 보호된 콘텐츠를 보여주거나 폼을 제출하세요
  };
  window.jcaptchaV2OnBlocked = function (data) {
    console.log("차단됨:", data.reason); // "time_restricted"
  };
</script>
```

- `jcaptchaV2OnSuccess` : 차단 시간대가 아니면 자동 호출됨
- `jcaptchaV2OnBlocked` : 차단 시간대에 접속하면 자동 호출됨 (자동 재시도는 없음 — 새로고침하면 그 시점 시간으로 다시 검사)

### 차단 시간대 수정하기

`jcaptcha-v2.js` 상단의 `BLOCK_RULES` 객체를 수정하면 됩니다. 요일은 `0=일요일 ~ 6=토요일`이고, 각 규칙은 `[시작시, 시작분, 종료시, 종료분]` 형식입니다 (양 끝 포함).

```js
const BLOCK_RULES = {
  1: [[8, 40, 14, 30]], // 월요일 08:40~14:30 차단
  3: [[8, 40, 13, 50]], // 수요일 08:40~13:50 차단
  // 필요하면 5(금)에 [[19,0,21,0]] 처럼 추가 시간대도 넣을 수 있음
};
```

### ⚠️ 참고

- 이 시간은 **접속하는 사람 기기의 로컬 시계**를 기준으로 판단합니다. 시스템 시계를 바꾸면 우회될 수 있으므로, 정말로 신뢰해야 하는 접근 제어라면 서버 쪽에서도 같은 시간 검사를 해야 합니다.
- 실제 자동화된 봇/트래픽 탐지 기능은 없습니다. 로딩 애니메이션과 문구만 클라우드플레어 스타일을 흉내낸 것입니다.

---

## V3 — 문제 풀이형 jCAPTCHA

체크박스를 누르면 아래 14개 문제 중 3개가 무작위로 출제됩니다. 각 문제마다 "함정 답"이 정해져 있고, **함정 답과 정확히 일치하는 답을 입력/선택하면 오히려 실패 처리**됩니다 (그 외 답은 전부 통과). 2개 이상 건너뛰어도 실패합니다. 3문제를 함정 없이 통과하면 성공 콜백과 토큰이 발급됩니다.

| 번호 | 유형 | 내용 |
|---|---|---|
| 1~3, 11 | 입력형 | ○ 채우기 (초성/글자 유추) |
| 4 | 버튼형 | 이모지 고르기 |
| 6 | 버튼형 | 과자 고르기 |
| 7 | 버튼형 | 계산 문제 |
| 8, 9 | 입력형 | 짧은 질문 답변 |
| 10 | 입력형 | 노래 제목 맞추기 (오디오 재생) |
| 12 | 입력형 | 코드 출력 횟수 세기 |
| 13 | 입력형 | 초성 `ㄴㅂㅌ` 해석하기 |
| 14 | 입력형 | 노래 가사 채우기 (오디오 재생, `mc1.mp3` 필요) |

### 사용법

```html
<div data-jcaptcha-v3></div>
<script src="jcaptcha.min.js"></script>
```

### 콜백 등록

```html
<script>
  window.jcaptchaV3OnSuccess = function (data) {
    console.log("인증 성공, 토큰:", data.token);
  };
  window.jcaptchaV3OnError = function (data) {
    console.log("인증 실패, 문제 재출제됨");
  };
</script>
```

### mp3 파일 위치

`gwitricks.mp3`(문제 10)와 `mc1.mp3`(문제 14)는 `jcaptcha.min.js`와 **같은 폴더**(저장소 루트)에 있어야 재생됩니다. `mc1.mp3`는 아직 업로드되지 않았으므로, 준비되는 대로 루트에 추가하세요.

---

## 클라이언트 전용 안내

V2, V3 모두 **클라이언트(브라우저)에서만 검증**합니다. 개발자 도구로 스크립트를 수정하거나 콘솔에서 콜백을 직접 호출하면 누구나 우회할 수 있습니다. 친구들끼리 장난용으로 쓰는 건 문제없지만, 실제로 막아야 하는 상황(로그인, 결제 등)에는 발급된 `token`을 서버에서도 검증하는 절차가 반드시 필요합니다.
