# Amazon Bedrock AgentCore 런타임(Runtime)

## 개요
Amazon Bedrock AgentCore 런타임(Runtime)은 AI 에이전트(Agent)와 도구(Tool)를 배포(Deployment)하고 확장하기 위해 설계된 안전한 서버리스 런타임(Runtime)입니다.
모든 프레임워크(Framework), 모델, 프로토콜을 지원하여 개발자가 최소한의 코드 변경으로 로컬 프로토타입을 프로덕션 준비 솔루션으로 전환할 수 있습니다.

Amazon BedrockAgentCore Python SDK는 에이전트(Agent) 함수를 Amazon Bedrock과 호환되는 HTTP 서비스로 배포(Deployment)하는 데 도움이 되는 경량 래퍼를 제공합니다. 모든 HTTP 서버 세부사항을 처리하므로 에이전트(Agent)의 핵심 기능에 집중할 수 있습니다.

함수에 `@app.entrypoint` 데코레이터를 추가하고 SDK의 `configure` 및 `launch` 기능을 사용하여 에이전트(Agent)를 AgentCore 런타임(Runtime)에 배포(Deployment)하기만 하면 됩니다. 그러면 애플리케이션은 SDK 또는 boto3, AWS SDK for JavaScript, AWS SDK for Java와 같은 AWS 개발자 도구를 사용하여 이 에이전트(Agent)를 호출(Invocation)할 수 있습니다.

![런타임(Runtime) 개요](images/runtime_overview.png)

## 주요 기능

### 프레임워크(Framework) 및 모델 유연성

- 모든 프레임워크(Framework)(Strands Agents, LangChain, LangGraph, CrewAI 등)에서 에이전트(Agent)와 도구(Tool) 배포(Deployment)
- 모든 모델 사용 (Amazon Bedrock 내외부)

### 통합

Amazon Bedrock AgentCore 런타임(Runtime)은 통합 SDK를 통해 다른 Amazon Bedrock AgentCore 기능과 통합됩니다:

- Amazon Bedrock AgentCore 메모리(Memory)
- Amazon Bedrock AgentCore 게이트웨이(Gateway)
- Amazon Bedrock AgentCore 관측성(Observability)
- Amazon Bedrock AgentCore 도구(Tools)

이 통합은 개발 프로세스를 단순화하고 AI 에이전트(Agent)를 구축, 배포(Deployment), 관리하기 위한 포괄적인 플랫폼을 제공하는 것을 목표로 합니다.

### 사용 사례

런타임(Runtime)은 다음을 포함한 광범위한 애플리케이션에 적합합니다:

- 실시간 대화형 AI 에이전트(Agent)
- 장기 실행, 복잡한 AI 워크플로우
- 멀티모달 AI 처리 (텍스트, 이미지, 오디오, 비디오)

## 튜토리얼 개요

이 튜토리얼에서는 다음 기능을 다룹니다:

- [에이전트(Agent) 호스팅](01-hosting-agent)
- [MCP 서버 호스팅](02-hosting-MCP-server)
- [고급 개념](03-advanced-concepts)

