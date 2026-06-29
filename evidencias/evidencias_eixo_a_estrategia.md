# Evidências - Eixo A: Estratégia Atual de Testes

Este documento reúne os links e caminhos reais do repositório que comprovam como o projeto organiza e executa sua validação, conforme analisado no Eixo A do documento final.

* **A1 — Estrutura dedicada a testes:**
  * Pasta de testes: [https://github.com/vanna-ai/vanna/tree/main/tests](https://github.com/vanna-ai/vanna/tree/main/tests)
  * Configuração do Pytest (`testpaths`, `python_files`, `python_classes`, `python_functions`): `pyproject.toml`, seção `[tool.pytest.ini_options]`.
  * Ferramentas de apoio configuradas: Pytest, Pytest-Cov, Tox, Ruff e MyPy (`pyproject.toml` e `tox.ini`).
  * *Referência no Doc Final:* Eixo A, item A1.

* **A2 — Tipos de teste encontrados:**
  * Unitários: `tests/test_agent_memory.py`, `tests/test_tool_permissions.py`.
  * Integração: `tests/test_database_sanity.py`, `tests/test_gemini_integration.py`.
  * Fluxo / end-to-end: `tests/test_agents.py`, `tests/test_workflow.py`.
  * *Achado:* ausência de suíte formal de regressão e de testes manuais documentados.
  * *Referência no Doc Final:* Eixo A, item A2.

* **A3 — Instruções de execução dos testes:**
  * Comandos de teste no `CONTRIBUTING.md` (ex.: `tox -e py311-unit`, `pytest tests/test_tool_permissions.py -v`): [https://github.com/vanna-ai/vanna/blob/main/CONTRIBUTING.md](https://github.com/vanna-ai/vanna/blob/main/CONTRIBUTING.md)
  * Workflow de automação: `.github/workflows/tests.yml`.
  * *Achado:* o README principal não traz uma seção de execução de testes.
  * *Referência no Doc Final:* Eixo A, item A3.

* **A4 — Integração com CI/CD:**
  * Pasta de workflows: [https://github.com/vanna-ai/vanna/tree/main/.github/workflows](https://github.com/vanna-ai/vanna/tree/main/.github/workflows)
  * Workflow "Basic Integration Tests" (`tests.yml`) dispara **apenas em `push` na branch `main`**, não em Pull Requests.
  * Workflow "Upload Python Package" (`python-publish.yaml`) publica no PyPI **sem executar testes** antes.
  * *Referência no Doc Final:* Eixo A, item A4.

* **A5 — Indicadores de cobertura e qualidade:**
  * README apresenta apenas badges de licença (MIT) e estilo de código (black): [https://github.com/vanna-ai/vanna/blob/main/README.md](https://github.com/vanna-ai/vanna/blob/main/README.md)
  * *Achado:* ausência de badge ou relatório de cobertura (Codecov, Coveralls, SonarCloud).
  * *Referência no Doc Final:* Eixo A, item A5.
