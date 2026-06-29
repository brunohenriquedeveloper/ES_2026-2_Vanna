# Evidências - Eixo C: Lacunas, Riscos e Falhas Potenciais

Links e caminhos que comprovam as lacunas de cobertura e os riscos de regressão identificados no Eixo C.

* **C1 — Funcionalidades críticas sem testes:**
  * `ErrorRecoveryStrategy` (retry com backoff exponencial) sem nenhum teste: `src/vanna/core/recovery/base.py`.
  * *Verificação:* `grep -rEn "ErrorRecovery|recovery|retry" tests/` retorna 0 ocorrências.
  * Único fluxo de geração de SQL ponta a ponta depende de credenciais reais e é ignorado (skipped) no CI: `tests/test_agents.py` (com `tests/conftest.py`).
  * Outros módulos sem teste: streaming (`src/vanna/servers/fastapi/routes.py`), geração de gráficos (`src/vanna/integrations/plotly/chart_generator.py`), execução de código (`src/vanna/tools/python.py`).
  * *Referência no Doc Final:* Eixo C, item C1.

* **C4 — Proteção contra regressões:**
  * Workflow `tests.yml` dispara só em `push` na `main`, nunca em Pull Requests: [https://github.com/vanna-ai/vanna/tree/main/.github/workflows](https://github.com/vanna-ai/vanna/tree/main/.github/workflows)
  * `tox` env `py311-unit` cobre apenas 4 arquivos (`tox.ini`): `test_tool_permissions`, `test_llm_context_enhancer`, `test_workflow`, `test_memory_tools`.
  * *Verificação:* `grep -nE "addopts|--cov|fail_under" pyproject.toml tox.ini` retorna 0 ocorrências (sem limiar mínimo de cobertura).
  * *Referência no Doc Final:* Eixo C, item C4.

* **C5 — Detecção de bugs severos:**
  * Cenário 1: `Agent._build_llm_request` (montagem do payload) em `src/vanna/core/agent/agent.py`, sem validação de conteúdo do array de mensagens.
  * Cenário 2: loop de tool calls em `src/vanna/core/agent/agent.py` — o ramo `is_tool_call() == True` não é exercitado pelos testes unitários.
  * Cenário 3: `ErrorRecoveryStrategy` em `src/vanna/core/recovery/base.py`, sem testes de retry/backoff/fallback.
  * *Referência no Doc Final:* Eixo C, item C5.
