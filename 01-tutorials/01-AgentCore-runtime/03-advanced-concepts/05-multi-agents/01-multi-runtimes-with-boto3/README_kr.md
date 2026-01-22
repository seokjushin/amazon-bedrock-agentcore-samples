# Amazon Bedrock AgentCore를 사용한 분산 멀티에이전트(Multi-Agent) 솔루션

## 개요

이 튜토리얼에서는 각각의 Bedrock AgentCore 런타임(Runtime)에서 독립적으로 에이전트(Agent)를 호스팅하고 서로 다른 에이전트(Agent) 프레임워크(Framework)로 구축하는 방법을 배웁니다. 그런 다음 분산 멀티에이전트(Multi-Agent) 솔루션을 위해 에이전트(Agent) 간의 통신을 활성화합니다.

이 예제에서는 다음을 생성합니다:
1. 프로그래밍 및 기술 문제 해결에 대한 기술적 질문에 답변하는 것을 전문으로 하는 기술 에이전트(Agent)(tech_agent).
2. 회사 복리후생을 전문으로 하는 HR 에이전트(Agent)(hr_agent).
3. 질문을 기술 또는 HR 에이전트(Agent)로 라우팅하는 오케스트레이터 에이전트(Agent)(orchestrator_agent).

이 세 에이전트(Agent)를 함께 배치하면 사용자 질문을 적절한 하위 에이전트(Agent)로 라우팅할 수 있는 감독자가 있는 멀티에이전트(Multi-Agent) 구성을 얻습니다. 이 시스템은 회사에서 직원이 가질 수 있는 다양한 질문에 답변할 수 있습니다.

## 튜토리얼 세부사항

| 정보                | 세부사항                                                                         |
|:--------------------|:--------------------------------------------------------------------------------|
| 튜토리얼 유형       | 대화형                                                                          |
| 에이전트(Agent) 유형 | 멀티에이전트(Multi-Agent) (감독자가 에이전트(Agent)를 도구(Tool)로 호출)           |
| 에이전트(Agent) 프레임워크(Framework) | Strands Agents 및 LangGraph                                       |
| LLM 모델            | Anthropic Claude Haiku 4.5                                                      |
| 튜토리얼 구성 요소   | AgentCore 런타임(Runtime)에 에이전트(Agent) 호스팅 및 멀티에이전트(Multi-Agent) 협업 활성화 |
| 튜토리얼 분야       | 범용                                                                            |
| 예제 복잡도         | 중간                                                                            |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK 및 boto3                                     |

## 튜토리얼 아키텍처

<div style="text-align:left">
    <img src="architecture.png" width="100%"/>
</div>

## 시작하기

이 노트북의 지침을 따르세요 [distributed_agents_with_agentcore.ipynb](distributed_agents_with_agentcore.ipynb)
