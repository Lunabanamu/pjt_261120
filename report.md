# Report Agent 프로젝트 보고서
작성자: 신민경
일자: 2026년 11월 20일
프로젝트: Report Agent Basic

## 프로젝트 결론

보고서 데이터를 저장하고 관리하는 기본 백엔드에 AI 검색과 요약 기능을 결합한 시스템이다. 전체 흐름은 보고서 데이터 관리 → E5 임베딩 색인 → Hybrid 검색 → LLM 원문 요약 → Agent action 실행으로 정리된다.

## 구현 범위
- 6종 보고서 유형의 등록·조회·수정·삭제(CRUD)
- PostgreSQL 및 pgvector 기반 데이터 저장
- intfloat/multilingual-e5-small을 이용한 384차원 임베딩 생성
- Keyword 검색 우선 및 Semantic 검색 대체 경로를 사용하는 Hybrid 검색
- 실제 LLM API를 이용한 보고서 원문 요약
- Basic Agent의 명시적 action 기반 조회·검색·요약 실행
- Health, 카테고리, CRUD, 검색, 요약, Agent를 포함한 API-00~09
- 계약 테스트, 실제 DB 테스트 및 Postman 통합 검증 절차

## API 구성
| API ID | Method | 기능 | 주요 결과 |
|---|---|---|---|
| API-00 | `GET` | 서버·DB·pgvector 연결 확인 | DB 및 vector 준비 상태 |
| API-01 | `GET` | 6개 카테고리 조회 | 카테고리 목록 |
| API-02 | `POST` | 보고서 등록 | 생성 ID, revision, 색인 상태 |
| API-03 | `GET` | 단건 조회 | 공통 필드와 상세 데이터 |
| API-04 | `GET` | 제목 기반 목록 조회 | items, total, limit, offset |
| API-05 | `PUT` | 보고서 수정 | revision 및 임베딩 갱신 여부 |
| API-06 | `DELETE` | 보고서 삭제 | 204 No Content |
| API-07 | `POST` | Keyword + Semantic 검색 | best_match 또는 not_found |
| API-08 | `POST` | LLM 원문 요약 | 요약문 및 source revision |
| API-09 | `POST` | Agent action 실행 | action, tool_calls, data, status |