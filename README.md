# **Auditoria de Maturidade em Ecossistemas LLM: Vanna AI**

🎥 [**ASSISTA AO VÍDEO DA DEFESA TÉCNICA AQUI**](https://youtu.be/EXBY1mnRQP0)

Este repositório contém os artefatos, evidências e o relatório técnico produzidos para a Atividade Avaliativa 1 (A1) da disciplina de Engenharia de Software (COMP0503) da Universidade Federal de Sergipe (UFS).

O objetivo do trabalho é realizar uma auditoria rigorosa no projeto open source [Vanna AI](https://github.com/vanna-ai/vanna), contrastando a agilidade do seu ecossistema com as práticas exigidas pelos modelos de maturidade **CMMI-DEV (v2.0)** e **MPS.BR (Nível G)**.

## **📂 Guia do Repositório**

Abaixo está o mapeamento dos artefatos produzidos pela equipe. O relatório escrito aprofundado encontra-se na raiz, enquanto os insumos técnicos estão divididos em diretórios:

* 🚀 **/analise**: Análise detalhada dos eixos apresentados na auditoria com possivelmente evidências complementares adicionais em relação às utilizadas no pdf.
* 🗺️ **/diagramas**: Contém o diagrama de classes UML (arquitetura\_vanna.png) evidenciando o desacoplamento da IA (Eixo III).  
* 📸 **/evidencias**: Links de rastreabilidade transversal (Issues/PRs) e dos workflows de CI/CD referentes ao PDF.
* 📄 **A1\_Vanna\_Matheus.pdf**: O relatório principal e completo contendo a auditoria acadêmica.  


## **📊 Sumário Executivo: O que você encontrará no PDF?**

O relatório final está estruturado em 5 Eixos de Auditoria, culminando em um Plano de Melhoria. Aqui está um resumo dos achados detalhados no documento:

1. **Eixo I (Ciclo de Vida e Ágil \- GPR):** \- *Achado:* O projeto tem um ciclo ágil altamente agressivo (78 releases contínuas), mas peca pela ausência de planejamento de longo prazo (Milestones inativos) e total ausência de um mapeamento formal de riscos (vulnerabilidade a APIs de LLM externas).  
2. **Eixo II (Engenharia de Requisitos \- GRE):** \- *Achado:* A rastreabilidade básica existe no GitHub, mas é puramente reativa. Não há processos formais de RFCs (Request for Comments) para funcionalidades críticas, impactando a gestão de mudanças.  
3. **Eixo III (Arquitetura e Modelagem):** \- *Achado:* Excelente isolamento da camada de IA. O sistema adota padrões como *Strategy* e *Microkernel*, tornando os LLMs (OpenAI, Gemini, Ollama) componentes perfeitamente plugáveis e agnósticos à regra de negócio principal.  
4. **Eixo IV (Verificação e Validação \- V\&V):** \- *Achado:* Forte adoção de Integração Contínua (CI) com validações automatizadas via tox. Contudo, falha criticamente na Validação da IA, pois carece de pipelines defensivas em produção para evitar "alucinações" na geração das consultas SQL.  
5. **Eixo V (Qualidade de Software \- GQA):** \- *Achado:* Utiliza ferramentas rigorosas no CONTRIBUTING.md (Ruff, Mypy Strict), mas possui dívida técnica acumulada intencionalmente na transição para a "Versão 2.0" e ausência de varreduras de segurança estática (SAST).  
6. **Plano de Melhoria de Processo:**  
   * *Proposta:* Implementação de 2 ações prioritárias de baixo custo focadas em **GPR** (Gerência de Projetos com Controle de Riscos) e **GRE** (Rastreabilidade Bidirecional com templates).

## **👥 Equipe de Auditoria**

* **Diego Carvalho Cavalcante** \- Eixo I (Ágil)  
* **Bruno Henrique Carneiro da Silva** \- Eixo II (Requisitos)  
* **Fábio Henrique Lisboa de Souza** \- Eixo III (Arquitetura)  
* **Leonardo Caricchio do Nascimento** \- Eixo IV (V\&V)  
* **José Weverton de Oliveira Vilar** \- Eixo V (Qualidade)  
* **Flávio Henrique de Jesus Cruz** \- Estratégia e Plano de Melhoria  
* **Matheus Henrique Silva de Melo** \- Tech Lead (Estrutura e Revisão)  
* **Guilherme Sollano Andrade dos Santos** \- Direção e Edição do Vídeo
