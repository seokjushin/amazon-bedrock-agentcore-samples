# Amazon Bedrock AgentCore 런타임(Runtime)에서 Amazon Bedrock 모델과 Strands Agents를 사용한 스트리밍(Streaming) 응답

## 개요

이 튜토리얼에서는 기존 에이전트(Agent)와 함께 Amazon Bedrock AgentCore 런타임(Runtime)을 사용하여 스트리밍(Streaming) 응답을 구현하는 방법을 배웁니다.

실시간 스트리밍(Streaming) 기능을 시연하는 Amazon Bedrock 모델을 사용하는 Strands Agents 예제에 초점을 맞춥니다.

### 튜토리얼 세부사항

| 정보                | 세부사항                                                                         |
|:--------------------|:--------------------------------------------------------------------------------|
| 튜토리얼 유형       | 스트리밍(Streaming)이 포함된 대화형                                               |
| 에이전트(Agent) 유형 | 단일                                                                            |
| 에이전트(Agent) 프레임워크(Framework) | Strands Agents                                                    |
| LLM 모델            | Anthropic Claude Haiku 4.5                                                      |
| 튜토리얼 구성 요소   | AgentCore 런타임(Runtime)으로 스트리밍(Streaming) 응답. Strands Agent 및 Amazon Bedrock 모델 사용 |
| 튜토리얼 분야       | 범용                                                                            |
| 예제 복잡도         | 쉬움                                                                            |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK 및 boto3                                     |

### 튜토리얼 아키텍처

이 튜토리얼에서는 스트리밍(Streaming) 에이전트(Agent)를 AgentCore 런타임(Runtime)에 배포(Deployment)하는 방법을 설명합니다.

데모 목적으로 스트리밍(Streaming) 기능이 있는 Amazon Bedrock 모델을 사용하는 Strands Agent를 사용합니다.

이 예제에서는 `get_weather`, `get_time`, `calculator` 세 가지 도구(Tool)가 있는 간단한 에이전트(Agent)를 사용하지만, 실시간 스트리밍(Streaming) 응답 기능이 향상되었습니다.

<div style="text-align:left">
    <img src="images/architecture_runtime.png" width="100%"/>
</div>

### 튜토리얼 주요 기능

* Amazon Bedrock AgentCore 런타임(Runtime)에서 스트리밍(Streaming) 응답 구현
* Server-Sent Events (SSE)를 사용한 실시간 부분 결과 전달
* 스트리밍(Streaming) 기능이 있는 Amazon Bedrock 모델 사용
* 비동기 스트리밍(Streaming) 지원이 있는 Strands Agents 사용
* 점진적 응답 표시로 향상된 사용자 경험

