# Amazon Bedrock AgentCore에서 TypeScript MCP 서버

## 개요

이 튜토리얼은 Amazon Bedrock AgentCore 런타임(Runtime) 환경을 사용하여 TypeScript 기반 MCP(Model Context Protocol) 서버를 호스팅하는 방법을 보여줍니다.


### 튜토리얼 세부사항

| 정보                | 세부사항                                                  |
|:--------------------|:---------------------------------------------------------|
| 튜토리얼 유형       | TypeScript MCP 서버 호스팅                                |
| 도구(Tool) 유형     | MCP 서버                                                  |
| 튜토리얼 구성 요소   | AgentCore 런타임(Runtime)에 TypeScript MCP 서버 호스팅     |
| 튜토리얼 분야       | 범용                                                      |
| 예제 복잡도         | 쉬움                                                      |
| 사용 SDK            | MCP용 Anthropic TypeScript SDK                            |

## 사전 요구사항

- Node.js v22 이상
- Docker (컨테이너화용)
- Docker 이미지 저장을 위한 Amazon ECR (Elastic Container Registry)
- Bedrock AgentCore에 액세스할 수 있는 AWS 계정

---

## AgentCore 런타임(Runtime) 서비스 계약

[공식 서비스 계약 문서](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-service-contract.html)를 참조하세요.

**런타임(Runtime) 구성(Configuration):**
- **호스트:** `0.0.0.0`
- **포트:** `8000`
- **전송:** Stateless `streamable-http`
- **엔드포인트(Endpoint) 경로:** `POST /mcp`

## 로컬 개발

1. 종속성 설치

```
npm install
```

2. AWS 자격 증명 설정
```
aws configure
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_REGION=us-east-1
```

3. 서버 시작
```
npm run start
```

4. [MCP inspector](https://github.com/modelcontextprotocol/inspector)를 사용하여 로컬에서 테스트

```
npx @modelcontextprotocol/inspector
```

## Docker 배포(Deployment)

1. ECR 리포지토리 생성
```
aws ecr create-repository --repository-name mcp-server --region us-east-1
```
2. ECR에 이미지 빌드 및 푸시
```
# 로그인 토큰 가져오기
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin [account-id].dkr.ecr.us-east-1.amazonaws.com

docker buildx --platform linux/arm64 \
  -t [account-id].dkr.ecr.us-east-1.amazonaws.com/mcp-server:latest --push .
```

3. Bedrock AgentCore에 배포(Deployment)

    - AWS 콘솔 → Bedrock → AgentCore → Create Agent로 이동
    - 프로토콜로 MCP 선택
    - Agent Runtime 구성(Configuration):
        - Image URI: [account-id].dkr.ecr.us-east-1.amazonaws.com/mcp-server:latest
        - Bedrock 모델 액세스를 위한 IAM 권한 설정
        - Agent Sandbox에서 배포(Deployment) 및 테스트


4. 인코딩된 ARN MCP URL 구성

```
echo "agent_arn" | sed 's/:/%3A/g; s/\//%2F/g'
```

```
https://bedrock-agentcore.{region}.amazonaws.com/runtimes/{encoded_arn}/invocations?qualifier=DEFAULT
```

5. [MCP inspector](https://github.com/modelcontextprotocol/inspector)와 함께 MCP URL 사용.

## 참조
- https://aws.amazon.com/bedrock/agentcore/
- https://github.com/modelcontextprotocol/typescript-sdk
- https://github.com/modelcontextprotocol/inspector


