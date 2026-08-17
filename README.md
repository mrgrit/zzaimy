# 「짜이미」 ZZAIMY

> **공고를 날실로, 우리의 실적을 씨실로.**

영남이공대학교 **국고사업 계획서 작성 지원 시스템**. 교내에 축적된 수만 장 규모의 국고사업 문서(공고·지침, 과거 사업계획서, 프로그램 결과보고서, 지출 문서)를 실적 데이터베이스와 학습 데이터로 전환하여, 새 공고가 나왔을 때 계획서 초안 작성 기간을 단축하는 것을 목표로 한다.

- 문장을 창조하지 않고 **이미 존재하는 근거를 엮어 배치**한다. 수치는 생성하지 않고 **인출**한다.
- 문서에 개인정보·기관 내부정보가 포함되므로 **완전 온프레미스**(NVIDIA DGX Spark)를 전제로 한다. 외부 API로의 문서 전송은 원칙적으로 불가.
- 오픈소스 베이스 모델을 교내 문서로 파인튜닝한 특화 모델 4종(`ZZAIMY-Embed` / `ZZAIMY-Rerank` / `ZZAIMY-Writer` / `ZZAIMY-Extract`)을 vLLM으로 서빙한다.

## 현재 상태

| 단계 | 내용 | 상태 |
|---|---|---|
| Phase 0 | 기술 리서치 (14개 주제 + 종합) | ✅ 완료 (2026-08) |
| — | 시스템 조감도 · 사업기획서 | ✅ 완료 |
| Phase 1 | 아키텍처 설계 | ⏸ 승인 대기 |

각 Phase는 종료 시 사용자 검토·승인 후 다음 단계로 진행한다(브리프 12장).

## 문서 지도

### 기준 문서

| 문서 | 내용 |
|---|---|
| [PROJECT_BRIEF.md](PROJECT_BRIEF.md) | 프로젝트 브리프 v2 — 모든 판단의 기준 문서 |

### 기획 문서

| 문서 | 내용 |
|---|---|
| [docs/business-plan.md](docs/business-plan.md) | 사업기획서 — 배경, 2025~26 국내외 사례 12건, 필요성, 기대효과, 일정(2026.9~2027.6), 인력·비용, 요구사항, 후속 확장 계획 |
| [research/02-system-overview.md](research/02-system-overview.md) | 시스템 조감도 — 구성·프로세스·유즈케이스 |

### Phase 0 리서치

| 문서 | 주제 |
|---|---|
| [research/00-summary.md](research/00-summary.md) | **종합 요약 — 여기부터 읽을 것** |
| [research/01-A-document-parsing.md](research/01-A-document-parsing.md) | 문서 파싱 (MinerU / Docling) |
| [research/01-B-rag-framework.md](research/01-B-rag-framework.md) | RAG 프레임워크 |
| [research/01-C-knowledge-graph.md](research/01-C-knowledge-graph.md) | 지식그래프 (미도입 결론) |
| [research/01-D-embedding-reranker.md](research/01-D-embedding-reranker.md) | 임베딩·리랭커 |
| [research/01-E-embedding-training.md](research/01-E-embedding-training.md) | 임베딩 학습 |
| [research/01-F-llm-finetuning.md](research/01-F-llm-finetuning.md) | LLM 파인튜닝 |
| [research/01-G-hybrid-search.md](research/01-G-hybrid-search.md) | 하이브리드 검색 |
| [research/01-H-vector-db.md](research/01-H-vector-db.md) | 벡터 DB |
| [research/01-I-structured-extraction.md](research/01-I-structured-extraction.md) | 정형 추출 |
| [research/01-J-llm-serving.md](research/01-J-llm-serving.md) | LLM 서빙 (vLLM) |
| [research/01-K-generation-models.md](research/01-K-generation-models.md) | 생성 모델 선정 |
| [research/01-L-evaluation.md](research/01-L-evaluation.md) | 평가 체계 |
| [research/01-M-pii.md](research/01-M-pii.md) | 개인정보 비식별화 |
| [research/01-N-prior-art.md](research/01-N-prior-art.md) | 유사 선행 사례 |
| [research/50-model-track.md](research/50-model-track.md) | 모델 트랙 종합 |
| [research/99-open-questions.md](research/99-open-questions.md) | 미해결 질문 (사용자 답변 필요) |

## 웹 열람

렌더링된 버전(다이어그램 포함)은 아래에서 볼 수 있다.

- 📚 [문서고 — 전체 문서 통합 열람](https://claude.ai/code/artifact/0568068a-58c5-4e2c-a19b-4527bc9ff353)
- 🧶 [사업기획서 (디자인판)](https://claude.ai/code/artifact/359584f8-0b05-40c8-877e-7a8ae3426210)
- 🧵 [시스템 조감도 (디자인판)](https://claude.ai/code/artifact/8acedbb9-f831-46ea-b07f-3f2cba88bf1b)

## 기술 스택 요약 (Phase 0 추천안)

| 영역 | 선정 |
|---|---|
| 문서 파싱 | MinerU ↔ Docling bake-off 후 결정 |
| 임베딩 / 리랭커 | KURE-v1 → `ZZAIMY-Embed` / bge-reranker-v2-m3 → `ZZAIMY-Rerank` |
| 생성 | Qwen3.5-35B-A3B (MoE) → `ZZAIMY-Writer` / Qwen3-4B → `ZZAIMY-Extract` |
| 서빙 | vLLM (aarch64, LoRA 동적 로딩) + xgrammar |
| 검색 | Qdrant + Kiwi 하이브리드 |
| 실적 DB | PostgreSQL (실적 카드) |
| 개인정보 | Presidio + 한국형 recognizer |

상세 근거와 대안 비교는 [research/00-summary.md](research/00-summary.md) 참조. 하드웨어 1차 필터는 aarch64(DGX Spark GB10) 호환 여부다.

## 명명 규칙

「짜임」 + 「이」 → [짜이미]. Y는 영남이공대학교의 이니셜을 겸한다. `ZZAIMY` / `zzaimy` 외의 표기는 사용하지 않는다.
