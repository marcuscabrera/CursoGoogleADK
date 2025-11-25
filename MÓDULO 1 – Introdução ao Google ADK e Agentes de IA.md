# MÓDULO 1 – Introdução ao Google ADK e Agentes de IA  
(Duração estimada: 3–4 horas)

---

## 1. Visão Geral do Módulo

Este módulo introduz o Google ADK (Agent Development Kit) como framework para desenvolvimento de agentes de IA, apresentando seus conceitos fundamentais, arquitetura e ecossistema. A partir daqui, os participantes terão base sólida para os módulos mais práticos.

### Objetivos Específicos

Ao final do módulo, o participante deverá:

- Compreender claramente o que é o Google ADK e por que ele existe.  
- Entender sua arquitetura modular e extensível.  
- Saber que o ADK é model-agnostic e como isso se traduz na prática (Gemini, OpenAI, Anthropic).  
- Ter uma visão clara dos componentes principais (Agents, Tools, Memory, Callbacks) e como eles se relacionam.  
- Conseguir identificar quando faz sentido usar o ADK e em quais tipos de projetos.  

---

## 2. O que é o Google ADK

### 2.1 Conceito: Framework open-source para agentes inteligentes

O **Google ADK (Agent Development Kit)** é um framework open-source projetado para:

- **Criar agentes de IA** de forma estruturada e escalável.  
- **Padronizar a interação com modelos de linguagem** (LLMs) de diferentes provedores.  
- **Simplificar a orquestração** de agentes, ferramentas, memória e fluxos multiagente.

Em vez de escrever scripts “soltos” chamando APIs de LLM diretamente, o ADK provê:

- Abstrações de alto nível (Agent, Tool, Memory, Orchestrator…).  
- Padrões prontos para arquiteturas multiagente.  
- Integração nativa com os modelos **Gemini**, e extensões para outros provedores.

Em termos práticos, o ADK responde a perguntas como:

- “Como estruturar um agente bem definido, com instruções, ferramentas e memória?”  
- “Como mudar de modelo (ex.: Gemini para OpenAI) sem reescrever tudo?”  
- “Como monitorar, debugar e colocar agentes em produção?”

### 2.2 Por que um framework de agentes?

Motivações principais:

- **Redução de complexidade**:  
  Organiza o código em componentes reutilizáveis, em vez de “chamadas soltas” à API de LLM.

- **Manutenibilidade**:  
  Fácil evoluir o sistema (substituir modelos, adicionar ferramentas, criar novos agentes).

- **Escalabilidade**:  
  Ajuda a sair de um único chatbot simples para sistemas multiagente com regras, memória, ferramentas e monitoramento.

- **Boas práticas embutidas**:  
  Prompts organizados, segurança, logs, callbacks, padrão para usar ferramentas, etc.

### 2.3 Arquitetura modular e extensível

De forma simplificada, a arquitetura do ADK é composta por:

- **Camada de Modelos**  
  Adapta provedores (Gemini, OpenAI, Anthropic, etc.) a uma interface comum.

- **Camada de Agentes**  
  Define comportamento, instruções, parâmetros, ferramentas e memória de cada agente.

- **Camada de Ferramentas (Tools)**  
  Permite que o agente execute ações concretas (chamar APIs, consultar BD, ler arquivos…).

- **Camada de Memória**  
  Armazena e recupera contexto de conversas, conhecimento e histórico.

- **Camada de Orquestração / Multiagente**  
  Coordena a interação entre múltiplos agentes (sequencial, paralelo, loop, etc.).

Essa modularidade traz dois benefícios centrais:

- **Extensibilidade**:  
  É possível adicionar novos tipos de memória, modelos, ferramentas, padrões de orquestração.

- **Substituibilidade**:  
  Você troca o modelo ou a ferramenta sem reescrever toda a aplicação.

#### Diagrama de Arquitetura (Visão Simplificada)

Representação textual da arquitetura típica de uma aplicação usando ADK:

