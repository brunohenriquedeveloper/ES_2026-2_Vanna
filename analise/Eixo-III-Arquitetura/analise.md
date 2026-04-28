# Análise de Arquitetura e Modelagem (Eixo III - PJR)

## 1. Introdução
Esta análise técnica integra a Auditoria de Maturidade no Ecossistema **Vanna AI**, focando na área de processo **Projeto e Construção (PJR)** do MPS.BR e na área de **Solução Técnica (TS)** do CMMI-DEV v2.0. O objetivo central é avaliar como o design técnico do projeto isola a lógica de negócio da volatilidade inerente às APIs externas de Modelos de Linguagem de Larga Escala (LLMs).

## 2. Visão Geral da Arquitetura
O Vanna AI adota uma arquitetura inspirada no padrão **Microkernel** (ou Arquitetura de Plug-ins). O "Core" do sistema atua como o Kernel, definindo os contratos e fluxos de execução, enquanto as integrações com diferentes provedores de IA (OpenAI, Anthropic, etc.) funcionam como plug-ins intercambiáveis.

## 3. Padrões de Projeto e Mecanismos de Desacoplamento

### 3.1. Abstract Base Classes (ABC) e Strategy Pattern
O projeto utiliza o padrão **Strategy** para permitir a troca dinâmica de provedores de IA. A base dessa implementação reside em `src/vanna/core/llm/base.py`, onde a classe abstrata `LlmService` define os métodos contratuais obrigatórios (`send_request`, `stream_request`). 
* **Justificativa:** Isso garante que o sistema não dependa de implementações concretas, mas de abstrações, respeitando o Princípio da Inversão de Dependência (D do SOLID).

### 3.2. Dependency Injection (DI)
A classe orquestradora `Agent` (`src/vanna/core/agent/agent.py`) utiliza Injeção de Dependência via construtor. Ela recebe uma instância de `LlmService` em sua inicialização, delegando a responsabilidade de escolha do provedor para a camada de configuração externa.
* **Evidência:** `def __init__(self, llm_service: LlmService, ...)`

### 3.3. Uso de Data Transfer Objects (DTO)
Para evitar o acoplamento de tipos de dados proprietários (como os esquemas JSON específicos da OpenAI), o Vanna utiliza DTOs como `LlmRequest` e `LlmResponse`. 
* **Impacto:** Todas as "Strategies" (integrations) devem traduzir o payload interno do Vanna para o formato da API externa e vice-versa, protegendo o domínio da aplicação de mudanças de versão em APIs de terceiros.

## 4. Alinhamento com Modelos de Maturidade

### 4.1. CMMI-DEV v2.0: Solução Técnica (TS)
O Vanna demonstra maturidade na prática de "Selecionar e Projetar Soluções" (TS 2.1) ao isolar os componentes tecnológicos voláteis. A arquitetura permite que novas tecnologias de IA sejam incorporadas sem alterações no núcleo do agente, mitigando riscos técnicos e reduzindo custos de manutenção.

### 4.2. MPS.BR: Projeto e Construção (PJR)
Conforme o nível de maturidade G, o processo PJR exige que o projeto do software descreva interfaces de forma precisa. A modelagem do Vanna utiliza tipagem estática e interfaces abstratas para definir limites de sistema, garantindo que a construção do software seja rastreável e desacoplada, facilitando a substituição de componentes (PJR 2).

## 5. Conclusão Técnica
A arquitetura do Vanna AI é um exemplo de design defensivo contra a volatilidade tecnológica. O isolamento das APIs externas em uma camada de integração, mediada por contratos abstratos e injeção de dependência, confere ao projeto um alto nível de manutenibilidade e escalabilidade, características fundamentais para organizações que buscam certificações de alta maturidade em Engenharia de Software.
