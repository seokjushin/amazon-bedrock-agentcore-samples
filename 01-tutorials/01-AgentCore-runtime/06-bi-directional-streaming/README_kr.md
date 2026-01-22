# Amazon Bedrock AgentCore - 양방향 WebSocket 샘플

이 리포지토리에는 Amazon Bedrock AgentCore와의 양방향 WebSocket 통신을 시연하는 샘플 구현이 포함되어 있습니다:

- **Sonic** - AgentCore에 직접 배포(Deployment)되는 네이티브 Amazon Nova Sonic Python WebSocket 구현. 직접적인 이벤트 처리로 Nova Sonic 프로토콜에 대한 완전한 제어를 제공합니다. 음성 선택 및 중단 지원이 포함된 실시간 오디오 대화를 테스트하기 위한 웹 클라이언트가 포함되어 있습니다.

- **Strands** - 간소화된 실시간 오디오 대화를 위해 Strands BidiAgent를 사용하는 고수준 프레임워크(Framework) 구현. 자동 세션 관리, 도구(Tool) 통합 및 간소화된 API를 갖춘 Nova Sonic 위에 구축되었습니다. 프레임워크(Framework) 추상화의 이점을 누리는 빠른 프로토타이핑 및 프로덕션 애플리케이션에 적합합니다.

- **Echo** - AI 기능 없이 WebSocket 연결 및 인증(Authentication)을 테스트하기 위한 간단한 에코 서버.

모든 샘플은 루트 `setup.sh` 및 `cleanup.sh` 스크립트를 통해 통합된 설정 및 정리 프로세스를 사용합니다.

## 사전 요구사항

- 적절한 권한으로 구성(Configuration)된 AWS CLI
- Python 3.12+
- Docker (사용자 정의 에이전트(Agent) 이미지 빌드용)
- AWS 계정 ID

---

## Sonic 샘플 - 네이티브 Nova Sonic 2 구현

이 샘플은 **네이티브 Amazon Nova Sonic 2 Python WebSocket 서버**를 AgentCore에 직접 배포(Deployment)합니다. 직접적인 이벤트 처리로 Nova Sonic 프로토콜에 대한 완전한 제어를 제공하여 세션 관리, 오디오 스트리밍(Streaming) 및 응답 생성에 대한 완전한 가시성을 제공합니다.

**아키텍처:**

![AgentCore Sonic 아키텍처](./images/agentcore-sonic-architecture.png)

**최적 대상:** 세션 관리 및 이벤트 처리에 대한 세밀한 제어가 필요한 실시간 오디오 대화가 포함된 프로덕션 애플리케이션.

### 설정

```bash
# 필수
export ACCOUNT_ID=your_aws_account_id

# 선택사항 - 이것들을 사용자 정의하거나 기본값 사용
export AWS_REGION=us-east-1
export IAM_ROLE_NAME=WebSocketSonicAgentRole
export ECR_REPO_NAME=agentcore_sonic_images
export AGENT_NAME=websocket_sonic_agent

# AWS 인증(Authentication) (방법 중 하나 선택):

# 방법 1: AWS Profile 사용 (권장)
# AWS_PROFILE 환경 변수 설정 또는 기본 프로필에 적절한 액세스 권한이 있는지 확인
export AWS_PROFILE=your_profile_name

# 방법 2: AWS 자격 증명 직접 사용
# export AWS_ACCESS_KEY_ID=your_access_key
# export AWS_SECRET_ACCESS_KEY=your_secret_key
# export AWS_SESSION_TOKEN=your_session_token  # 선택사항, 임시 자격 증명용

# 설정 실행
./setup.sh sonic
```

### 클라이언트 실행

**옵션 1: 시작 스크립트 사용 (권장)**
```bash
./start_client.sh sonic
```

**옵션 2: 수동 시작**
```bash
# 환경 변수 내보내기 (설정 출력에서)
export AWS_REGION="us-east-1"

# AWS 인증(Authentication) (방법 중 하나 선택):
# AWS_PROFILE 환경 변수 설정 또는 기본 프로필에 적절한 액세스 권한이 있는지 확인
export AWS_PROFILE=your_profile_name
# 또는
# export AWS_ACCESS_KEY_ID=your_access_key
# export AWS_SECRET_ACCESS_KEY=your_secret_key
# export AWS_SESSION_TOKEN=your_session_token  # 선택사항

# 웹 클라이언트 시작
python sonic/client/client.py --runtime-arn "<agent-arn-from-setup>"
```

