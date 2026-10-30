# j-hub (`choimin`)

j-hub는 Choimini, jCAPTCHA, 해킹 시뮬레이터와 앱 다운로드를 안내하는 홈페이지입니다.

| 저장소 | 역할 |
| --- | --- |
| `choimin` | j-hub 홈페이지, 실행 자산, 빌드된 앱과 확장 프로그램 |
| `choimini` | Choimini 메인 웹사이트 |
| `hacking` | 해킹 시뮬레이터 |
| `choimini-apps` | 앱 개발 소스와 테스트 (비공개) |

## 앱 배포

`App/`에 완성된 파일을 다음 이름으로 올립니다. 확장자를 포함해야 합니다.

- Android: `Choimini-Android-1.2.39.apk`
- Desktop: `Choimini-Desktop-2.2.1.exe` 또는 `Choimini-Desktop-2.2.1.zip`

숫자 버전은 예시입니다. 홈페이지는 이름에 포함된 버전을 비교하여 플랫폼별 최신 파일을 선택합니다. 버전이 붙은 파일이 없거나 목록을 가져오지 못하면 기존 `Choimini-Android.apk`와 `Choimini-Desktop-Windows.zip` 링크를 유지합니다. 기존 파일은 임의 삭제하거나 새 버전으로 표시하지 않습니다.

`Jcaptcha_Modules/jcaptcha.min.js`, `gwitricks.mp3`, `mc1.mp3`는 jCAPTCHA가 사용하는 자산입니다. jCAPTCHA는 클라이언트 체험용이며 보안 인증 수단이 아닙니다. 개발용 소스와 비밀키는 이 공개 저장소에 올리지 않습니다.
