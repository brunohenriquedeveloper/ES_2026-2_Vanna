# Evidências Arquiteturais (Eixo III - PJR)

## 1. Escopo da Auditoria
Esta trilha de evidências documenta os artefatos e trechos de código do projeto **Vanna AI** que comprovam a maturidade arquitetural e o desacoplamento da camada de Inteligência Artificial.

## 2. Rastreabilidade de Arquivos Auditados

### 2.1. Contrato Base (Interface)
* **Arquivo:** `src/vanna/core/llm/base.py`
* **Achado:** Classe `LlmService(ABC)` e métodos decorados com `@abstractmethod`.
* **Justificativa Técnica:** Comprova a criação de uma **Fronteira de Isolamento**. O sistema define *o que* deve ser feito (enviar request, validar tools) sem se acoplar a *como* será feito. Alinhado ao **MPS.BR PJR 1 (Design do Software)**.

### 2.2. Implementações Concretas (Strategies)
* **Arquivos:** * `src/vanna/integrations/openai/llm.py`
    * `src/vanna/integrations/anthropic/llm.py`
* **Achado:** Classes `OpenAILlmService` e `AnthropicLlmService` herdando de `LlmService`.
* **Justificativa Técnica:** Evidencia o uso do **Strategy Pattern**. Cada provedor encapsula sua própria dependência externa (SDK da OpenAI ou Anthropic), garantindo que essas bibliotecas nunca sejam importadas no "Core". Cumpre o requisito de **Intercambialidade do CMMI TS**.

### 2.3. Orquestração e Injeção de Dependência
* **Arquivo:** `src/vanna/core/agent/agent.py`
* **Achado:** Método `__init__` recebendo `llm_service: LlmService`.
* **Justificativa Técnica:** Prova o uso de **Dependency Injection (DI)**. O `Agent` (Regra de Negócio) opera exclusivamente via interface abstrata. Isso garante que a lógica de geração de SQL seja agnóstica ao modelo de linguagem utilizado.

### 2.4. Evolução e Retrocompatibilidade (Adapter)
* **Arquivo:** `src/vanna/legacy/adapter.py`
* **Achado:** Classe `LegacyVannaAdapter` traduzindo métodos do `VannaBase` antigo para o novo sistema.
* **Justificativa Técnica:** Demonstra o tratamento de **Dívida Técnica** e evolução controlada da arquitetura. O uso do padrão **Adapter** permite a migração para a nova arquitetura modular sem romper contratos com usuários antigos, indicando maturidade de processo (Nível G do MPS.BR).

## 3. Artefato Visual de Apoio
A modelagem detalhada desta fronteira arquitetural, representando as relações de realização, agregação e dependência externa encapsulada, foi consolidada em um **Diagrama de Classes UML**.
* **Localização:** `analise/Eixo-III-Arquitetura/imagens/diagrama_classes_vanna.png`
* **Documentação do Diagrama:** `diagramas/diagrama.md`

## 4. Conclusão da Evidência
A combinação de Injeção de Dependência com Abstrações Formais e DTOs documentados nos arquivos acima constitui prova material de que o projeto Vanna AI segue padrões rigorosos de Engenharia de Software, resultando em uma solução técnica robusta e de baixo acoplamento.