```
+----------------------------------------------------+
|                    SUA APLICAÇÃO                   |
|  (API, Web, CLI, Backend, Workflow de Negócio)     |
+---------------------------+------------------------+
                            |
                            v
                  +---------------------+
                  |        AGENTS       |
                  |  - Instruções       |
                  |  - Parâmetros LLM   |
                  |  - Ferramentas      |
                  |  - Memória          |
                  +----------+----------+
                             |
     +-----------------------+-----------------------+
     |                       |                       |
     v                       v                       v
+----------+           +-----------+           +-----------+
|  TOOLS   |           |  MEMORY   |           | CALLBACKS |
| - APIs   |           | - Context |           | - Logs    |
| - BD     |           | - Histórico          | - Métricas|
| - FS     |           | - Vetores            | - Monitor |
+----------+           +-----------+           +-----------+
     |
     v
+----------------------+
|  EXTERNAL SERVICES   |
|  (REST APIs, DBs,    |
|   sistemas legados)  |
+----------------------+

                 +--------------------+
                 |   MODEL ADAPTERS   |
                 |  (Gemini, OpenAI,  |
                 |   Anthropic, etc.) |
                 +---------+----------+
                           |
                           v
                    +-------------+
                    |   LLMs /    |
                    | Modelos de  |
                    | Linguagem   |
                    +-------------+
```

---

### 2.4 Integração nativa com múltiplos modelos de linguagem

O ADK foi projetado para ser **model-agnostic**, permitindo que o mesmo código de agente utilize:

- **Gemini (Google)** – integração nativa e priorizada.  
- **OpenAI (GPT)** – via adaptadores.  
- **Anthropic (Claude)** – via adaptadores.  
- Outros modelos proprietários ou open-source, desde que se implemente um adapter.

Isso é fundamental para:

- Testar **performance e custo** entre provedores.  
- Ter **planos de contingência** (fallback) se um provedor ficar indisponível.  
- Acompanhar a evolução do mercado sem reescrever sua aplicação.

---

## 3. Diferenciações do ADK

### 3.1 Suporte model-agnostic (Gemini, OpenAI, Anthropic)

O ADK oferece uma **interface unificada** para diferentes modelos. Na prática, você define o modelo ao criar o agente ou o cliente de modelo, sem alterar a lógica principal.

Exemplo conceitual (pseudo-Python):

```python
from google_adk import Agent
from google_adk.models import GeminiModel, OpenAIModel, AnthropicModel

# Agente usando Gemini
support_agent_gemini = Agent(
    name="SuporteCliente",
    model=GeminiModel("gemini-3.0-pro"),
    instructions="Você é um agente de suporte ao cliente cordial e objetivo."
)

# Mesmo agente, mas com modelo OpenAI
support_agent_openai = Agent(
    name="SuporteCliente",
    model=OpenAIModel("gpt-4.1"),
    instructions="Você é um agente de suporte ao cliente cordial e objetivo."
)

# Mesmo agente, mas com modelo Anthropic
support_agent_anthropic = Agent(
    name="SuporteCliente",
    model=AnthropicModel("claude-3.5-sonnet"),
    instructions="Você é um agente de suporte ao cliente cordial e objetivo."
)
```

Com poucas mudanças, você consegue:

- Comparar a qualidade das respostas.  
- Avaliar custos por mil tokens.  
- Escolher o modelo mais adequado para cada parte do sistema.

### 3.2 Gestão simplificada de contexto e memória

Uma das maiores dores ao trabalhar diretamente com LLMs é:

- Gerenciar toda a **janela de contexto** (qual parte do histórico enviar?).  
- Decidir o que deve ser “lembrado” ou esquecido.  
- Implementar manualmente memória de longo prazo (RAG, vetores, etc.).

O ADK ajuda com:

- Abstrações de **Memory** (memória de curto e longo prazo).  
- Estratégias prontas de **resumo e compressão** de histórico.  
- Integração facilitada com bancos vetoriais (vector stores).

Exemplo conceitual em código:

```python
from google_adk import Agent
from google_adk.memory import VectorMemory

# Configura uma memória vetorial para armazenar contexto
memory = VectorMemory(
    storage_path="./data/memoria_assistente",
    embedding_model="text-embedding-004"
)

assistant = Agent(
    name="AssistentePessoal",
    instructions="Lembre-se de preferências do usuário e histórico relevante.",
    memory=memory
)

assistant.run("Meu café favorito é expresso duplo.")
assistant.run("Minha linguagem preferida é Python.")

# Mais tarde...
resposta = assistant.run("Que café e linguagem eu gosto?")
# O agente recupera da memória e responde com base no histórico persistente.
```

Benefícios:

