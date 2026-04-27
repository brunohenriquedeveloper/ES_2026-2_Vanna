# Auditoria Ágil e Ciclo de Vida  
## Projeto analisado: Vanna-ai

---

# Roadmap e Visão Estratégica

O repositório da equipe mantém um roadmap público alinhando a visão geral do projeto, estando estruturado em três metas principais:

- Precisão  
- Interações  
- Autonomia  

Esse roadmap demonstra uma estratégia clara com a intenção de transformar o Vanna em um analista de dados baseado em IA open-source.

Apesar disso, o roadmap não está estruturado como plano ou arquivo interno no repositório GitHub, estando hospedado externamente no site oficial (`vanna.ai/docs`) e parcialmente documentado no README.

Dessa forma, não existe vinculação automática entre roadmap, issues e milestones internos, o que dificulta a rastreabilidade entre planejamento estratégico e implementação técnica.

## Recomendação

Criar um link explícito no próprio README que leve diretamente ao roadmap online e, se possível, listar no próprio repositório itens alinhados às metas estratégicas.

---

# Milestones e Projects

No GitHub não há milestones criados para este repositório.

A interface apresenta a mensagem:

> "You haven’t created any Milestones"

Isso confirma que não há agrupamentos oficiais de issues por versões ou entregas.

O elemento mais próximo disso são as tags de release indicando atualizações em versões.

Também não foram encontrados Projects para planejamento de sprints ou releases.

Essas ausências demonstram falta de planejamento formal dentro do GitHub, dificultando:

- Controle de escopo;
- Rastreamento de entregas;
- Previsibilidade de prazo.

## Recomendação

Uma prática ágil recomendada seria criar milestones por versões (ex: `v0.8`, `v1.0`) contendo issues e pull requests relacionados a cada release.

Isso aumentaria a transparência no processo de lançamento e facilitaria o controle de escopo.

## Estado Atual

| Categoria | Status | Observações |
|---|---:|---|
| Milestones | 0 | Nenhum agrupamento de issues/pulls |
| Releases | 9 principais | Releases sem milestones correspondentes |

---

# Releases e Controle de Versão

O projeto utiliza versionamento semântico, com tags variando entre `v0.7.x` até `v2.0.x`.

Foi observado um salto direto para a versão `2.0.0` em novembro de 2025, sem passagem por versões `1.x`.

Foram identificados 9 releases principais.

## Histórico de Releases

| Release | Data | Descrição |
|---|---|---|
| v0.7.4 | 23/10/2024 | Atualização de dependências |
| v0.7.5 | 25/10/2024 | Integração com novo LLM |
| v0.7.6 | 08/02/2025 | Melhorias de memória e integrações |
| v0.7.7 | 07/04/2025 | Integração com Cohere |
| v0.7.8 | 08/04/2025 | Correções de dependências (ChromaDB) |
| v0.7.9 | 10/04/2025 | Suporte ampliado a queries SQL |
| v2.0.0 | 19/11/2025 | Reescrita completa (Web UI e segurança) |
| v2.0.1 | 20/11/2025 | Correção de bug de memória |
| v2.0.2 | 02/02/2026 | Correção de inserção desnecessária |

## Análise

A cronologia irregular sugere:

- concentração de patches em períodos curtos;
- grandes intervalos sem lançamentos;
- forte mudança estrutural na versão 2.0.

Após a versão 2.0, houve redução no volume de releases, indicando possível estabilização.

Não foram encontrados:

- pré-releases;
- milestones vinculados;
- planejamento de releases futuros.

## Recomendação

Associar cada release a milestones correspondentes e estabelecer um cronograma previsível de lançamentos.

---

# Gestão de Riscos e APIs Externas (LLMs)

O Vanna depende fortemente de APIs de LLM e serviços externos, incluindo:

- OpenAI
- Gemini
- Azure
- Ollama

Essa dependência fica evidente nas dependências opcionais presentes no arquivo `pyproject.toml`.

O código implementa abstrações para diferentes provedores através de classes como:

- `OpenAI_Chat`
- `GeminiService`
- `OllamaDirect`

Essa arquitetura reduz o risco de dependência direta de um único fornecedor.

Caso uma API mude, basta atualizar seu adaptador específico.

O próprio README mostra exemplos de extensão utilizando múltiplos provedores.

## Risco identificado

Não foi encontrada política formal documentada para atualização e manutenção dessas APIs.

Mudanças drásticas em modelos, autenticação ou restrições podem quebrar funcionalidades.

## Recomendação

- Criar testes automatizados de integração contínua;
- Monitorar mudanças nas APIs externas;
- Atualizar dependências regularmente.

---

# Outros Riscos Técnicos e de Manutenção

## Testes Automatizados

O repositório possui cobertura significativa de testes, incluindo:

- agentes;
- memória;
- integrações com Azure;
- Gemini;
- Ollama.

O CI via GitHub Actions executa:

- Basic Integration Tests

Isso demonstra preocupação com estabilidade e regressão.

### Avaliação

Ponto positivo em maturidade técnica.

---

## Manutenção e Comunidade

O projeto foi arquivado em março de 2026.

Isso indica encerramento de manutenção ativa.

Antes disso, havia forte atividade da comunidade e correções frequentes.

Foi identificado:

- correção de CVEs;
- correção de parsing JSON;
- melhorias estruturais.

### Avaliação

O arquivamento representa risco de continuidade.

---

## Resposta a Issues

O projeto possui aproximadamente 227 issues.

A quantidade de PRs e colaboradores indica comunidade ativa até pouco antes do arquivamento.

Porém, a ausência de milestones dificulta medir eficiência na resolução.

### Avaliação

Boa atividade comunitária, porém baixa rastreabilidade.

---

# Conclusão e Avaliação de Maturidade

O projeto Vanna apresenta forte maturidade técnica, evidenciada por:

- documentação robusta;
- documentação externa;
- testes automatizados;
- integração contínua;
- releases frequentes.

Por outro lado, apresenta falhas importantes na gestão formal do projeto:

- ausência de milestones;
- ausência de projects;
- ausência de rastreabilidade formal.

Isso sugere uma organização mais reativa do que proativa.

## Avaliação Final

### Maturidade Técnica: Alta ✅  
### Maturidade de Planejamento Ágil: Média ⚠️  
### Gestão Formal de Escopo: Baixa ❌

## Plano de Melhoria Sugerido

1. Implementação de milestones vinculadas às releases  
2. Formalização do roadmap dentro do próprio repositório  
3. Cronograma previsível de lançamentos  
4. Política documentada de manutenção de APIs externas
