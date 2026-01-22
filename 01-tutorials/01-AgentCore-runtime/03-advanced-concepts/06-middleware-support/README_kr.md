# AgentCore 런타임(Runtime)에서 미들웨어(Middleware) 지원

## 개요

이 튜토리얼은 Amazon Bedrock AgentCore 런타임(Runtime)에서 미들웨어(Middleware)를 구현하는 방법을 보여줍니다. 미들웨어(Middleware)를 사용하면 에이전트(Agent)에 도달하기 전에 요청을 처리하고 클라이언트로 다시 전송되기 전에 응답을 처리할 수 있습니다.

AgentCore 런타임(Runtime)은 Starlette의 ASGI 미들웨어(Middleware) 시스템을 사용하여 에이전트(Agent) 코드를 수정하지 않고도 로깅, 인증(Authentication), 헤더 조작과 같은 교차 기능을 추가할 수 있습니다.

## 튜토리얼 세부사항

| 정보                | 세부사항                                                                         |
|:--------------------|:--------------------------------------------------------------------------------|
| 튜토리얼 유형       | 미들웨어(Middleware) 구현                                                        |
| 에이전트(Agent) 유형 | 단일                                                                            |
| 에이전트(Agent) 프레임워크(Framework) | Strands Agents                                                    |
| LLM 모델            | Anthropic Claude Haiku 4.5                                                      |
| 튜토리얼 구성 요소   | 미들웨어(Middleware), 요청/응답 처리, AgentCore 런타임(Runtime), Strands Agent 및 Amazon Bedrock 모델 |
| 튜토리얼 분야       | 범용                                                                            |
| 예제 복잡도         | 중급                                                                            |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK 및 boto3                                     |

## 미들웨어(Middleware)란?

미들웨어(Middleware)는 애플리케이션을 래핑하여 요청과 응답을 가로채는 ASGI 컴포넌트입니다. 각 미들웨어(Middleware)는 다음을 수행할 수 있습니다:

- 들어오는 요청 검사 또는 수정
- 에이전트(Agent) 실행 전 로직 실행
- 나가는 응답 검사 또는 수정
- 헤더, 로깅 또는 메트릭 추가
- 인증(Authentication) 또는 속도 제한 처리

미들웨어(Middleware)는 지정된 순서대로 위에서 아래로 평가되며, 각 레이어가 다음 레이어를 래핑합니다.

## 작동 방식

BedrockAgentCoreApp은 초기화 중에 `middleware` 매개변수를 수락합니다:

```python
from bedrock_agentcore import BedrockAgentCoreApp
from starlette.middleware import Middleware
from starlette.middleware.base import BaseHTTPMiddleware

app = BedrockAgentCoreApp(
    middleware=[
        Middleware(CustomMiddleware),
    ]
)
```

각 미들웨어(Middleware)는 요청과 다음 레이어를 호출하는 `call_next` 함수를 받는 비동기 `dispatch` 메서드를 구현합니다.

## 튜토리얼 주요 기능

* **BaseHTTPMiddleware**: 요청/응답 인터페이스를 사용하여 미들웨어(Middleware) 작성
* **사용자 정의 헤더**: 추적 및 디버깅 헤더 추가
* **요청 타이밍**: 처리 시간 측정
* **로깅**: 중앙 집중식 요청/응답 로깅
* **체이닝**: 여러 미들웨어(Middleware) 컴포넌트 스택
* **테스팅**: TestClient를 사용한 로컬 테스트

## 사용 사례

- **로깅**: 요청/응답 타이밍 및 메타데이터 추적
- **인증(Authentication)**: API 키 또는 토큰 검증
- **헤더**: 추적을 위한 사용자 정의 헤더 추가
- **메트릭**: 성능 데이터 수집
- **CORS**: 교차 출처 요청 처리
- **속도 제한**: 요청 빈도 제어