- Você não precisa “manualizar” o envio de todo o histórico a cada chamada.  
- O ADK ajuda a **selecionar o contexto mais relevante**.  
- Facilita a implementação de agentes que aprendem com o usuário ao longo do tempo.

### 3.3 Sistema robusto de ferramentas e callbacks

#### Ferramentas (Tools)

“Ferramentas” são funções ou serviços que o agente pode chamar para:

- Buscar dados em APIs externas.  
- Consultar bancos de dados.  
- Executar cálculos complexos.  
- Interagir com sistemas legados.

O ADK organiza esse processo em torno de classes ou funções Tool, com:

- Descrição da ferramenta (para o modelo saber quando usá-la).  
- Esquema de entrada/saída (muitas vezes usando Pydantic).  
- Tratamento de erros e logging padronizados.

Exemplo simplificado de Tool:

```python
from google_adk import Tool

class GetWeatherTool(Tool):
    name = "get_weather"
    description = "Obtém a previsão do tempo para uma cidade específica."

    def execute(self, city: str) -> str:
        # Aqui estaria a chamada real à API de clima.
        # Exemplo simplificado:
        if city.lower() == "são paulo":
            return "Ensolarado, máxima de 28°C."
        return "Previsão indisponível para esta cidade no momento."
```

O agente pode então usar essa ferramenta quando apropriado, a partir das instruções e da compreensão do modelo.

#### Callbacks

Callbacks são “ganchos” que permitem:

- Monitorar o ciclo de vida das chamadas (antes/depois de cada request ao LLM ou ferramenta).  
- Registrar logs, métricas e traces.  
- Implementar regras adicionais (ex.: bloquear respostas com certos conteúdos).

Usos comuns:

- **Logging detalhado** (prompt, resposta, ferramentas usadas).  
- **Métricas de performance** (latência, tokens usados, custo).  
- **Auditoria** (quem pediu o quê, e o que foi respondido).

---

## 4. Ecossistema e Arquitetura do ADK

### 4.1 Componentes principais: Agents, Tools, Memory, Callbacks

#### 4.1.1 Agents (Agentes)

- São o **coração da aplicação**.  
- Representam “personalidades” ou “funções” específicas:  
  - Agente de atendimento,  
  - Agente pesquisador,  
  - Agente planejador, etc.

Cada agente define:

- **Instruções (system prompt)** – papel, tom, regras.  
- **Modelo** – qual LLM será usado.  
- **Ferramentas** – o que ele pode executar no “mundo real”.  
- **Memória** – o que ele recorda de interações anteriores.

#### 4.1.2 Tools (Ferramentas)

- São **capacidades adicionais** além de gerar texto.  
- Permitem que o agente:
  - Leia/escreva em bancos de dados,  
  - Faça chamadas HTTP,  
  - Execute scripts,  
  - Interaja com sistemas da empresa.

Ferramentas bem descritas permitem que o LLM decida **quando e como** chamá-las.

#### 4.1.3 Memory (Memória)

Responsável por:

- Armazenar histórico de conversas.  
- Recuperar informações relevantes para o contexto atual.  
- Suportar padrões como RAG (busca + geração).

Pode ser:

- Simples (apenas histórico recente).  
- Vetorial (base de conhecimento).  
- Híbrida (combinação de ambas).

#### 4.1.4 Callbacks

- “Ouvidos” do sistema: observam o que está acontecendo.  
- Podem registrar:
  - Prompt enviado,  
  - Resposta recebida,  
  - Ferramentas chamadas,  
  - Erros e exceções.

Essencial para:

- Debugging.  
- Observabilidade.  
- Governança e compliance.

---

### 4.2 Fluxo de execução e ciclo de vida de um agente no ADK

Vamos descrever um fluxo típico quando um usuário faz uma pergunta a um agente.

1. **Recebimento da Solicitação**
   - A aplicação (ex.: API REST) recebe uma entrada do usuário.
   - Essa entrada é encaminhada ao agente adequado.

2. **Construção do Contexto**
   - O agente reúne:
     - Instruções do sistema (role do agente).  
     - Histórico relevante (via Memory).  
     - A mensagem atual do usuário.

3. **Interpretação e Planejamento (via LLM)**
   - O modelo avalia:
     - “Preciso chamar uma ferramenta?”  
     - “Basta responder diretamente?”  
     - “Devo buscar algo na memória de longo prazo?”

