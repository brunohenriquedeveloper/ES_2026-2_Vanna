# Plano de Melhoria de Processo

**Eixo: Estratégia e Plano de Melhoria**  
**Autor:** Flávio Henrique de Jesus Cruz — 202400017782  
**Disciplina:** Engenharia de Software (COMP0503) — UFS  
**Projeto Auditado:** [Vanna AI](https://github.com/vanna-ai/vanna)

---

## 1. Introdução

Este documento apresenta o Plano de Melhoria de Processo formulado como corolário da auditoria técnica realizada sobre o projeto open source **Vanna AI** — um ecossistema Text-to-SQL baseado em LLMs com licença MIT.

O plano tem como objetivo estratégico retificar as lacunas organizacionais identificadas nos cinco eixos de auditoria, com foco nos parâmetros elementares de certificação do **MPS.BR Nível G** (Gerenciado), referenciando também o **CMMI-DEV v2.0 Nível 2**.

O Vanna AI está em um momento crítico de transição arquitetural — a reescrita completa para a **Versão 2.0**, que introduz um framework baseado em agentes e mantém um `LegacyVannaAdapter` para compatibilidade retroativa. É exatamente neste momento que riscos de regressão e ausência de rastreabilidade são mais perigosos, tornando este plano urgente e estrategicamente oportuno.

---

## 2. Síntese dos Gaps Identificados

A auditoria revelou que o Vanna AI possui bases técnicas robustas (linting, type checking, CI automatizado), mas carece de fundamentos gerenciais elementares. A tabela abaixo sintetiza os principais gaps por eixo:

| Eixo | Gap Central | Impacto | Nível MPS.BR Atual |
|------|------------|---------|-------------------|
| GPR (Eixo I) | Ausência de Risk Register e Roadmap formal | Alto — APIs externas voláteis sem mitigação | Abaixo do Nível G |
| GRE (Eixo II) | Sem RFCs; rastreabilidade apenas reativa | Alto — mudanças sem controle de impacto | Abaixo do Nível G |
| V&V (Eixo IV) | Sem pipelines defensivas para alucinações SQL | Crítico — SQL inválido executado diretamente | Parcial |
| GQA (Eixo V) | Sem SAST; dívida técnica sem gestão formal | Alto — risco de SQL Injection em produção | Abaixo do Nível 2 |

### Critérios de Priorização

As ações foram priorizadas com base em três critérios:

1. **Alinhamento com MPS.BR Nível G** — processos obrigatórios para o nível de certificação alvo
2. **Impacto imediato na saúde do projeto** — redução mensurável de bugs e retrabalho
3. **Baixo atrito de implementação** — compatível com cultura open source, sem exigir ferramentas pagas ou processos burocráticos

---

## 3. Ação 1 — Gerência de Requisitos com Rastreabilidade Formal (GRE)

### 3.1 Problema Central

A auditoria identificou que o fluxo de requisitos do Vanna opera de forma **puramente reativa**: issues são abertas por usuários, tratadas individualmente pelos 72 contribuidores, e frequentemente resolvidas sem PR formal (ex.: Requisito 2 — integração Gemini, corrigido via commit direto sem revisão).

Evidências concretas coletadas:
- **227 issues abertas** acumuladas no repositório
- **99 bugs não corrigidos** sem priorização formal
- **Issue #986 → Commit direto** (sem PR, sem revisão, sem teste associado)
- Ausência total de processos RFC para novas integrações sistêmicas

Esse cenário configura **baixa maturidade GRE** pois não há:
- Critérios de Aceitação formalizados antes da implementação
- Mapeamento entre Issue → Teste → Código (rastreabilidade bidirecional)
- Processo de aprovação para mudanças arquiteturais relevantes

### 3.2 Medidas Propostas

#### Medida 1.A — Templates Restritivos de Issues

Criar templates obrigatórios em `.github/ISSUE_TEMPLATE/` que forcem o reportador a fornecer informações mínimas antes da triagem:

**`BUG_REPORT.md`** deve conter obrigatoriamente:
- Descrição objetiva do comportamento incorreto
- Passos para reprodução (mínimo 3 etapas)
- Comportamento esperado vs. comportamento atual
- Versão do Vanna, LLM utilizado e banco de dados
- **Critérios de Aceitação da correção** (o que deve ser verdade para considerar o bug resolvido)
- Impacto: bloqueante / degradante / cosmético

**`FEATURE_REQUEST.md`** deve conter obrigatoriamente:
- Descrição da funcionalidade proposta
- Motivação e caso de uso concreto
- **Critérios de Aceitação** (comportamento esperado, testes necessários)
- Impacto em componentes existentes (LLM Interface, Agent, SQL Runner)
- Referência a issues relacionadas

> **Justificativa MPS.BR:** O Atributo de Processo AP 1.1 do Nível G exige que os resultados esperados dos processos sejam identificados. Critérios de Aceitação nas issues operacionalizam esse requisito.

#### Medida 1.B — Repositório Formalizado de RFCs

Criar o diretório `docs/rfcs/` com um template padrão `RFC_TEMPLATE.md`. Toda proposta de:
- Nova integração com LLM ou banco de dados
- Mudança arquitetural (ex.: transição VannaBase → Agent)
- Alteração no schema de embeddings ou vector store

...deve obrigatoriamente passar por uma RFC antes de qualquer implementação.

**Estrutura mínima de uma RFC:**
```
# RFC-[número]: [Título]
## Status: [Proposta | Em Revisão | Aprovada | Rejeitada]
## Autor: [nome e @github]
## Data: [YYYY-MM-DD]

## Motivação
## Proposta Técnica
## Alternativas Consideradas
## Impacto em Componentes Existentes
## Critérios de Aceitação
## Riscos Identificados
## Referências (Issues, PRs, documentação)
```

#### Medida 1.C — Tracker de Rastreabilidade (TRACEABILITY.md)

Criar e manter o arquivo `TRACEABILITY.md` na raiz do repositório para auditar PRs críticos. Formato sugerido:

```markdown
| RFC/Issue | PR | Commit(s) | Testes Associados | Status |
|-----------|-----|-----------|-------------------|--------|
| Issue #1031 | PR #1081 | abc1234 | test_chromadb_persistence.py | ✅ Fechado |
| Issue #659 | PR #660 | def5678 | test_pgvector_async.py | ⚠️ Dívida técnica |
| RFC-001 | PR #XXX | — | — | 🔄 Em progresso |
```

> **Impacto esperado:** Redução do acúmulo de issues sem dono e aumento da cobertura de testes por funcionalidade entregue.

---

## 4. Ação 2 — Gerência de Projetos com Controle de Riscos (GPR)

### 4.1 Problema Central

O desenvolvimento do Vanna ocorre de forma **completamente reativa**. Não há:
- Milestones ativos no GitHub agrupando entregas temáticas
- Roadmap público para a comunidade de 72 contribuidores
- Registro formal de riscos arquiteturais ou de negócio

O risco mais crítico identificado é a **volatilidade de APIs externas**: o Vanna 2.0 integra OpenAI, Anthropic, Ollama, Azure, Google Gemini, AWS Bedrock e Mistral. Qualquer mudança de precificação, deprecação de endpoint ou breaking change nessas APIs pode paralisar integrações sem aviso prévio — e o projeto não possui nenhum mecanismo formal para monitorar ou mitigar esse risco.

Risco adicional crítico identificado no Eixo V: **ausência de SAST (Static Application Security Testing)**. Para um sistema que executa SQL gerado por IA diretamente em bancos de dados, a ausência de varredura de segurança estática (ex.: Bandit, CodeQL) representa um vetor real de SQL Injection não mitigado no pipeline de CI.

### 4.2 Medidas Propostas

#### Medida 2.A — Adoção de Milestones no GitHub

Ativar e utilizar sistematicamente a feature de **Milestones** do GitHub para encapsular entregas temáticas. Cada Milestone deve conter:
- Título descritivo (ex.: `v2.1 — Estabilização PGVector e ChromaDB`)
- Data-alvo de fechamento
- Issues e PRs associados
- Meta de fechamento de bugs (ex.: reduzir de 99 para ≤ 50 bugs abertos)

Milestones sugeridos para o ciclo imediato pós-auditoria:
1. `Estabilização v2.0` — fechar os 99 bugs críticos da transição
2. `Rastreabilidade GRE` — implementar templates e RFC process
3. `Segurança SAST` — integrar Bandit/CodeQL ao pipeline CI
4. `Roadmap Público` — publicar planejamento de features v2.1

#### Medida 2.B — RISK_REGISTER.md

Criar e manter o arquivo `RISK_REGISTER.md` na raiz do repositório. Este documento deve ser atualizado a cada Milestone e revisado em pull requests que alterem dependências externas.

**Estrutura do Risk Register:**

```markdown
# Risk Register — Vanna AI

Última atualização: [data]
Responsável: [maintainer]

## Tabela de Riscos

| ID | Categoria | Descrição | Probabilidade | Impacto | Severidade | Mitigação | Status |
|----|-----------|-----------|--------------|---------|-----------|-----------|--------|
| R01 | API Externa | Deprecação de endpoint OpenAI (ex.: gpt-4 → gpt-4o migration) | Alta | Alto | Crítico | Adapter pattern isolado; testes E2E com mock | Monitorando |
| R02 | API Externa | Mudança de precificação Anthropic/Google Gemini | Média | Médio | Moderado | Documentar alternativas OSS (Ollama/Mistral) | Monitorando |
| R03 | Segurança | SQL Injection via query gerada por LLM sem validação | Alta | Crítico | Crítico | Implementar SAST (Bandit) no CI; validação de sintaxe SQL | Aberto |
| R04 | Arquitetura | Regressões na camada legacy (VannaBase) durante transição v2.0 | Alta | Alto | Crítico | Manter suite de testes legacy; LegacyVannaAdapter testado | Em progresso |
| R05 | Dependências | Ausência de version pinning (pandas, PyYAML, sqlalchemy) | Média | Alto | Alto | Implementar pin de versões no pyproject.toml | Aberto |
| R06 | Qualidade | 25+ regras Ruff ignoradas por "legacy compatibility" | Baixa | Médio | Moderado | Roadmap de remoção gradual dos ignores por Milestone | Planejado |
```

> **Justificativa MPS.BR:** O processo GPR do Nível G exige identificação e monitoramento de riscos do projeto. O RISK_REGISTER.md operacionaliza esse requisito com custo zero de infraestrutura.

#### Medida 2.C — Política de Version Pinning

Como extensão do controle de riscos, adicionar ao `CONTRIBUTING.md` e ao `pyproject.toml` a política de que **toda dependência nuclear deve ter version pinning com faixa segura**:

```toml
# Antes (inseguro):
dependencies = ["pandas", "PyYAML", "sqlalchemy"]

# Depois (seguro):
dependencies = [
  "pandas>=2.0.0,<3.0.0",
  "PyYAML>=6.0,<7.0",
  "sqlalchemy>=2.0.0,<3.0.0"
]
```

---

## 5. Plano de Execução — Roadmap de 4 Semanas

A implementação deve ser cadenciada para respeitar a cultura open source do projeto, evitando barreiras de entrada para novos contribuidores.

```
Semana 1 — Fundação Documental
├── Criar templates BUG_REPORT.md e FEATURE_REQUEST.md
├── Criar RFC_TEMPLATE.md e diretório docs/rfcs/
├── Publicar RISK_REGISTER.md com riscos R01–R06 mapeados
└── Abrir issue anunciando as mudanças de processo para a comunidade

Semana 2 — Rastreabilidade Ativa
├── Criar TRACEABILITY.md com histórico retroativo dos PRs críticos
├── Aplicar política de rastreabilidade em todos os novos PRs
├── Triagem das 227 issues abertas com os novos templates
└── Classificar os 99 bugs por severidade (Bloqueante / Degradante / Cosmético)

Semana 3 — Milestones e Riscos
├── Criar os 4 Milestones sugeridos no GitHub
├── Associar issues existentes aos Milestones correspondentes
├── Iniciar RFC-001: Integração SAST (Bandit/CodeQL) no pipeline CI
└── Atualizar RISK_REGISTER.md com status das mitigações

Semana 4 — Consolidação e Métricas
├── Fechar Milestone "Rastreabilidade GRE" com evidências
├── Publicar roadmap público v2.1 no README
├── Medir métricas de eficácia (ver Seção 6)
└── Retrospectiva: ajustar templates com base no feedback da comunidade
```

---

## 6. Métricas de Eficácia

A avaliação do plano será auferida através das seguintes métricas, coletadas ao final de cada Milestone:

| Métrica | Baseline (Auditoria) | Meta após 4 semanas |
|---------|---------------------|---------------------|
| % de PRs linkados a issues categorizadas | ~40% (estimado) | ≥ 85% |
| Bugs não corrigidos abertos | 99 | ≤ 60 |
| Issues abertas sem Critério de Aceitação | ~100% | ≤ 20% |
| Milestones ativos com data-alvo | 0 | ≥ 4 |
| Riscos formalmente documentados | 0 | ≥ 6 (R01–R06) |
| PRs com commit direto (sem revisão) | Recorrente | 0 (política aplicada) |

---

## 7. Impacto Esperado e Estado Futuro

Se aplicadas diligentemente, as ações propostas promovem a seguinte evolução de maturidade:

| Processo | Status Atual | Status Projetado |
|----------|-------------|-----------------|
| Gerência de Requisitos (GRE) | Informal / Reativo | MPS.BR Nível G — Gerenciado |
| Gerência de Projetos (GPR) | Ausente | MPS.BR Nível G — Gerenciado |
| Garantia da Qualidade (GQA) | Parcial | Caminho para Nível 2 |
| Verificação e Validação (V&V) | Parcial | Parcial (requer esforço adicional) |

O estado futuro do Vanna AI representaria um repositório maduro não apenas tecnicamente, mas **formalizado nos pilares de auditoria**. Com GRE e GPR estabelecidos, o ecossistema estaria plenamente elegível para atuar em missões críticas corporativas com respaldo **CMMI Nível 2 / MPS.BR Nível G** — objetivo declarado deste plano.

---

## 8. Referências

- SOFTEX. **MPS.BR — Melhoria de Processo do Software Brasileiro: Guia Geral MPS de Software**. Versão 2020.
- CMMI Institute. **CMMI Development, Version 2.0**. 2018.
- Repositório oficial: https://github.com/vanna-ai/vanna
- Issue #1031 → PR #1081 (ChromaDB persistence): https://github.com/vanna-ai/vanna/issues/1031
- Issue #986 (Gemini commit direto): https://github.com/vanna-ai/vanna/issues/986
- Issue #659 → PR #660 (PGVector): https://github.com/vanna-ai/vanna/issues/659
- Workflow CI: https://github.com/vanna-ai/vanna/blob/main/.github/workflows/tests.yml
- CONTRIBUTING.md: https://github.com/vanna-ai/vanna/blob/main/CONTRIBUTING.md
