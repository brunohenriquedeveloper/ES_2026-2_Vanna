# Eixo V — Qualidade de Software (GQA)

> **Projeto analisado:** [vanna-ai/vanna](https://github.com/vanna-ai/vanna)  
> **Alinhamento:** CMMI PPQA • MPS.BR Garantia da Qualidade

---

## 1. Auditoria do CONTRIBUTING.md

> ⚠️ **Nota:** O repositório foi arquivado em março de 2026 e está em modo somente leitura. Contribuições de código não são mais aceitas.

O `CONTRIBUTING.md` do projeto é bem estruturado e cobre os principais aspectos do processo de contribuição:

| Critério | Situação | Evidência |
|---|---|---|
| Pré-requisitos do ambiente descritos | ✅ Conforme | Python 3.11+, Git, GitHub account |
| Setup de ambiente virtual documentado | ✅ Conforme | Seção "Ambiente Virtual" |
| Padrão de commits definido | ✅ Conforme | `feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:` |
| Processo de Pull Request descrito | ✅ Conforme | Seção "Pull Requests" |
| Checklist pré-PR documentado | ✅ Conforme | `ruff format`, `mypy`, `tox` obrigatórios |
| Docstrings obrigatórias | ✅ Conforme | "Adicione docstrings em funções e classes públicas" |
| Guia de testes presente | ✅ Conforme | Seção "Testes" com boas práticas |
| Template de PR com checklist automático | ❌ Gap | Não há `.github/PULL_REQUEST_TEMPLATE` |
| Mínimo de aprovações obrigatório | ❌ Gap | Não definido em branch protection rules |
| Definição de "Done" formal | ❌ Gap | Sem critérios objetivos de aceitação |

---

## 2. Ferramentas de Análise Estática

O projeto adota ferramentas modernas de qualidade para Python, integradas ao fluxo de contribuição via `CONTRIBUTING.md` e pre-commit hooks.

| Ferramenta | Função | Status | Observação |
|---|---|---|---|
| Ruff | Linting + formatação | ✅ Presente | Substitui Flake8, isort e Black |
| Mypy (strict) | Verificação de tipos | ✅ Presente | Obrigatório para código novo |
| pytest-cov | Cobertura de testes | ⚠️ Parcial | Sem meta mínima definida |
| Pre-commit hooks | Automação de checks | ✅ Presente | `.pre-commit-config.yaml` na raiz |
| Tox | Execução padronizada | ✅ Presente | `tox.ini` + integração com CI |
| CodeQL | Segurança (SAST) | ❌ Ausente | Nenhum workflow configurado |
| SonarQube/SonarCloud | Qualidade consolidada | ❌ Ausente | Sem dashboard de métricas |
| Bandit | Segurança Python | ❌ Ausente | Crítico para projetos com SQL dinâmico |

> **⚠️ Ponto de atenção:** A ausência de ferramentas SAST (CodeQL e Bandit) é especialmente relevante no Vanna, pois o projeto executa queries SQL geradas dinamicamente por LLMs — cenário com risco inerente de SQL injection indireta.

---

## 3. Dívida Técnica

### 3.1 Visão Geral

A transição para a versão 2.0 — reescrita arquitetural completa lançada em fevereiro de 2026 — é o maior indicador de dívida acumulada. O projeto passou a manter código legado e novo simultaneamente, elevando a complexidade de manutenção.

| Indicador | Valor | Impacto |
|---|---|---|
| Issues abertas | 227 | 🔴 Alto |
| Bugs não corrigidos (label `bug`) | 99 | 🔴 Alto |
| Pull Requests aguardando revisão | 64 | 🟡 Médio |
| Releases publicadas | 78 | 🟢 Positivo — ritmo ativo |
| Contribuidores | 72 | 🟢 Positivo — base ampla |

> A quantidade de issues abertas não indica abandono — o Vanna possui 22.700+ estrelas e comunidade ativa. Indica que a capacidade de resolução não acompanha o ritmo de crescimento, o que é característico de dívida técnica acumulada.

---

### 3.2 Regras de Qualidade Ignoradas no Ruff

O `pyproject.toml` contém um bloco `[tool.ruff.lint] ignore` com 25+ regras desabilitadas, várias com justificativas explícitas como `"legacy compatibility"` e `"intentional in legacy code"` — evidência objetiva de dívida técnica intencional.

| Regra | Problema | Justificativa declarada |
|---|---|---|
| `F401` | Imports não utilizados (código morto) | Não declarada |
| `F841` | Variáveis declaradas e nunca usadas | Não declarada |
| `N801–N806` | Nomes fora do padrão PEP8 | `"legacy compatibility"` |
| `B024` | Classe abstrata sem `@abstractmethod` | Não declarada |
| `B027` | Método vazio sem decorador abstrato | `"intentional in legacy code"` |
| `B904` | `raise` sem `from` dentro de `except` | `"intentional in legacy code"` |
| `E402` | Import fora do topo do arquivo | Não declarada |

---

### 3.3 Dívida de Arquitetura — Coexistência Legada

Com o Vanna 2.0, o projeto mantém duas arquiteturas conectadas por um `LegacyVannaAdapter`:

- **Testes duplicados:** `test_legacy_adapter.py` mantido paralelamente à suíte nova
- **Documentação paralela:** `MIGRATION_GUIDE.md` e `README_LEGACY.md` precisam de atualização contínua
- **Risco de divergência:** comportamentos podem diferir entre a API legada e a nova

---

### 3.4 Dívida de Dependências

**Dependências principais sem versão mínima:**  
`pandas`, `PyYAML`, `plotly`, `sqlparse`, `sqlalchemy` e `requests` estão declaradas sem versão mínima no `pyproject.toml`. Uma atualização breaking pode introduzir regressões silenciosas — o SQLAlchemy 2.0, por exemplo, introduziu mudanças incompatíveis que afetariam diretamente o núcleo do projeto.

**Conflito de versão no módulo Oracle:**  
```toml
oracle = ["oracledb", "chromadb<1.0.0"]
```
O módulo Oracle está preso no `chromadb<1.0.0`, enquanto o restante do projeto exige `chromadb>=1.1.0`. Quem instalar o módulo Oracle receberá versões conflitantes da mesma biblioteca.

---

### 3.5 Ausência de Meta de Cobertura de Testes

O `pytest-cov` está presente como dependência, mas **não há `fail_under` configurado** no `pyproject.toml` nem nos scripts de CI. Na prática, um PR pode ser aprovado mesmo reduzindo significativamente a cobertura existente.

---

## 4. Avaliação CMMI (PPQA) e MPS.BR (GQA)

| Critério | Situação | Nível CMMI/MPS.BR |
|---|---|---|
| `CONTRIBUTING.md` estruturado | ✅ Conforme | Nível 2 — Gerenciado |
| Linting e formatação automatizados (Ruff) | ✅ Conforme | Nível 2 — Gerenciado |
| Type checking obrigatório (Mypy strict) | ✅ Conforme | Nível 3 — Definido |
| Pre-commit hooks configurados | ✅ Conforme | Nível 2 — Gerenciado |
| Ferramenta de cobertura presente (pytest-cov) | ⚠️ Parcial | Gap — sem meta mínima |
| Análise de segurança automatizada (SAST) | ❌ Ausente | Abaixo do Nível 2 |
| Gestão formal de dívida técnica | ❌ Ausente | Abaixo do Nível 2 |
| Dependências versionadas corretamente | ⚠️ Parcial | Abaixo do Nível 2 |
| Meta mínima de cobertura de testes | ❌ Ausente | Abaixo do Nível 2 |

---

## 6. Conclusão

O projeto Vanna apresenta práticas de qualidade acima da média para projetos open source — especialmente o uso de Mypy strict e a configuração detalhada do Ruff. No entanto, a ausência de análise de segurança automatizada, a inexistência de meta mínima de cobertura e a gestão informal da dívida técnica acumulada na transição para a versão 2.0 são lacunas relevantes para um processo maduro alinhado ao CMMI/MPS.BR.

Os problemas identificados não são incomuns em projetos open source de crescimento acelerado, mas precisam ser endereçados sistematicamente para que o projeto atinja o nível de maturidade exigido por ambientes corporativos e certificações formais.
