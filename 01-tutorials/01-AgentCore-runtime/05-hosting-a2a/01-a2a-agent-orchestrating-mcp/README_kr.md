## AgentCore 런타임(Runtime)에서 A2A 시작하기

### 개요

Amazon Bedrock AgentCore 런타임(Runtime)은 AI 에이전트(Agent)와 도구(Tool)를 배포(Deployment)하고 확장하기 위해 설계된 안전한 서버리스 런타임(Runtime)입니다.
모든 프레임워크(Framework), 모델, 프로토콜을 지원하여 개발자가 최소한의 코드 변경으로 로컬 프로토타입을 프로덕션 준비 솔루션으로 전환할 수 있습니다.

[Strands Agents](https://strandsagents.com/latest/)는 에이전트(Agent)를 구축하기 위한 사용하기 쉬운 코드 우선 프레임워크(Framework)입니다.

최근 AWS는 AgentCore 런타임(Runtime)에 대한 [A2A 지원](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-a2a.html)을 발표했습니다.

이 예제에서는 Amazon Bedrock AgentCore와 Strands Agents를 사용하여 멀티에이전트(Multi-Agent) 시스템을 구축합니다.

이 튜토리얼에서는 3개의 에이전트(Agent)를 생성하는 과정을 안내합니다. 첫 번째는 MCP를 사용하여 AWS 문서를 소비하는 AWS 문서 전문가입니다. 두 번째는 웹에서 최신 블로그와 AWS 뉴스를 검색하고, 세 번째는 MCP를 사용하여 이전 에이전트(Agent)들을 호출(Invocation)하는 오케스트레이터입니다.

<img src="images/architecture.png" style="width: 80%;">

### 튜토리얼 개요

이 튜토리얼에서는 다음 기능을 다룹니다:

- [1 - Strands와 Bedrock AgentCore로 A2A 시작하기](01-a2a-getting-started-agentcore-strands.ipynb)
- [2 - A2A를 사용하여 하위 에이전트(Agent)를 호출(Invocation)하는 오케스트레이터 생성](02-a2a-deploy-orchestrator.ipynb)
- [3 - 정리](03-a2a-cleanup.ipynb)

