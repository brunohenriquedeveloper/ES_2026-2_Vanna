# Modelagem de Arquitetura - Vanna AI

Este diretório centraliza os artefatos de modelagem UML desenvolvidos durante a auditoria técnica da disciplina de Engenharia de Software.

## 1. Diagrama de Classes (Eixo III - PJR)

O diagrama abaixo ilustra o desacoplamento entre a camada de negócio (Agent) e as APIs externas de LLM, utilizando os padrões **Strategy** e **Dependency Injection**.

### Visualização
![Diagrama de Classes UML](../analise/Eixo-III-Arquitetura/imagens/diagrama_classes_vanna.png)

## 2. Código Fonte (PlantUML)

Para garantir a reprodutibilidade e futuras manutenções da arquitetura, o código fonte abaixo pode ser processado em qualquer interpretador PlantUML.

```plantuml
@startuml
' Configurações de estilo acadêmico
skinparam classAttributeIconSize 0
skinparam monochrome true
skinparam shadowing false
skinparam packageStyle rectangle
skinparam nodesep 70
skinparam ranksep 80
top to bottom direction

title Eixo III - Desacoplamento da Camada de IA (Vanna AI)

package "Camada de Negócio (Core)" {
    class Agent {
        - llm_service: LlmService
        + __init__(llm_service: LlmService)
        + send_message(message: str): AsyncGenerator
        - _send_llm_request(request: LlmRequest): LlmResponse
    }
}

package "Fronteira de Isolamento (Contratos)" {
    abstract class LlmService <<abstract>> {
        + {abstract} send_request(request: LlmRequest): LlmResponse
        + {abstract} stream_request(request: LlmRequest): AsyncGenerator
        + {abstract} validate_tools(tools: List[Any]): List[str]
    }

    class LlmRequest <<DTO>> {
        + messages: List<Dict>
        + system_prompt: str
        + tools: List<ToolSchema>
    }

    class LlmResponse <<DTO>> {
        + content: str
        + tool_calls: List<ToolCall>
    }
}

package "Camada de Integração (Strategies)" {
    class OpenAILlmService {
        - _client: OpenAI
        + __init__(api_key: str, model: str)
        + send_request(request: LlmRequest): LlmResponse
        - _build_payload(request: LlmRequest): Dict
    }

    class AnthropicLlmService {
        - _client: Anthropic
        + __init__(api_key: str, model: str)
        + send_request(request: LlmRequest): LlmResponse
        - _build_payload(request: LlmRequest): Dict
        - _parse_message_content(msg: Any): Tuple
    }
}

package "APIs Externas" {
    class OpenAISDK <<external>>
    class AnthropicSDK <<external>>
}

' Relacionamentos Estruturais
Agent "1..1" o--> "1..1" LlmService : Agregação\n(Injeção de Dependência)

LlmService ..> LlmRequest : Depende
LlmService ..> LlmResponse : Depende

OpenAILlmService .up.|> LlmService : Realização\n(Strategy)
AnthropicLlmService .up.|> LlmService : Realização\n(Strategy)

OpenAILlmService ..> OpenAISDK : Dependência Externa\nEncapsulada
AnthropicLlmService ..> AnthropicSDK : Dependência Externa\nEncapsulada

@enduml