웹 클라이언트는:
1. 브라우저에서 자동으로 열림
2. 마이크 액세스 요청
3. AI와의 실시간 오디오 대화 활성화

### 기능

- **실시간 오디오 스트리밍(Streaming)** - 자연스럽게 말하고 즉각적인 응답 받기
- **음성 선택** - 여러 언어(영어, 프랑스어, 이탈리아어, 독일어, 스페인어)에서 여러 음성 선택
- **동적 음성 전환** - 활성 대화 중 음성 변경
- **중단 지원** - 응답 중간에 어시스턴트를 중단하는 바지인 기능
- **도구(Tool) 통합** - "지금 몇 시야?" 또는 "오늘이 무슨 요일이야?"와 같은 질문에 응답하는 샘플 `getDateTool` 포함
- **웹 기반 UI** - 설치 필요 없음, 모든 최신 브라우저에서 작동
- **세션 관리** - 자동 세션 처리 및 오디오 버퍼링
- **이벤트 로깅** - 필터링 기능이 있는 실시간 모든 WebSocket 이벤트 보기

### 샘플 도구(Tool): getDateTool

Sonic 구현에는 도구(Tool) 통합의 작동 예제가 포함되어 있습니다. `getDateTool`은 다음을 시연합니다:
- 클라이언트 구성(Configuration)에서 도구(Tool) 정의 ([`sonic/client/sonic-client.html`](sonic/client/sonic-client.html#L617-L628))
- 세션 설정 중 도구(Tool) 구성(Configuration) 전송 ([`sonic/client/sonic-client.html`](sonic/client/sonic-client.html#L773-L784))
- 서버에서 도구(Tool) 호출(Invocation) 처리 ([`sonic/websocket/s2s_session_manager.py`](sonic/websocket/s2s_session_manager.py#L339-L342))
- 대화 흐름으로 결과 반환

**시도해 보세요:** "지금 몇 시야?" 또는 "오늘 날짜가 뭐야?"와 같은 질문을 하면 어시스턴트가 현재 UTC 날짜와 시간을 얻기 위해 도구(Tool)를 호출(Invocation)합니다.

### 정리

```bash
./cleanup.sh sonic
```

---

## Strands 샘플 - 프레임워크(Framework) 기반 구현

이 샘플은 Amazon Nova Sonic과의 실시간 오디오 대화를 위해 **Strands BidiAgent 프레임워크(Framework)**를 사용하는 방법을 보여줍니다. Strands는 양방향 스트리밍(Streaming), 자동 세션 관리 및 도구(Tool) 통합을 단순화하는 고수준 추상화를 제공합니다.

**아키텍처:**

Strands 구현은 BidiAgent 프레임워크(Framework)를 사용하여 WebSocket 통신, 오디오 스트리밍(Streaming) 및 도구(Tool) 오케스트레이션의 복잡성을 자동으로 처리합니다.

**최적 대상:** 전체 Nova Sonic 기능을 유지하면서 프레임워크(Framework) 추상화의 이점을 누리는 빠른 프로토타이핑 및 프로덕션 애플리케이션.

### 설정

```bash
# 필수
export ACCOUNT_ID=your_aws_account_id

# 선택사항 - 이것들을 사용자 정의하거나 기본값 사용
export AWS_REGION=us-east-1
export IAM_ROLE_NAME=WebSocketStrandsAgentRole
export ECR_REPO_NAME=agentcore_strands_images
export AGENT_NAME=websocket_strands_agent

# AWS 인증(Authentication) (방법 중 하나 선택):

# 방법 1: AWS Profile 사용 (권장)
# AWS_PROFILE 환경 변수 설정 또는 기본 프로필에 적절한 액세스 권한이 있는지 확인
export AWS_PROFILE=your_profile_name

# 방법 2: AWS 자격 증명 직접 사용
# export AWS_ACCESS_KEY_ID=your_access_key
# export AWS_SECRET_ACCESS_KEY=your_secret_key
# export AWS_SESSION_TOKEN=your_session_token  # 선택사항, 임시 자격 증명용

# 설정 실행
./setup.sh strands
```

### 클라이언트 실행

**옵션 1: 시작 스크립트 사용 (권장)**
```bash
./start_client.sh strands
```

**옵션 2: 수동 시작**
```bash
# 환경 변수 내보내기 (설정 출력에서)
export AWS_REGION="us-east-1"

# AWS 인증(Authentication) (방법 중 하나 선택):
# AWS_PROFILE 환경 변수 설정 또는 기본 프로필에 적절한 액세스 권한이 있는지 확인
export AWS_PROFILE=your_profile_name
# 또는
# export AWS_ACCESS_KEY_ID=your_access_key
# export AWS_SECRET_ACCESS_KEY=your_secret_key
# export AWS_SESSION_TOKEN=your_session_token  # 선택사항

# 웹 클라이언트 시작
python strands/client/client.py --runtime-arn "<agent-arn-from-setup>"
```

웹 클라이언트는:
1. 브라우저에서 자동으로 열림
2. 마이크 액세스 요청
3. AI와의 실시간 오디오 대화 활성화

### 샘플 도구(Tool): 계산기

Strands 구현에는 프레임워크(Framework) 기반 도구(Tool) 통합을 시연하는 계산기 도구(Tool)가 포함되어 있습니다. 이 도구(Tool)는 기본 산술 연산을 수행할 수 있습니다.

**시도해 보세요:** "25 곱하기 4는 뭐야?" 또는 "100 나누기 5 계산해줘"와 같은 질문을 하면 어시스턴트가 계산기 도구(Tool)를 사용합니다.

### Sonic 샘플과의 주요 차이점

- **추상화 수준:** Strands는 Sonic의 직접 프로토콜 제어 대비 더 높은 수준의 API 제공
- **코드 복잡성:** Strands는 세션 관리를 위한 보일러플레이트 코드가 적음
- **도구(Tool) 통합:** 프레임워크(Framework)가 도구(Tool) 오케스트레이션을 자동으로 처리
- **유연성:** Sonic은 이벤트와 응답에 대한 더 세밀한 제어 제공

### 정리

```bash
./cleanup.sh strands
```

---

## Echo 샘플 - WebSocket 테스트

WebSocket 연결 및 인증(Authentication)을 테스트하기 위한 간단한 에코 서버.

### 설정

```bash
# 필수
export ACCOUNT_ID=your_aws_account_id

# 선택사항 - 이것들을 사용자 정의하거나 기본값 사용
export AWS_REGION=us-east-1
export IAM_ROLE_NAME=WebSocketEchoAgentRole
export DOCKER_REPO_NAME=agentcore_echo_images
export AGENT_NAME=websocket_echo_agent

# AWS 인증(Authentication) (방법 중 하나 선택):

# 방법 1: AWS Profile 사용 (권장)
# AWS_PROFILE 환경 변수 설정 또는 기본 프로필에 적절한 액세스 권한이 있는지 확인
export AWS_PROFILE=your_profile_name

# 방법 2: AWS 자격 증명 직접 사용
# export AWS_ACCESS_KEY_ID=your_access_key
# export AWS_SECRET_ACCESS_KEY=your_secret_key
# export AWS_SESSION_TOKEN=your_session_token  # 선택사항, 임시 자격 증명용

# 설정 실행
./setup.sh echo
```

### 클라이언트 실행

**옵션 1: 시작 스크립트 사용 (권장)**
```bash
./start_client.sh echo
```

**옵션 2: 수동 시작**
```bash
# 환경 변수 내보내기 (설정 출력에서)
export AWS_REGION="us-east-1"

# AWS 인증(Authentication) (방법 중 하나 선택):
# AWS_PROFILE 환경 변수 설정 또는 기본 프로필에 적절한 액세스 권한이 있는지 확인
export AWS_PROFILE=your_profile_name
# 또는
# export AWS_ACCESS_KEY_ID=your_access_key
# export AWS_SECRET_ACCESS_KEY=your_secret_key
# export AWS_SESSION_TOKEN=your_session_token  # 선택사항

# SigV4 헤더 인증(Authentication)으로 테스트
python echo/client/client.py --runtime-arn "<agent-arn-from-setup>" --auth-type headers

# SigV4 쿼리 매개변수로 테스트
python echo/client/client.py --runtime-arn "<agent-arn-from-setup>" --auth-type query
```

### 기능

- **간단한 에코** - 메시지를 보내고 에코 응답 확인
- **여러 인증(Authentication) 방법** - SigV4 헤더 또는 쿼리 매개변수 테스트
- **연결 테스트** - WebSocket 연결 확인
- **최소 종속성** - 디버깅에 적합

### 예상 출력

```
WebSocket connected
Sent: {"msg": "Hello, World! Echo Test"}
Received: {"msg": "Hello, World! Echo Test"}
Echo test PASSED
```

### 정리

```bash
./cleanup.sh echo
```

---

## 배포(Deployment) 작동 방식

`setup.sh` 스크립트는 전체 배포(Deployment)를 자동화합니다:

1. **사전 요구사항 확인** - jq, Python 3, Docker 및 AWS CLI가 설치되어 있는지 확인
2. **Python 환경** - 가상 환경 생성 및 종속성 설치
3. **Docker 빌드 및 푸시** - ARM64 컨테이너 이미지를 빌드하고 Amazon ECR에 푸시
4. **IAM 역할** - ECR, CloudWatch, Bedrock 및 X-Ray에 대한 권한이 있는 역할 생성
5. **Agent Runtime** - Bedrock AgentCore에 WebSocket 서버 배포(Deployment)
6. **구성(Configuration)** - 쉬운 정리를 위해 배포(Deployment) 세부사항을 `setup_config.json`에 저장

배포(Deployment) 후에는 ECR 리포지토리, IAM 역할, 실행 중인 에이전트(Agent) 런타임(Runtime) 및 쉬운 정리를 위한 구성(Configuration) 파일이 생성됩니다.

---

## 파일 구조

```
.
├── setup.sh                       # 통합 설정 스크립트 (폴더 매개변수 사용)
├── start_client.sh                # 통합 클라이언트 시작 스크립트 (폴더 매개변수 사용)
├── cleanup.sh                     # 통합 정리 스크립트 (폴더 매개변수 사용)
├── requirements.txt               # Python 종속성
├── websocket_helpers.py           # 공유 WebSocket 유틸리티 (SigV4 인증(Auth), 사전 서명 URL)
├── agent_role.json               # IAM 역할 정책 템플릿
├── trust_policy.json             # IAM 신뢰 정책
│
├── sonic/                        # Sonic 샘플 (네이티브 구현)
│   ├── client/                   # 웹 기반 클라이언트
│   │   ├── sonic-client.html     # 음성 선택이 있는 HTML UI
│   │   ├── client.py             # 웹 서버
│   │   └── requirements.txt      # 클라이언트 종속성
│   ├── websocket/                # 서버 구현
│   │   ├── server.py             # Sonic WebSocket 서버
│   │   ├── s2s_session_manager.py # 세션 관리
│   │   ├── s2s_events.py         # 이벤트 처리
│   │   ├── Dockerfile            # 컨테이너 정의
│   │   └── requirements.txt      # 서버 종속성
│   └── setup_config.json         # setup.sh에 의해 생성됨
│
├── strands/                      # Strands 샘플 (프레임워크(Framework) 기반)
│   ├── client/                   # 웹 기반 클라이언트
│   │   ├── strands-client.html   # HTML UI
│   │   ├── client.py             # 웹 서버
│   │   └── requirements.txt      # 클라이언트 종속성
│   ├── websocket/                # 서버 구현
│   │   ├── server.py             # Strands BidiAgent 서버
│   │   ├── Dockerfile            # 컨테이너 정의
│   │   └── requirements.txt      # 서버 종속성
│   └── setup_config.json         # setup.sh에 의해 생성됨
│
└── echo/                         # Echo 샘플 (테스트)
    ├── client/                   # CLI 클라이언트
    │   └── client.py             # 에코 테스트 클라이언트
    ├── websocket/                # 서버 구현
    │   ├── server.py             # 에코 WebSocket 서버
    │   ├── Dockerfile            # 컨테이너 정의
    │   └── requirements.txt      # 서버 종속성
    └── setup_config.json         # setup.sh에 의해 생성됨
```

---
