# Evidências - Eixo D: Plano de Evolução da Qualidade

Arquivos do repositório referenciados pelas propostas do plano de evolução (itens D1 a D5). As evidências aqui apontam os pontos exatos que o plano propõe criar ou modificar.

* **D1 / D2 / D3 — Priorização, camadas de foco e implantação gradual:**
  * Lacunas críticas que orientam a priorização: `src/vanna/core/recovery/base.py`, `src/vanna/core/agent/agent.py`.
  * Infraestrutura reaproveitável para iniciar sem travar o desenvolvimento: `MockLlmService` (`tests/test_llm_context_enhancer.py`) e runner SQLite (fixture em `tests/conftest.py`).
  * *Referência no Doc Final:* Eixo D, itens D1, D2 e D3.

* **D4 — Automação (pipeline e política mínima):**
  * Gatilho a adicionar (`on: pull_request`): `.github/workflows/tests.yml`.
  * Novo ambiente de regressão sem credenciais a criar: `tox.ini`.
  * Gate de cobertura a ativar (`pytest-cov` já é dependência declarada): `pyproject.toml`, seção `[tool.pytest.ini_options]`.
  * Publicação condicionada a testes: `.github/workflows/python-publish.yaml`.
  * *Referência no Doc Final:* Eixo D, item D4.

* **D5 — Critérios de sucesso (entrada e saída):**
  * Base para os critérios de cobertura: módulos centrais em `src/vanna/core/`.
  * Testes hoje ignorados (skipped) por falta de credencial, alvo dos critérios de saída: `tests/test_agents.py`, `tests/conftest.py`.
  * *Referência no Doc Final:* Eixo D, item D5.
