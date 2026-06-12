Week12 - CI/CD 구축 정리 + DevOps 전략

12강 파트1: CI/CD 구축

CI/CD 전체 흐름

코드를 변경하고 push하면 자동으로 빌드, 테스트, 배포까지 이어지는 구조다.
이번 강의에서는 그 흐름을 어떻게 구성하는지 정리했다.

코드 변경 (git push)
→ VCS(GitHub)가 변경 감지
→ CI 서버에 Webhook으로 알림
→ 빌드 / 테스트 / 배포 자동 수행
→ Slack 등으로 결과 알림

GitHub Actions 실습

GitHub Actions를 활용해서 Docker 이미지를 자동으로 빌드하고 Docker Hub에 push하는 파이프라인을 구성했다.

실습 구성:
Dockerfile 작성 (nginx 기반)
index.html 작성
.github/workflows/docker-build.yml 작성
Docker Hub Personal Access Token 발급 및 GitHub Secret 등록
push 트리거로 파이프라인 자동 실행 확인

결과: dokyeomlee/devops-practice:latest 이미지가 Docker Hub에 자동 push됨
VCS에서 관리하는 항목
분류내용인프라 설정Ansible Playbook, Serverspec, Dockerfile애플리케이션 테스트단위 테스트(모든 브랜치), 통합 테스트(main 브랜치)배포 설정Ansible Playbook, docker-compose

- GitHub Actions vs GitLab CI/CD

GitHub Actions
└── .github/workflows/\*.yml
└── GitHub에서 직접 Runner 제공
└── 퍼블릭 레포는 무료

GitLab CI/CD
└── .gitlab-ci.yml
└── Self-hosted Runner 직접 구성
└── On-Premise 환경에 적합

12강 파트2: DevOps 전략
핵심 주제 4가지

1. 다중 속도 IT 달성

서비스마다 개발 속도와 릴리즈 주기가 다르다.
각 애플리케이션 간의 종속성을 파악하고 문서화해서 독립적으로 배포할 수 있게 만드는 게 목표다.
API를 잘 정의해두면 서로의 내부 구현을 몰라도 통신할 수 있어서 결합도가 낮아진다.

2. 지속적인 타당성 확인

린 스타트업 방식처럼 완성된 제품을 한 번에 내놓는 게 아니라,
최소 기능 제품(MVP)부터 시작해서 고객 반응을 보면서 방향을 조정해야 한다.
A/B 테스트를 통해 두 버전을 동시에 테스트하고 더 나은 방향을 선택하는 방식이 대표적이다.

3. 실험 활성화

빠른 실패(Fail Fast)가 핵심이다.
실험을 두려워하지 말고, 잘못된 방향이면 빨리 알아채고 수정하는 게 낫다.
A/B 테스트, MVP 개발이 여기에 해당한다.

4. 안티프래질 시스템

장애가 발생하지 않는 시스템을 만드는 게 아니라,
장애가 발생해도 빠르게 복구할 수 있는 시스템을 만드는 게 목표다.

기존 관점: MTBF (평균 고장 간격) → 고장을 최대한 줄이자
안티프래질: MTTR (평균 수리 시간) → 고장나도 빨리 고치자

Blue-Green 배포, Immutable Infrastructure가 이 개념을 구현하는 대표적인 방법이다.

DevOps 플랫폼 도구 스택

소스 코드 관리 → GitHub / GitLab
빌드 → Maven, Gradle, npm
CI → GitHub Actions, Jenkins, GitLab CI
배포 자동화 → Ansible, Shell Script
미들웨어 설정 → Docker, Kubernetes
환경 프로비저닝 → Vagrant, Terraform

IaaS vs PaaS vs 컨테이너

IaaS (서비스형 인프라)
→ VM, 스토리지, 네트워크 제공
→ 고객이 OS부터 앱까지 직접 관리

PaaS (서비스형 플랫폼)
→ 런타임, 미들웨어까지 제공
→ 고객은 앱과 데이터만 관리

컨테이너
→ Docker + Kubernetes
→ 환경 추상화, 격리, 이식성

마이크로서비스 아키텍처 특징

서비스를 작은 단위로 컴포넌트화
비즈니스 기능 중심으로 구성
각 서비스는 독립적으로 배포 가능
분산 데이터 관리
실패를 가정한 설계 (안티프래질)
