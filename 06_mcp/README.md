MCP(Model Context Protocol)
AI 모델(LLM)이 외부 데이터, 도구, 시스템과 "표준 방식"으로 연결되도록 만드는 프로토콜입니다.

1. 기존 방식
- LLM이 외부 데이터에 접근하려면
    1. API 직접 호출 코드 작성
    2. DB 연결 코드 작성
    3. 툴마다 별도 구현 필요
- 툴마다 다르게 연결하고 사용해야 하는 비표준 구조

2. MCP 방식
- 중간 표준 인터페이스 역할을 함
- [LLM] <-> [MCP] <-> [Tools / DB / API ...]
- LLM은 MCP 규칙만 알면 됨
- 어떤 툴이든 동일한 방식으로 호출 가능

    Tool: LLM이 사용할 기능(CSV 분석, DB 조회, 웹 검색, 코드 실행...)

        @mcp.tool() # MCP에서 Tool 등록
        def func1():
            ...

    Prompt: LLM에게 어떻게 행동할지 알려주는 규칙

        from mcp.server.fastmcp.prompts import base # 프롬프트도 구조화해서 관리

    Server: MCP 서버가 전체를 관리

        mcp = FastMCP("DataAnalysis") # 역할: Tool 등록 / 요청 처리 / LLM과 통신



3. MCP 흐름
- 사용자 질문
    "CSV 파일에서 XX컬럼의 평균값을 알려줘"

- LLM 판단
    "앗! 이건 Tool이 필요하네"

- MCP 통해 Tool 호출
    describe_column(csv, "XX컬럼")

- 결과 반환
    평균: XX.XX

- LLM이 자연어로 정리
    최종 답변 생성



4. CSV 파일을 읽어서 분석해주는 MCP 서버