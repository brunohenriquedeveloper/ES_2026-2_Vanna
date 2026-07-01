# **Auditoria de Testes de Software e Evolução da Qualidade: Vanna AI**

🎥 https://youtu.be/aNFVPIMzgwY?si=VibYBFucFYJv5608


Este repositório contém os artefatos, evidências e o relatório técnico produzidos para a **Atividade Avaliativa 3 (A3)** da disciplina de Engenharia de Software (COMP0503) da Universidade Federal de Sergipe (UFS).

Dando continuidade às Atividades 1 e 2 sobre o mesmo projeto, o objetivo desta etapa é realizar um **diagnóstico da estratégia de testes** do projeto open source [Vanna AI](https://github.com/vanna-ai/vanna) — investigando existência, qualidade, cobertura e relevância dos testes automatizados — e propor um **plano concreto de evolução da qualidade** com base em boas práticas.

## **📂 Guia do Repositório**

Abaixo está o mapeamento dos artefatos produzidos pela equipe. O relatório completo encontra-se em `/docs`, enquanto os insumos técnicos estão divididos em diretórios:

* 📄 **/docs**: Relatório técnico completo da auditoria (PDF) e a apresentação de impacto (slides Beamer exportados).
* 📸 **/evidencias**: Links de rastreabilidade reais (arquivos, configs, workflows, Issues/PRs) que comprovam cada achado do PDF, organizados por eixo.
* 🗺️ **/diagramas**: Diagrama da evolução do pipeline de qualidade (Eixo D · D4), em PNG e SVG.

## **📊 Sumário Executivo: O que você encontrará no PDF?**

O relatório está estruturado em 4 Eixos de Investigação, culminando em um Plano de Evolução da Qualidade. Resumo dos achados:

1. **Eixo A (Estratégia Atual de Testes):** \- *Achado:* Há pasta `/tests` dedicada e convenções no `pyproject.toml` (Pytest, Tox, Ruff, MyPy). Porém o CI dispara **apenas em `push` na `main`, nunca em Pull Requests**, e não há badge nem relatório de cobertura. **(Risco Médio)**

2. **Eixo B (Qualidade Técnica dos Testes):** \- *Achado:* Os testes existentes têm nomenclatura clara, docstrings e boa organização por funcionalidade, seguindo boas práticas de legibilidade e manutenção. **(Risco Baixo)**

3. **Eixo C (Lacunas, Riscos e Falhas Potenciais):** \- *Achado:* Funcionalidades centrais do produto (geração de SQL, `ErrorRecoveryStrategy`, streaming, gráficos, execução de código) **não possuem testes independentes de credenciais externas**. Sem execução em PR e sem limiar de cobertura, não há proteção real contra regressões — uma "ilusão de cobertura". **(Risco Alto)**

4. **Eixo D (Plano de Evolução da Qualidade):** \- Proposta: Priorização do Top 3 de riscos (Testes de Contrato/Segurança contra Prompt Injections, Isolamento de Rede para erradicar Flaky Tests, e Travas de Regressão na CI/CD); foco estrito na Camada de Integração e Contratos (evitando o custo de refatoração para testes unitários na God Class); implantação gradual (Shift-Left) sem travar o desenvolvimento; automação em duas fases (prevenção local silenciosa com SAST/Bandit no pre-commit e pipeline de CI/CD com diff-cover exigindo testes apenas em código novo); e critérios de sucesso focados em estancar o crescimento da dívida técnica e elevar a maturidade do projeto (MPS.BR - Nível G).

## **👥 Equipe de Auditoria**

* **Leonardo Caricchio do Nascimento** \- Eixo A — itens A1, A2 e A3 (estrutura, tipos de teste e documentação)
* **Bruno Henrique Carneiro da Silva** \- Eixos A e B — itens A4, A5 e B1 (CI/CD, cobertura e clareza dos testes)
* **Diego Carvalho Cavalcante** \- Eixos B e C — itens B3, C2 e C3 (mocks/fixtures, módulos de risco e isolamento de dependências)
* **Matheus Henrique Silva de Melo** \- Eixo B — itens B2, B4 e B5 (isolamento, relevância e manutenção) · Tech Lead (Estrutura e Revisão)
* **José Weverton de Oliveira Vilar** \- Eixo C — itens C1, C4 e C5 (lacunas críticas, regressões e diagnóstico de bugs)
* **Fábio Henrique Lisboa de Souza** \- Eixo D — itens D1, D2 e D3 (priorização, camadas de foco e implantação gradual)
* **Flávio Henrique de Jesus Cruz** \- Eixo D — itens D4 e D5 (automação e critérios de sucesso)
* **Guilherme Sollano Andrade dos Santos** \- Direção e Edição do Vídeo
