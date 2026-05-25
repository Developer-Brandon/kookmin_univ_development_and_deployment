Week07 - DevOps를 위한 변화 (12 Factor App)

---

개요
최신 웹 애플리케이션의 베스트 프랙티스 문서
참고: http://12factor.net/ko

12 Factor App 정리

1. Code Base

하나의 애플리케이션 = 하나의 저장소
테스트/운영 환경을 코드로 구분하지 않음
구성관리도구(IaC)로 관리
하나의 코드에서 Release

2. 의존 관계 (Dependencies)

모든 라이브러리 의존성을 Manifest 파일로 명시
언어/프레임워크별 관리 파일

Python: requirements.txt
Node.js: package.json
Ruby: Gemfile

특정 시스템/라이브러리에 의존하지 않기

3. 설정 (Config)

코드에 리소스 정보, 환경 정보 포함 금지

DB 연결 정보, 인증정보, 호스트 이름 등

환경 변수(Environment Variable) 로 관리
.env 파일 활용

4. Backend 서비스

네트워크를 통해 이용하는 모든 서비스

데이터 스토어, 메시지 큐, 캐시 등

로컬/클라우드 구분 없이 동일하게 취급
설정 파일 또는 환경변수로 전환 가능하게 설계

5. Build / Release / 실행

코드를 Release 하기까지 3단계로 명확히 구분

Build Stage: 의존 관계 해소, 로컬 빌드
Release Stage: 빌드 결과 + 환경별 설정 결합
실행 Stage: 선택한 리소스에서 프로세스 가동

6. 프로세스 (Processes)

Stateless 하게 설계 (상태 정보 갖지 않음)
상태 정보가 필요하면 Backend 서비스에서 관리
비공유 아키텍처 (프로세스간 독립적/자율적)
세션은 데이터 스토어에 저장

7. Port Binding

애플리케이션이 직접 포트에 바인딩
Apache/Tomcat 같은 컨테이너에 의존하지 않음
HTTP 서비스로 직접 공개

8. 병행성 (Concurrency)

Unix 프로세스 모델 사용
개별 워크로드 유형을 프로세스 타입에 할당
다양한 워크로드를 처리할 수 있도록 설계

9. 폐기 용이성 (Disposability)

기동 시간 최소화
즉시 기동/종료할 수 있도록 설계
SIGTERM 시그널 활용
정상적인 종료 과정으로 동작

10. 개발 환경 / 운영 환경 일치

지속적인 배포(Deploy)를 쉽게 하기 위해
개발 / 스테이징 / 운영 환경을 최대한 일치
Docker 활용으로 환경 일치 가능

11. 로그 (Logs)

로그 파일에 기록하여 관리하지 않기
출력 스트림(output stream)으로 처리
애플리케이션 외부에서 수정/변경 가능하게

12. 관리 프로세스 (Admin Processes)

DB 마이그레이션, 레코드 수정, 검사용 명령어 등
일회성 프로세스로 수행
일반 애플리케이션과 동일한 저장소에서 관리
