# AgentCore 런타임(Runtime)에서 MCP 서버 호스팅

## 개요

이 세션에서는 Amazon Bedrock AgentCore 런타임(Runtime)에서 MCP 도구(Tool)를 호스팅하는 방법에 대해 설명합니다.

Amazon Bedrock AgentCore Python SDK를 사용하여 에이전트(Agent) 함수를 Amazon Bedrock AgentCore와 호환되는 MCP 서버로 래핑합니다.
SDK가 MCP 서버 세부사항을 처리하므로 에이전트(Agent)의 핵심 기능에 집중할 수 있습니다.

Amazon Bedrock AgentCore Python SDK는 에이전트(Agent) 또는 도구(Tool) 코드를 AgentCore 런타임(Runtime)에서 실행할 수 있도록 준비합니다.

코드를 AgentCore 표준화된 HTTP 프로토콜 또는 MCP 프로토콜 계약으로 변환하여 기존 요청/응답 패턴(HTTP 프로토콜)을 위한 직접 REST API 엔드포인트(Endpoint) 통신 또는 도구(Tool) 및 에이전트(Agent) 서버를 위한 Model Context Protocol(MCP 프로토콜)을 허용합니다.

도구(Tool)를 호스팅할 때, Amazon Bedrock AgentCore Python SDK는 [세션 격리](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#session-management)를 위한 `MCP-Session-Id` 헤더와 함께 [Stateless Streamable HTTP] 전송 프로토콜을 구현합니다. 서버는 플랫폼이 생성한 Mcp-Session-Id 헤더를 거부하지 않도록 상태 비저장(stateless) 작동을 지원해야 합니다.
MCP 서버는 포트 `8000`에서 호스팅되며 하나의 호출(Invocation) 경로인 `mcp-POST`를 제공합니다. 이 상호작용 엔드포인트(Endpoint)는 MCP RPC 메시지를 수신하고 도구(Tool)의 기능을 통해 처리합니다. 응답 콘텐츠 유형으로 application/json과 text/event-stream 모두 지원합니다.

AgentCore 프로토콜을 MCP로 설정하면, AgentCore 런타임(Runtime)은 MCP 서버 컨테이너가 `0.0.0.0:8000/mcp` 경로에 있을 것으로 예상합니다. 이는 대부분의 공식 MCP 서버 SDK에서 지원하는 기본 경로입니다.

AgentCore 런타임(Runtime)은 기본적으로 세션 격리를 제공하고 헤더가 없는 모든 요청에 자동으로 Mcp-Session-Id 헤더를 추가하기 때문에 상태 비저장(stateless) streamable-http 서버를 호스팅해야 합니다. 이를 통해 MCP 클라이언트가 동일한 Bedrock AgentCore 런타임(Runtime) 세션 ID에 대한 연결 연속성을 가질 수 있습니다.

`InvokeAgentRuntime` API의 페이로드(Payload)는 완전히 통과(pass through)되므로 MCP와 같은 프로토콜의 RPC 메시지를 쉽게 프록시할 수 있습니다.

이 튜토리얼에서 배울 내용:

* 도구(Tool)가 있는 MCP 서버를 만드는 방법
* 서버를 로컬에서 테스트하는 방법
* AWS에 서버를 배포(Deployment)하는 방법
* 배포(Deployment)된 서버를 호출(Invocation)하는 방법

### 튜토리얼 세부사항

| 정보                | 세부사항                                                  |
|:--------------------|:---------------------------------------------------------|
| 튜토리얼 유형       | 도구(Tool) 호스팅                                         |
| 도구(Tool) 유형     | MCP 서버                                                  |
| 튜토리얼 구성 요소   | AgentCore 런타임(Runtime)에 도구(Tool) 호스팅, MCP 서버 생성 |
| 튜토리얼 분야       | 범용                                                      |
| 예제 복잡도         | 쉬움                                                      |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK 및 MCP Client         |

### 튜토리얼 아키텍처
이 튜토리얼에서는 기존 MCP 서버를 AgentCore 런타임(Runtime)에 배포(Deployment)하는 방법을 설명합니다.

데모 목적으로 `add_numbers`, `multiply_numbers`, `greet_users` 3가지 도구(Tool)가 있는 매우 간단한 MCP 서버를 사용합니다.

![MCP 아키텍처](images/hosting_mcp_server.png)

### 튜토리얼 주요 기능

* MCP 서버 호스팅
