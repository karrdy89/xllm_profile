# :pushpin: LLM 기반 서비스 플랫폼 구축
>LLM 기반 서비스 및 LLM 서비스 플랫폼 개발

</br>

## 1. 수행 기간
- 2024년 6월 - 진행중

## 2. 사용 기술
#### `Back-end`
  - Huggingface
  - Transformers
  - Pytorch
  - Python
  - MySQL
  - FastAPI
  - Docker
  - Opensearch
  - NGINX
  - Langchain
  - Langgraph
  - vLLM
  - paddlestructure
  - ONNX
  - NLP

## 3. 데모
업로드 예정

## 4. 아키텍처
![Image](https://github.com/user-attachments/assets/0e6017c6-2384-4a7c-8a82-3c6b0abc56f2)


## 5. 핵심 서비스(RAG)
  

<details>
<summary><b>펼치기</b></summary>
<div markdown="1">

### 5.1. 설명
지식베이스 내의 내용을 토대로 사용자의 질문에 응답을 해주는 ReAct기반 RAG agent

### 5.2. Agent Flow
![Image](https://github.com/user-attachments/assets/75cba692-539a-4857-b5de-245d7bebaf0e)

- **RAG Agnet**
  - ReAct 기반 Agent로써, EXAONE-3.5-7.8B 모델을 DeepSeek-R1 모델에서 distillation 하여 Reasoning 성능을 향상 시킨 모델을 기반으로 RAG 태스크에 맞게 Fine-tuning 한 모델을 사용
  - Action과 Final Answer 시 Sentinel(Action 수행 시와 Final Answer 생성 시 <break/>)을 출력하게끔 하여 해당 토큰을 기반으로 토큰 생성을 멈추고 이후 단계로 이행
  - 멀티턴 대화에서 User의 말에 즉답을 하려면 어떻게 해야 하는지에 대하여 단계별로 계획을 세운 이후 수행
  - 주어진 Action은 Knowledge Search와 Chat 두가지로 Knowledge Searchs는 지식베이스에서 검색을 하는 tool, tool을 사용할 필요가 없을 경우에 사용하는 chat Action으로 기존의 assistant가 대화를 이어서 진행

- **Query Processing**
  - 복합 쿼리 분할
  - 시간정보(오늘, 어제)가 포함될 경우 현재 날짜 기준으로 치환
  - 수사를 제외하고 핵심 검색어만 추출
  - 검색 필터 추출(문서 명을 명시한 경우)
    
- **Retrieval Action**
  - 쿼리들에 대한 검색 수행(기본적으로 Semantic Search와 Keword search에 대해 RRF를 수행 후 ReRanking)
    
- **Validate**
  - 검색된 내용에 질문에 대한 답변이 포합되어 있는지 검증

### 5.3. Event Handling

에이전트에서 발생되는 상황을 추적하고, 현재 상황을 Target System에 실시간으로 보내는 것이 필요하며, 에이전트 상태 업데이트 시 Event Manager를 통하여 발생되는 Event에 대한 처리 제공

### 5.4. Connection Handling

Agentic RAG 서비스는 웹소켓 기반으로 제공되며 Connection들에 관하여 세션관리 제공

</div>
</details>

</br>

## 6. 수행 내용
  - 서버 및 시스템 구축
  - 모델 분산 학습/배포 환경 구축(Ray cluster 기반)
  - 인증 및 보안 적용
  - LLM/VLM  배포 플랫폼 개발(vLLM async engine기반)
    - Embedding/Ranking 모델 배포 지원
  - Document Parsing 서버 구축
    - structured HTML parser 개발
    - pdf parser 구축(unstructured, pymupdf 기반)
  - Vector DB 구축(opensearch 기반)
    - 클러스터 구성
    - 인덱스 설계
  - LLM 서비스 엔진 개발
    - Namespace기반 논리적 서비스 분할(멀티테넌시) 환경 구축
    - 프롬프트 생성 및 관리 모듈 개발
    - 서비스 생성 및 관리 모듈 개발
    - Document ETL 파이프라인 개발
    - LLM Provider에 관계 없이 호출 가능한 LLM Adapter개발
    - 저수준 RDB, Vector DB 커넥터를 wrapping 하는 고수준 인터페이스 개발
    - 저수준 Langchain/Langgraph를 wrapping 하는 고수준 인터페이스 개발
    - 비동기 Task를 위한 TaskManager 모듈 개발
    -  다중 Agent 이벤트 처리를 위한 메시징 모듈 개발
    -  RAG 서비스 성능 측정 모듈 개발(ragas 기반)
    -  Retrieve 모듈 개발
    -  Agent Routing 모듈 개발
  - LLM 서비스 개발
    - KMS와 연계된 ReAct 에이전트 RAG 챗봇
    - 상담 분석 서비스(분류, 키워드 추출, 요약, 품질평가)
    - 챗봇 학습용 데이터 생성 서비스(Q&A 세트 생성, 질문 확장, 의도 분류)
    - 문서 요약 서비스
    - Q&A 생성 서비스
    - 유사 문서 검색 서비스
    - 유사 Q&A 추천 서비스
 - 서비스 고도화
   - Prompt Engineering
   - 도메인 특화 Embedding 모델 개발
   - Ranking 모델 개발
   -  도메인/태스크/Reasoning 특화 LLM 개발(SFT, DPO, GRPO, Distillation 등)
 - 시스템 요소 컨테이너화 및 배포
