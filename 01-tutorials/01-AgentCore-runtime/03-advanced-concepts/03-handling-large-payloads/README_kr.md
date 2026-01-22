# AgentCore 런타임(Runtime)에서 대용량 멀티모달 페이로드(Payload) 처리

## 개요

이 튜토리얼은 Amazon Bedrock AgentCore 런타임(Runtime)이 Excel 파일과 이미지와 같은 멀티모달 콘텐츠를 포함하여 최대 100MB의 대용량 페이로드(Payload)를 처리하는 방법을 보여줍니다. AgentCore 런타임(Runtime)은 리치 미디어 콘텐츠와 대규모 데이터셋을 원활하게 처리하도록 설계되었습니다.

### 튜토리얼 세부사항

| 정보                | 세부사항                                                     |
|:--------------------|:------------------------------------------------------------|
| 튜토리얼 유형       | 대용량 페이로드(Payload) 및 멀티모달 처리                      |
| 에이전트(Agent) 유형 | 단일                                                        |
| 에이전트(Agent) 프레임워크(Framework) | Strands Agents                                |
| LLM 모델            | Anthropic Claude Haiku 4.5                                   |
| 튜토리얼 구성 요소   | 대용량 파일 처리, 이미지 분석, Excel 데이터 처리               |
| 튜토리얼 분야       | 데이터 분석 및 멀티모달 AI                                    |
| 예제 복잡도         | 중급                                                         |
| 사용 SDK            | Amazon BedrockAgentCore Python SDK                           |

### 주요 기능

* **대용량 페이로드(Payload) 지원**: 최대 100MB 크기의 파일 처리
* **멀티모달 처리**: Excel 파일, 이미지, 텍스트를 동시에 처리
* **데이터 분석**: 구조화된 데이터와 시각적 콘텐츠에서 인사이트 추출
* **Base64 인코딩**: JSON 페이로드(Payload)를 통한 바이너리 데이터의 안전한 전송

