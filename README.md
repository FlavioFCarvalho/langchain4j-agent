# Multi-Agent Order Support — LangChain4j

Projeto de estudo e implementação de um sistema de atendimento inteligente para e-commerce, utilizando **Java, Spring Boot, LangChain4j e OpenAI**.

A proposta é desenvolver uma arquitetura multiagente (*Multi-Agent System*), na qual um agente Supervisor será responsável por interpretar as solicitações dos usuários e delegar tarefas para agentes especializados.

## 🎯 Objetivo

Construir um assistente inteligente capaz de atender solicitações relacionadas a pedidos de e-commerce, utilizando LLMs (*Large Language Models*) e ferramentas integradas à aplicação.

O projeto explora conceitos de:
- Inteligência Artificial Generativa.
- Large Language Models (LLMs).
- Multi-Agent Systems.
- Supervisor Agent.
- Tool Calling / Function Calling.
- Integração entre IA e serviços de negócio.
- Persistência de dados com Spring Data JPA.

## 🏗️ Arquitetura proposta

```text
                  CLIENTE
                     |
                     v
              AiSupportController
                     |
                     v
              SupervisorAIAgent
                     |
          +----------+----------+
          |                     |
          v                     v
   JavaTechieAIAgent    OrderSupportAIAgent
                                |
                                v
                            OrderTools
                                |
                                v
                           OrderService
                                |
                                v
                          OrderRepository
                                |
                                v
                              MySQL
```

### Agentes

**SupervisorAIAgent**
- Recebe as mensagens dos usuários.
- Interpreta a intenção da solicitação.
- Identifica o agente especializado responsável.
- Coordena a execução das tarefas.
- Consolida as respostas.

**JavaTechieAIAgent**
- Responsável pelo atendimento geral.
- Responde dúvidas que não exigem operações específicas sobre pedidos.

**OrderSupportAIAgent**
- Especializado no atendimento de pedidos.
- Consulta informações reais no banco de dados.
- Utiliza ferramentas disponibilizadas pela aplicação.
- Não deve inventar informações sobre pedidos.

## 🛠️ Tecnologias utilizadas

| Tecnologia | Finalidade |
|---|---|
| Java | Linguagem de programação |
| Spring Boot | Framework da aplicação |
| LangChain4j | Integração e orquestração de IA |
| OpenAI | Modelo de linguagem (LLM) |
| Spring Data JPA | Persistência de dados |
| Hibernate | Mapeamento objeto-relacional |
| MySQL | Banco de dados |
| Maven | Gerenciamento de dependências |

## 🔧 Ferramentas (Tools)

O projeto utiliza o recurso de Tool Calling do LangChain4j para permitir que o agente execute operações reais na aplicação.

Atualmente, a classe `OrderTools` disponibiliza:

### getOrderStatus

Consulta o status atual de um pedido.

```java
@Tool("Get the current status and delivery information for an order")
public OrderEntity getOrderStatus(String orderId) {
    return orderService.getOrderStatus(orderId);
}
```

### cancelOrder

Realiza o cancelamento de um pedido, respeitando as regras de negócio implementadas no serviço.

```java
@Tool("Cancel an order")
public OrderEntity cancelOrder(String orderId) {
    return orderService.cancelOrder(orderId);
}
```

## 🗄️ Modelo de dados

A entidade `OrderEntity` representa os pedidos armazenados no banco de dados.

Principais atributos:

| Campo | Descrição |
|---|---|
| id | Identificador interno |
| orderId | Identificador do pedido |
| customerName | Nome do cliente |
| productName | Nome do produto |
| status | Status do pedido |
| currentLocation | Localização atual |
| expectedDelivery | Previsão de entrega |
| totalAmount | Valor total |
| paymentStatus | Status do pagamento |

## 🚀 Endpoints

Base URL: `/api/ai-support`

| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/api/ai-support?message=...` | Atendimento geral |
| GET | `/api/ai-support/order?message=...` | Atendimento especializado em pedidos |

Exemplo:

```http
GET /api/ai-support/order?message=Qual o status do pedido ORD123?
```

## ⚙️ Configuração

### Pré-requisitos

- Java 17 ou superior.
- Maven.
- MySQL.
- Chave de API da OpenAI.

### Variáveis de ambiente

Configure sua chave da OpenAI como variável de ambiente:

```properties
OPENAI_API_KEY=sua-chave-api
```

Configure também as propriedades de conexão com o MySQL no `application.yml`, conforme o ambiente de execução.

**Importante:** não versione chaves de API, senhas ou credenciais no repositório.

## 🧠 Conceitos de arquitetura explorados

O projeto busca explorar a evolução de um agente individual para uma arquitetura multiagente.

Principais conceitos:

- **Agent:** agente inteligente responsável por uma tarefa específica.
- **Supervisor:** agente responsável pela coordenação e delegação.
- **Tool Calling:** mecanismo que permite ao LLM solicitar a execução de métodos da aplicação.
- **Orchestration:** coordenação entre agentes especializados.
- **Separation of Concerns:** separação entre inteligência artificial, regras de negócio e persistência.

## 📌 Roadmap

- [x] Configuração inicial do Spring Boot.
- [x] Integração com LangChain4j.
- [x] Integração com OpenAI.
- [x] Implementação do agente de atendimento geral.
- [x] Implementação do agente de suporte a pedidos.
- [x] Implementação das ferramentas de consulta e cancelamento.
- [x] Integração com MySQL utilizando Spring Data JPA.
- [ ] Implementação do Supervisor Agent.
- [ ] Orquestração entre agentes especializados.
- [ ] Implementação de novos agentes de negócio.
- [ ] Gerenciamento de contexto e memória das conversas.
- [ ] Testes de integração dos agentes.

## 📚 Referências

- [LangChain4j Documentation](https://docs.langchain4j.dev/)
- [LangChain4j GitHub](https://github.com/langchain4j/langchain4j)
- [OpenAI API Documentation](https://platform.openai.com/docs/)

---

**Projeto desenvolvido para estudos práticos de Inteligência Artificial Generativa, arquitetura multiagente e integração de LLMs com aplicações Java e Spring Boot.**