4. **Chamadas de Ferramentas (se necessário)**
   - O LLM emite uma “ação” de ferramenta (ex.: chamar GetWeatherTool).  
   - O ADK executa a ferramenta, captura o resultado.  
   - O resultado volta como input adicional ao LLM.

5. **Geração da Resposta Final**
   - Com base em:
     - Contexto,  
     - Resultado de ferramentas (se houve),  
     - Regras de segurança,
   - O modelo gera a resposta final.

6. **Atualização da Memória**
   - O ADK pode:
     - Armazenar a nova interação (pergunta + resposta).  
     - Atualizar preferências do usuário.  
     - Indexar novos documentos ou insights.

7. **Registro via Callbacks**
   - Antes/depois das chamadas, os callbacks:
     - Logam informações,  
     - Enviam métricas,  
     - Podem aplicar políticas.

8. **Resposta para a Aplicação**
   - A resposta final é enviada de volta para a camada de aplicação.  
   - A aplicação retorna ao usuário (ex.: via API, interface web, chatbot, etc.).

#### Diagrama de Fluxo (Texto)

```
Usuário
  |
  v
Sua Aplicação (API, Web, Bot)
  |
  v
[ Agente ADK ]
  1. Recebe input
  2. Consulta MEMÓRIA (histórico, conhecimento)
  3. Constrói prompt completo
  4. Envia para LLM (via Model Adapter)
      |
      v
    [LLM]
      |
      v
   Decide:
     - Responder direto
     - ou Chamar TOOL
           |
           v
        [TOOL] -----> APIs externas / BD / serviços
           |
           v
   Incorpora resultado da TOOL
      |
      v
   Gera resposta final
  5. Atualiza MEMÓRIA
  6. Callbacks registram logs/métricas
  7. Envia resposta para a aplicação
      |
      v
   Usuário recebe resposta
```

---

### 4.3 Integração com APIs externas (exemplos práticos)

Um dos grandes valores de um agente é **fazer coisas** no mundo real, não apenas “falar”. Isso é feito via Tools integradas a APIs externas.

#### Exemplo 1: Consulta de dados de um sistema de pedidos

Cenário: um agente de atendimento precisa consultar status de pedidos em um sistema interno.

1. **Definir a Tool**

```python
from google_adk import Tool
import requests

class GetOrderStatusTool(Tool):
    name = "get_order_status"
    description = "Consulta o status de um pedido pelo ID."

    def execute(self, order_id: str) -> str:
        # Exemplo de chamada a uma API interna de pedidos
        url = f"https://api.minhaempresa.com/pedidos/{order_id}"
        response = requests.get(url, timeout=5)

        if response.status_code != 200:
            return "Não foi possível obter o status do pedido no momento."

        data = response.json()
        status = data.get("status", "desconhecido")
        entrega_prevista = data.get("data_entrega_prevista", "não informada")

        return f"O pedido {order_id} está com status '{status}', " \
               f"com entrega prevista para {entrega_prevista}."
```

2. **Criar o agente que usa essa Tool**

```python
from google_adk import Agent
from google_adk.models import GeminiModel

customer_service_agent = Agent(
    name="AtendenteVirtual",
    model=GeminiModel("gemini-3.0-pro"),
    instructions="""
    Você é um atendente virtual. Ajude o cliente a consultar o status de seus pedidos.
    Sempre que o cliente mencionar um número de pedido, use a ferramenta adequada.
    """,
    tools=[GetOrderStatusTool()]
)
```

3. **Fluxo de uso**

```python
pergunta = "Quero saber o status do meu pedido 12345."
resposta = customer_service_agent.run(pergunta)
print(resposta.content)
```

O LLM:

- Entende que precisa consultar o pedido 12345.  
- Decide usar a ferramenta `get_order_status`.  
- O ADK executa a tool, obtém dados da API.  
- O modelo então formata a resposta final ao usuário.

#### Exemplo 2: Integração com sistema legado (ex.: WebMethods ou outro ESB)

Mesmo que os sistemas sejam antigos, desde que exista um **endpoint de integração** (REST, SOAP, fila, etc.), o ADK pode:

- Envolver essas chamadas em Tools.  
- Permitir que o agente atue como **fachada inteligente** para sistemas legados.

Benefícios:

- Você consegue “modernizar” a interface com sistemas antigos, sem reescrevê-los.  
- Usuários interagem com um **agente natural**, que por baixo faz chamadas a WebMethods, WebLogic, etc.

