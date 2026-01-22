# IAM 인증(Authentication)을 사용한 AgentCore A2A 샘플

이 샘플은 인바운드 인증(Authentication)에 AWS IAM을 사용하여 Amazon Bedrock AgentCore 런타임(Runtime)에 A2A(Agent-to-Agent) 에이전트(Agent)를 배포(Deployment)하는 방법을 보여줍니다. A2A 프로토콜과 IAM 기반 인증(Authentication)을 결합하여 AWS 자격 증명을 사용하여 통신하는 에이전트(Agent)를 배포(Deployment)하는 안전한 방법을 제공합니다.

## 아키텍처

```
┌─────────────┐         IAM 인증(Auth)       ┌──────────────────┐
│   클라이언트  │ ────────────────────────> │  A2A 에이전트(Agent)│
│   (SigV4)    │                           │  (AgentCore)     │
└─────────────┘                           └──────────────────┘
```

## 주요 기능

* 에이전트(Agent) 간 통신을 위한 A2A 프로토콜
* AWS IAM (SigV4) 인증(Authentication)
* 에이전트(Agent) 구현을 위한 Strands 프레임워크(Framework)
* AgentCore 런타임(Runtime)에 배포(Deployment)

## 사전 요구사항

* Python 3.10+
* 자격 증명이 구성(Configuration)된 AWS CLI
* 실행 중인 Docker
* pip 설치됨

## 설치

```bash
pip install -r requirements.txt
```

## 빠른 시작

### 옵션 1: Jupyter Notebook 사용 (권장)

```bash
jupyter notebook hosting_a2a_iam_auth.ipynb
```

노트북의 단계별 지침을 따르세요.

### 옵션 2: 수동 배포(Deployment)

#### 1단계: 로컬에서 테스트 (선택사항)

```bash
# 터미널 1: 에이전트(Agent) 시작
python agent.py

# 터미널 2: 에이전트(Agent) 카드 테스트
curl http://localhost:9000/.well-known/agent-card.json | jq .

# 터미널 2: 테스트 메시지 전송
curl -X POST http://localhost:9000 \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "req-001",
    "method": "message/send",
    "params": {
      "message": {
        "role": "user",
        "parts": [{
          "kind": "text",
          "text": "Hello! What can you do?"
        }],
        "messageId": "test-001"
      }
    }
  }' | jq .
```

#### 2단계: AgentCore 런타임(Runtime)에 배포(Deployment)

```bash
python deploy.py
```

스크립트는 다음을 수행합니다:
1. ECR에 Docker 이미지 빌드 및 푸시
2. 필요한 권한이 있는 실행 역할 생성
3. AgentCore 런타임(Runtime)에 에이전트(Agent) 배포(Deployment)
4. 에이전트(Agent) ARN 출력

#### 3단계: 배포(Deployment)된 에이전트(Agent) 테스트

```bash
# deploy 출력에서 에이전트(Agent) ARN 설정
export AGENT_ARN="arn:aws:bedrock-agentcore:us-east-1:..."

# 테스트 클라이언트 실행
python client.py
```

## 예상 출력

```
INFO:__main__:Using AWS region: us-east-1
INFO:__main__:Testing agent: arn:aws:bedrock-agentcore:...
INFO:__main__:Session ID: ...
INFO:__main__:Fetching agent card...
INFO:__main__:Agent: A2A IAM Auth Agent
INFO:__main__:Description: A simple A2A agent demonstrating IAM authentication...

============================================================
INFO:__main__:Sending message: Hello! What can you do?

INFO:__main__:Agent response:
I am an A2A agent deployed on Amazon Bedrock AgentCore Runtime...
```

## 문제 해결

### Docker가 실행되지 않음

```
Error: Cannot connect to the Docker daemon
Solution: Docker Desktop 또는 Docker 데몬 시작
```

### AWS 자격 증명이 구성(Configuration)되지 않음

```
Error: Unable to locate credentials
Solution: 'aws configure' 실행 또는 AWS_PROFILE 설정
```

### 권한 오류

배포(Deployment)에는 다음 IAM 권한이 필요합니다:
- `bedrock-agentcore:*` - AgentCore 작업
- `ecr:*` - 컨테이너 레지스트리
- `iam:CreateRole`, `iam:PutRolePolicy` - 실행 역할 생성
- `codebuild:*` - 컨테이너 이미지 빌드
- `logs:*` - CloudWatch 로그 액세스

실행 역할에는 다음이 필요합니다:
- `ecr:GetAuthorizationToken`, `ecr:BatchGetImage`, `ecr:GetDownloadUrlForLayer` - ECR 액세스
- `bedrock:InvokeModel`, `bedrock:InvokeModelWithResponseStream` - Bedrock 모델 액세스
- `logs:*` - CloudWatch 로그
- `bedrock-agentcore:GetWorkloadAccessToken*` - 워크로드 ID

전체 실행 역할 정책은 `execution-role-policy.json`을 참조하세요.

## 정리

```python
from bedrock_agentcore_starter_toolkit.operations.runtime import destroy_bedrock_agentcore
from pathlib import Path

destroy_bedrock_agentcore(
    config_path=Path(".bedrock-agentcore-config.yaml"),
    region="us-east-1"
)
```

## 파일

* `agent.py` - 도구(Tool)가 있는 A2A 에이전트(Agent) 구현
* `client.py` - IAM 인증(Authentication)으로 배포(Deployment)된 에이전트(Agent)를 테스트하는 클라이언트
* `deploy.py` - 배포(Deployment) 스크립트
* `requirements.txt` - Python 종속성
* `execution-role-policy.json` - 실행 역할을 위한 IAM 정책
* `hosting_a2a_iam_auth.ipynb` - 단계별 튜토리얼 노트북

## 참조

* [AgentCore 런타임(Runtime) 문서](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html)
* [A2A 프로토콜 사양](https://a2a-protocol.org/dev/specification/)
* [Strands Agents 프레임워크(Framework)](https://strandsagents.com/latest/)

