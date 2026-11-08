# ProxyTrace

SEC EDGAR에 올라온 위임장(DEF 14A)에서 주주제안과 회사 반론을 뽑아, 검증 가능한 주장(claim) 단위로 정리하고 ESG 관점에서 우선순위를 매기는 파이프라인.

티커 하나만 넣으면 전체 과정이 순서대로 돌아가도록 노트북을 나눠뒀어요.

## 파이프라인

| 노트북 | 하는 일 | 결과 |
| --- | --- | --- |
| [01_fetch_proxy.ipynb](01_fetch_proxy.ipynb) | SEC EDGAR API로 최신/직전 위임장 수집 | `data/raw/{TICKER}/{제출일}.htm`, `manifest.json` |
| [02_clean_text.ipynb](02_clean_text.ipynb) | HTML을 쪽 번호가 붙은 깨끗한 텍스트로 정리 | `data/clean/{TICKER}/{제출일}_pages.json`, `.txt` |
| [03_split_proposals.ipynb](03_split_proposals.ipynb) | 주주제안마다 주주의 말/회사의 말로 분리 | `data/parsed/{TICKER}/{제출일}_proposals.json` |
| [04_extract_claims.ipynb](04_extract_claims.ipynb) | GPT로 검증할 주장 추출 (quote, claim_ko, kind, checkable, topic, page_no 등) | `data/claims/{TICKER}/{제출일}_claims.json`, `.csv` |
| [05_esg_priority.ipynb](05_esg_priority.ipynb) | ESG pillar/SASB 이슈/중대성 태깅 + 판별 순서(priority) 산정 | `data/claims/{TICKER}/{제출일}_claims_esg.json`, `.csv` |

각 노트북은 앞 단계의 결과 파일을 읽어서 동작하므로, 다른 기업을 분석할 때는 01 노트북의 티커만 바꾸면 됩니다.

## 준비

- OpenAI API 키가 필요합니다 (04 노트북). `.env`에 `OPENAI_API_KEY`를 넣고 실행하세요. `.env`는 git에 포함되지 않습니다.
- 원본 위임장 PDF(`Proxy_Statement_2026.pdf`)는 용량 때문에 git에서 제외했습니다.
