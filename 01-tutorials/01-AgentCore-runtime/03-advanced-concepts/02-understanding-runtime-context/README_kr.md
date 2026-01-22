# AgentCore 런타임(Runtime)에서 런타임(Runtime) 컨텍스트와 세션 관리 이해

## 개요

이 튜토리얼에서는 Amazon Bedrock AgentCore 런타임(Runtime)에서 런타임(Runtime) 컨텍스트와 세션 관리를 이해하고 작업하는 방법을 배웁니다. 이 예제는 AgentCore 런타임(Runtime)이 세션을 처리하고, 여러 호출(Invocation)에 걸쳐 컨텍스트를 유지하며, 에이전트(Agent)가 컨텍스트 객체를 통해 런타임(Runtime) 정보에 액세스하는 방법을 보여줍니다.

Amazon Bedrock AgentCore 런타임(Runtime)은 각 사용자 상호작용에 대해 격리된 세션을 제공하여 에이전트(Agent)가 여러 호출(Invocation)에 걸쳐 컨텍스트와 상태를 유지하면서 서로 다른 사용자 간의 완전한 보안 격리를 보장합니다.

### 튜토리얼 세부사항

| 정보                | 세부사항                                                                         |
|:--------------------|:--------------------------------------------------------------------------------|
| 튜토리얼 유형       | 컨텍스트 및 세션 관리                                                            |
| 에이전트(Agent) 유형 | 단일                                                                            |
| 에이전트(Agent) 프레임워크(Framework) | Strands Agents                                                    |
| LLM 모델            | Anthropic Claude Haiku 4.5                                                      |
| 튜토리얼 구성 요소   | 런타임(Runtime) 컨텍스트, 세션 관리, AgentCore 런타임(Runtime), Strands Agent 및 Amazon Bedrock 모델 |
| 튜토리얼 분야       | 범용                                                                            |
| 예제 복잡도         | 중급                                                                            |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK 및 boto3                                     |

### 튜토리얼 아키텍처

이 튜토리얼에서는 Amazon Bedrock AgentCore 런타임(Runtime)이 세션을 관리하고 에이전트(Agent)에 컨텍스트를 제공하는 방법을 탐색합니다. 다음을 시연합니다:

1. **세션 연속성**: 동일한 세션 ID가 여러 호출(Invocation)에 걸쳐 컨텍스트를 유지하는 방법
2. **컨텍스트 객체**: 에이전트(Agent)가 컨텍스트 매개변수를 통해 런타임(Runtime) 정보에 액세스하는 방법
3. **세션 격리**: 서로 다른 세션 ID가 완전히 격리된 환경을 생성하는 방법
4. **페이로드(Payload) 유연성**: 페이로드(Payload)를 통해 에이전트(Agent)에 사용자 정의 데이터를 전달하는 방법

데모 목적으로 이러한 세션 관리 기능을 보여주는 Strands Agent를 사용합니다.


<div style="text-align:left">
    <img src="images/architecture_runtime.png" width="60%"/>
</div>

### 튜토리얼 주요 기능

* **세션 기반 컨텍스트 관리**: AgentCore 런타임(Runtime)이 세션 내에서 컨텍스트를 유지하는 방법 이해
* **런타임(Runtime) 세션 수명 주기**: 세션 생성, 유지 및 종료에 대해 학습
* **컨텍스트 객체 액세스**: 컨텍스트 매개변수를 통해 세션 ID와 같은 런타임(Runtime) 정보 액세스
* **세션 격리**: 서로 다른 세션이 완전한 격리를 제공하는 방법 시연
* **페이로드(Payload) 처리**: 사용자 정의 페이로드(Payload) 구조를 통한 유연한 데이터 전달
* **호출(Invocation) 간 상태**: 동일한 세션 내에서 여러 호출(Invocation)에 걸쳐 에이전트(Agent) 상태 유지