---

## 5. Conectando o Módulo aos Resultados Esperados

### 5.1 Compreensão clara dos conceitos fundamentais

Ao longo deste módulo, o participante:

- Viu o que é o Google ADK e por que ele existe.  
- Entendeu a arquitetura modular e seus componentes principais (Agents, Tools, Memory, Callbacks).  
- Percebeu como o ADK organiza agentes, ferramentas e memória de forma coerente.

Sugestão para reforço:

- Pedir aos participantes que expliquem com suas próprias palavras:
  - “O que é um agente no ADK?”  
  - “Qual a diferença entre ferramenta e memória?”  
  - “Como o ADK conversa com diferentes LLMs?”

### 5.2 Capacidade de avaliar quando utilizar o ADK

Após entender o modelo mental do ADK, o participante deve ser capaz de reconhecer cenários como:

- Quando NÃO usar ADK:
  - Scripts simples, one-off, chamadas testando apenas uma API de LLM.  
  - Protótipos muito pequenos, sem necessidade de manutenção.

- Quando usar ADK:
  - Aplicações que precisarão **crescer** (mais agentes, mais ferramentas).  
  - Projetos que exigem **manutenibilidade e governança**.  
  - Casos com **multiagentes, memória, RAG, integração complexa com sistemas**.  
  - Quando há potencial de **mudar de provedor de LLM** (custo, qualidade, política).

Atividade sugerida (10–15 min):

- Em grupos, listar 3 projetos da empresa/área onde ADK faria sentido.  
- Discutir brevemente por quê em cada caso.

### 5.3 Habilidade de identificar casos de uso aplicáveis

Exemplos de casos de uso típicos para ADK:

1. **Atendimento ao Cliente Omnichannel**
   - Agentes especializados por assunto (faturamento, logística, suporte técnico).  
   - Tools integradas a CRM, ERP, sistema de pedidos.  
   - Memória de histórico de interações.

2. **Assistentes Internos de TI / DevOps**
   - Agente que consulta logs, status de serviços, pipelines.  
   - Tools para chamar APIs de monitoria, Kubernetes, CI/CD.  
   - Callbacks para registrar ações tomadas.

3. **Orquestração de Processos de Negócio**
   - Vários agentes para etapas de um fluxo (triagem, análise, aprovação).  
   - Padrão sequencial ou paralelo.  
   - Memória para manter o estado do caso em andamento.

4. **Suporte à Migração de Sistemas / Nuvem**
   - Agentes que mapeiam serviços atuais, sugerem de/para (por exemplo, Azure → Huawei Cloud).  
   - Tools para consultar inventários, CMDB, documentações.  
   - Memória de decisões de arquitetura e justificativas.

Atividade rápida (5–10 min):

- Peça aos participantes que escrevam um caso de uso real onde gostariam de aplicar ADK (pessoal ou da empresa).  
- Em seguida, pergunte:
  - “Quais agentes você criaria?”  
  - “Quais ferramentas seriam necessárias?”  
  - “Qual tipo de memória faria sentido?”

---

## 6. Encerramento do Módulo 1

### 6.1 Recapitulando os pontos chave

- **Google ADK** é um framework open-source focado em agentes de IA.  
- Ele traz uma **arquitetura modular** com Agents, Tools, Memory, Callbacks.  
- É **model-agnostic**, suportando Gemini, OpenAI, Anthropic e outros.  
- Simplifica **gestão de contexto e memória**, além da integração com APIs externas.  
- Ajuda a construir sistemas robustos, **multiagente**, escaláveis e observáveis.

### 6.2 Preparação para o próximo módulo

No **Módulo 2**, os participantes:

- Configurarão o ambiente Python e o ADK.  
- Rodarão o primeiro tutorial do repositório (ex.: `1_starter_agent`).  
- Validarão que tudo está pronto para construir o primeiro agente completo no Módulo 3.

Sugestão de “tarefa de casa” entre os módulos:

- Ler o README do repositório do curso Google ADK.  
- Anotar dúvidas sobre arquitetura e componentes para discutir no início do Módulo 2.  
- Esboçar um diagrama (mesmo que simples) do seu futuro sistema com ADK (quais agentes, quais ferramentas, quais integrações).

Com isso, o Módulo 1 cumpre seu papel de criar uma base conceitual sólida para todo o restante do treinamento.