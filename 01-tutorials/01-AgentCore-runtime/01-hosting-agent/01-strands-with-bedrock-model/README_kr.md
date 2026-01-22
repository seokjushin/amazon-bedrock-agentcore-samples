# Amazon Bedrock AgentCore 런타임(Runtime)에서 Amazon Bedrock 모델과 Strands Agents 호스팅

## 개요

이 튜토리얼에서는 Amazon Bedrock AgentCore 런타임(Runtime)을 사용하여 기존 에이전트(Agent)를 호스팅하는 방법을 배웁니다.

Amazon Bedrock 모델을 사용하는 Strands Agents 예제에 초점을 맞춥니다. Amazon Bedrock 모델을 사용하는 LangGraph는 [여기](../02-langgraph-with-bedrock-model)를 확인하고, OpenAI 모델을 사용하는 Strands Agents는 [여기](../03-strands-with-openai-model)를 확인하세요.


### 튜토리얼 세부사항

| 정보                | 세부사항                                                                         |
|:--------------------|:--------------------------------------------------------------------------------|
| 튜토리얼 유형       | 대화형                                                                          |
| 에이전트(Agent) 유형 | 단일                                                                            |
| 에이전트(Agent) 프레임워크(Framework) | Strands Agents                                                    |
| LLM 모델            | Anthropic Claude Haiku 4.5                                                      |
| 튜토리얼 구성 요소   | AgentCore 런타임(Runtime)에 에이전트(Agent) 호스팅. Strands Agent 및 Amazon Bedrock 모델 사용 |
| 튜토리얼 분야       | 범용                                                                            |
| 예제 복잡도         | 쉬움                                                                            |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK 및 boto3                                     |

### 튜토리얼 아키텍처

이 튜토리얼에서는 기존 에이전트(Agent)를 AgentCore 런타임(Runtime)에 배포(Deployment)하는 방법을 설명합니다.

데모 목적으로 Amazon Bedrock 모델을 사용하는 Strands Agent를 사용합니다.

이 예제에서는 `get_weather`와 `get_time` 두 가지 도구(Tool)가 있는 매우 간단한 에이전트(Agent)를 사용합니다.

<div style="text-align:left">
    <img src="images/architecture_runtime.png" width="100%"/>
</div>

### 튜토리얼 주요 기능

* Amazon Bedrock AgentCore 런타임(Runtime)에서 에이전트(Agent) 호스팅
* Amazon Bedrock 모델 사용
* Strands Agents 사용

