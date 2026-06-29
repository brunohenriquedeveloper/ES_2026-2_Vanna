# Evidências - Eixo B: Qualidade Técnica dos Testes

Caminhos e exemplos reais do repositório utilizados para avaliar a qualidade da suíte de testes existente no Eixo B.

* **B1 — Clareza dos nomes e comportamento observável:**
  * Arquivos analisados: `tests/test_gemini_integration.py`, `tests/test_llm_context_enhancer.py`.
  * Exemplos de nomenclatura: `test_gemini_initialization_without_key`, `test_gemini_basic_request`, `test_custom_enhancer_system_prompt_is_called`, `test_no_enhancer_means_no_enhancement`.
  * *Achado:* prefixo `test_` seguido da descrição do cenário, cabeçalhos por arquivo e docstrings por método.
  * *Referência no Doc Final:* Eixo B, item B1.

* **B2 / B4 / B5 — Isolamento, relevância e manutenção:**
  * Configuração e fixtures compartilhadas: `tests/conftest.py`.
  * Dublê de LLM (`MockLlmService`) para isolar dependência externa: `tests/test_llm_context_enhancer.py`.
  * *Referência no Doc Final:* Eixo B, itens B2, B4 e B5.

* **B3 — Uso de mocks, stubs e fixtures:**
  * `MockLlmService` em `tests/test_llm_context_enhancer.py`; runner SQLite usado como fixture nos testes de fluxo.
  * *Referência no Doc Final:* Eixo B, item B3.
