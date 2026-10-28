# j-hub (`choimin`)

홈페이지와 사용자에게 제공하는 배포 파일을 보관합니다.

| 저장소 | 역할 |
| --- | --- |
| `choimin` | j-hub 홈페이지, 실행 자산, 빌드된 앱·설치용 확장 ZIP |
| `choimini` | 최미나이 메인 웹사이트 |
| `choimini-apps` (비공개) | 앱 소스 전체 프로젝트·개발 문서·테스트·빌드 설정 |

## 유지하는 배포물

- `index.html`, `choimini-localai.js`: 홈페이지가 실행하는 정적 HTML/JS. 별도 번들 빌드가 없으므로 이 파일 자체가 배포물입니다.
- `Jcaptcha_Modules/jcaptcha.min.js`, `gwitricks.mp3`, `mc1.mp3`: 홈페이지·메인 사이트가 사용하는 CAPTCHA 실행 자산.
- `App/Choimini-Android.apk`, `App/Choimini-Desktop-Windows.zip`: 빌드된 앱.
- `App/choimin-lnc.zip`, `App/choimini-extension.zip`: 사용자가 설치하는 확장 배포물. JavaScript를 포함해도 설치에 필요하므로 유지합니다.

개발용 소스 ZIP과 개발 문서는 비공개 `choimini-apps`에서 관리합니다. 이전 시 대상 파일의 해시를 먼저 확인합니다. 기존 Git 이력은 유지합니다.
