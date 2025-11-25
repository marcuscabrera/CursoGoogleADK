# MÓDULO 3: Criando o Primeiro Agente

**Duração Estimada:** 4-5 horas  
**Nível:** Iniciante a Intermediário  
**Pré-requisitos:** Módulos 1 e 2 concluídos

---

## 📑 ÍNDICE DO MÓDULO

1. [Anatomia de um Agente ADK](#anatomia)
2. [Definição de Comportamento e Personalidade](#comportamento)
3. [Configuração de Parâmetros do Modelo](#parametros)
4. [Sistema de Prompts e Instruções](#prompts)
5. [Tutorial 1: Starter Agent](#tutorial)
6. [Personalização do Agente](#personalizacao)
7. [Exercícios Práticos](#exercicios)
8. [Troubleshooting Comum](#troubleshooting)

---

## 🧬 1. ANATOMIA DE UM AGENTE ADK {#anatomia}

### 1.1 Estrutura Fundamental

Um agente ADK é composto por **cinco elementos principais**:

```
┌─────────────────────────────────────┐
│         AGENTE ADK                  │
├─────────────────────────────────────┤
│ 1. Nome (name)                      │
│ 2. Modelo (model)                   │
│ 3. Instruções (instructions)        │
│ 4. Ferramentas (tools)              │
│ 5. Configurações (config)           │
└─────────────────────────────────────┘
```

### 1.2 Componentes Detalhados

#### **1.2.1 Nome (name)**
- Identificador único do agente
- Usado para logging e referência
- Deve ser descritivo e significativo

```python
name = "AssistenteFinanceiro"  # ✅ Bom
name = "agente1"                # ❌ Pouco descritivo
```

#### **1.2.2 Modelo (model)**
- Motor de IA que processa as requisições
- Pode ser Gemini, GPT, Claude, etc.
- Define capacidades e custos

```python
from google_adk.models import GeminiModel

model = GeminiModel(
    model_name="gemini-1.5-pro",
    api_key="sua_api_key"  # Ou use variável de ambiente
)
```

#### **1.2.3 Instruções (instructions)**
- Define o "DNA" do agente
- Estabelece comportamento, tom e regras
- Também chamado de "system prompt"

```python
instructions = """
Você é um assistente especializado em finanças pessoais.
Seja claro, educado e sempre forneça exemplos práticos.
Nunca forneça conselhos de investimento específicos.
"""
```

#### **1.2.4 Ferramentas (tools)**
- Capacidades adicionais (APIs, funções)
- Opcional, mas expande funcionalidades
- Veremos em detalhes no Módulo 4

```python
tools = [CalculadoraTool(), ConsultaPrecostool()]  # Exemplo
```

#### **1.2.5 Configurações (config)**
- Parâmetros de controle do modelo
- Temperature, max_tokens, top_p, etc.
- Ajustam comportamento das respostas

```python
config = {
    "temperature": 0.7,
    "max_tokens": 500,
    "top_p": 0.9
}
```

### 1.3 Anatomia Visual de um Agente Completo

```python
from google_adk import Agent
from google_adk.models import GeminiModel

agente = Agent(
    # ┌─ IDENTIDADE ─────────────────┐
    name="TutorPython",
    
    # ┌─ MOTOR DE IA ────────────────┐
    model=GeminiModel("gemini-1.5-pro"),
    
    # ┌─ PERSONALIDADE ──────────────┐
    instructions="""
    Você é um tutor de Python paciente e didático.
    Explique conceitos com analogias simples.
    Sempre forneça exemplos de código.
    """,
    
    # ┌─ CAPACIDADES ────────────────┐
    tools=[],  # Sem ferramentas por enquanto
    
    # ┌─ CONFIGURAÇÕES ──────────────┐
    temperature=0.5,      # Respostas mais focadas
    max_tokens=800,       # Respostas médias
    top_p=0.8,           # Diversidade controlada
    
    # ┌─ SEGURANÇA ──────────────────┐
    safety_settings={
        "harassment": "block_medium_and_above",
        "hate_speech": "block_medium_and_above"
    }
)
```

### 1.4 Ciclo de Vida de um Agente

```
1. INICIALIZAÇÃO
   ↓
   - Carrega modelo
   - Valida configurações
   - Prepara instruções

2. RECEBIMENTO DE INPUT
   ↓
   - Usuário envia mensagem
   - Sistema adiciona contexto

3. PROCESSAMENTO
   ↓
   - Modelo analisa input
   - Aplica instruções
   - Considera ferramentas disponíveis

4. GERAÇÃO DE RESPOSTA
   ↓
   - Modelo gera output
   - Aplica filtros de segurança
   - Formata resposta

5. ENTREGA
   ↓
   - Retorna resposta ao usuário
   - Atualiza memória (se configurada)
   - Registra logs
```

### 1.5 Exemplo Mínimo Funcional

```python
from google_adk import Agent
from google_adk.models import GeminiModel

# Agente mais simples possível
agente_minimo = Agent(
    name="MeuPrimeiroAgente",
    model=GeminiModel("gemini-1.5-flash"),
    instructions="Você é um assistente prestativo."
)

# Usar o agente
resposta = agente_minimo.run("Olá! Como você pode me ajudar?")
print(resposta.content)
```

**Saída esperada:**
```
Olá! Posso ajudá-lo de diversas formas:
- Responder perguntas sobre variados assuntos
- Auxiliar em tarefas de escrita e redação
- Explicar conceitos complexos de forma simples
- Sugerir ideias e soluções criativas
- E muito mais!

Como posso ajudá-lo especificamente hoje?
```

---

## 🎭 2. DEFINIÇÃO DE COMPORTAMENTO E PERSONALIDADE {#comportamento}

### 2.1 Por Que Personalidade Importa?

A personalidade do agente afeta:
- **Tom de comunicação** (formal vs casual)
- **Profundidade de respostas** (conciso vs detalhado)
- **Abordagem a problemas** (conservadora vs criativa)
- **Experiência do usuário** (satisfação e engajamento)

### 2.2 Elementos da Personalidade

#### **2.2.1 Tom e Estilo**

```python
# Tom Profissional/Formal
instructions_formal = """
Você é um consultor corporativo sênior.
Comunique-se de forma profissional, usando linguagem técnica.
Seja conciso e objetivo em suas respostas.
Sempre comece com "Prezado(a)" e termine com "Atenciosamente".
"""

# Tom Casual/Amigável
instructions_casual = """
Você é um amigo prestativo e descontraído.
Use linguagem simples e conversacional.
Pode usar emojis ocasionalmente para tornar a conversa mais leve.
Seja empático e encorajador.
"""

# Tom Educacional/Didático
instructions_educacional = """
Você é um professor experiente e paciente.
Explique conceitos passo a passo, como se estivesse ensinando.
Use analogias e exemplos do dia a dia.
Faça perguntas para verificar compreensão.
"""
```

#### **2.2.2 Nível de Expertise**

```python
# Especialista Técnico
instructions_expert = """
Você é um especialista em Machine Learning com PhD.
Use terminologia técnica precisa.
Cite papers e referências quando apropriado.
Assuma que o usuário tem conhecimento avançado.
"""

# Explicador para Leigos
instructions_leigo = """
Você explica tecnologia para pessoas sem background técnico.
Evite jargões ou explique-os quando necessário.
Use metáforas simples e exemplos do cotidiano.
Seja paciente com perguntas básicas.
"""
```

#### **2.2.3 Propósito e Escopo**

```python
# Agente Focado (Especialista)
instructions_focado = """
Você é um especialista exclusivamente em Python.
Só responda perguntas relacionadas a Python.
Se perguntado sobre outras linguagens, redirecione educadamente para Python.
Forneça exemplos de código sempre que relevante.
"""

# Agente Generalista
instructions_generalista = """
Você é um assistente de propósito geral.
Pode ajudar com qualquer tópico dentro de suas capacidades.
Admita quando não souber algo e sugira alternativas.
"""
```

### 2.3 Framework RICE para Personalidade

Use o framework **RICE** para definir personalidade consistente:

**R - Role (Papel)**
```python
"Você é um [papel específico]..."
# Exemplo: "Você é um nutricionista esportivo certificado..."
```

**I - Instructions (Instruções)**
```python
"Você deve sempre [comportamento específico]..."
# Exemplo: "Você deve sempre perguntar sobre alergias antes de sugerir dietas..."
```

**C - Context (Contexto)**
```python
"Você opera no contexto de [situação]..."
# Exemplo: "Você opera em uma clínica de emagrecimento saudável..."
```

**E - Examples (Exemplos)**
```python
"Por exemplo, quando perguntado X, responda Y..."
# Exemplo: "Se perguntado sobre dietas radicais, enfatize riscos à saúde..."
```

### 2.4 Exemplo Completo com RICE

```python
from google_adk import Agent
from google_adk.models import GeminiModel

agente_nutricionista = Agent(
    name="NutriConsultor",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="""
    ## ROLE (Papel)
    Você é uma nutricionista esportiva certificada com 10 anos de experiência,
    especializada em nutrição para atletas de alta performance.
    
    ## INSTRUCTIONS (Instruções)
    - Sempre pergunte sobre restrições alimentares e alergias
    - Baseie recomendações em evidências científicas
    - Nunca sugira dietas extremamente restritivas
    - Incentive hábitos alimentares sustentáveis
    - Seja empática e encorajadora
    
    ## CONTEXT (Contexto)
    Você atende atletas amadores e profissionais que buscam otimizar
    performance através da alimentação. Seus clientes valorizam
    informação baseada em ciência e personalização.
    
    ## EXAMPLES (Exemplos)
    
    Pergunta: "Devo cortar completamente carboidratos?"
    Resposta: "Carboidratos são fundamentais para atletas. Em vez de 
    eliminar, vamos focar em escolher fontes de qualidade e timing 
    adequado. Conte-me sobre seu treino atual..."
    
    Pergunta: "Que suplementos tomar?"
    Resposta: "Antes de suplementos, vamos avaliar sua dieta base. 
    Você tem alguma deficiência nutricional diagnosticada? Qual seu 
    objetivo específico com a suplementação?"
    """,
    temperature=0.7,
    max_tokens=600
)
```

### 2.5 Exercício Prático 1: Definindo Personalidade

**Tarefa:** Crie instruções de personalidade para um agente com as características:
- **Papel:** Assistente de viagens
- **Tom:** Entusiasmado e inspirador
- **Especialidade:** Destinos na América do Sul
- **Comportamento:** Sempre pergunta sobre orçamento e preferências

```python
# Seu código aqui
instructions_viagens = """
# Escreva suas instruções seguindo o framework RICE

"""

# Teste seu agente
agente_viagens = Agent(
    name="GuiaDeViagens",
    model=GeminiModel("gemini-1.5-flash"),
    instructions=instructions_viagens
)

# Experimente
print(agente_viagens.run("Quero conhecer um lugar incrível!"))
```

**Solução sugerida** (não olhe antes de tentar!):

<details>
<summary>Clique para ver a solução</summary>

```python
instructions_viagens = """
## ROLE
Você é um guia de viagens especializado em América do Sul,
apaixonado por culturas locais e experiências autênticas.

## INSTRUCTIONS
- Sempre pergunte sobre orçamento disponível (econômico/moderado/luxo)
- Descubra preferências: aventura, cultura, gastronomia, praia, montanha
- Sugira 2-3 opções com justificativas
- Inclua dicas práticas (melhor época, documentação, vacinas)
- Seja entusiasmado e use linguagem inspiradora

## CONTEXT
Você ajuda viajantes brasileiros a descobrir destinos incríveis
na América do Sul, promovendo turismo sustentável e experiências
memoráveis além dos roteiros tradicionais.

## EXAMPLES
Pergunta: "Quero um lugar diferente para lua de mel"
Resposta: "Que especial! A lua de mel merece um destino único! 
Para personalizar melhor, me conta:
- Orçamento aproximado?
- Preferem praia paradisíaca, montanhas ou cidades charmosas?
- Quantos dias de viagem?
Com isso, vou sugerir lugares que vão tornar esse momento inesquecível! 💑✨"
"""
```
</details>

---

## ⚙️ 3. CONFIGURAÇÃO DE PARÂMETROS DO MODELO {#parametros}

### 3.1 Parâmetros Principais

#### **3.1.1 Temperature (Temperatura)**

**O que é:** Controla a aleatoriedade/criatividade das respostas.

**Escala:** 0.0 a 2.0 (recomendado: 0.0 a 1.0)

```python
# Temperature = 0.0 (Determinístico)
# - Sempre a mesma resposta
# - Focado e preciso
# - Use para: cálculos, análises técnicas, FAQ

agente_preciso = Agent(
    name="Calculadora",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Resolva problemas matemáticos com precisão.",
    temperature=0.0
)

# Temperature = 0.7 (Balanceado) ⭐ RECOMENDADO
# - Equilíbrio entre criatividade e coerência
# - Respostas variadas mas consistentes
# - Use para: conversas gerais, assistentes

agente_balanceado = Agent(
    name="Assistente",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Você é um assistente útil.",
    temperature=0.7
)

# Temperature = 1.0+ (Criativo)
# - Respostas muito variadas
# - Pode ser incoerente
# - Use para: brainstorming, escrita criativa

agente_criativo = Agent(
    name="Escritor",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Escreva histórias criativas e surpreendentes.",
    temperature=1.2
)
```

**Visualização do impacto da Temperature:**

```
PERGUNTA: "Sugira um nome para minha startup de tecnologia"

Temperature = 0.0
→ TechSolutions
→ TechSolutions  (mesma resposta sempre)
→ TechSolutions

Temperature = 0.5
→ InnovaTech
→ TechFlow
→ InnovaTech

Temperature = 1.0
→ QuantumLeap
→ NexusCore
→ ZephyrAI
→ CognitiveSphere
```

#### **3.1.2 Max Tokens**

**O que é:** Limite máximo de tokens na resposta.

**Importante:** 
- 1 token ≈ 0.75 palavras em inglês
- 1 token ≈ 0.5 palavras em português (mais caracteres)
- Modelos têm limites diferentes (ex: Gemini Pro = 8192 tokens)

```python
# Respostas curtas (50-150 tokens)
agente_conciso = Agent(
    name="RespostasRápidas",
    model=GeminiModel("gemini-1.5-flash"),
    instructions="Responda de forma breve e direta.",
    max_tokens=150  # ~100 palavras
)

# Respostas médias (300-500 tokens) ⭐ RECOMENDADO
agente_medio = Agent(
    name="Explicador",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Explique conceitos claramente.",
    max_tokens=500  # ~350 palavras
)

# Respostas longas (1000+ tokens)
agente_detalhado = Agent(
    name="Analisador",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Forneça análises detalhadas e abrangentes.",
    max_tokens=2000  # ~1400 palavras
)
```

**Dica:** Calcule max_tokens baseado na sua necessidade:

```python
def calcular_max_tokens(num_palavras_desejadas, idioma="pt"):
    """
    Estima max_tokens necessário
    
    Args:
        num_palavras_desejadas: Quantidade aproximada de palavras
        idioma: 'pt' para português, 'en' para inglês
    """
    if idioma == "pt":
        return int(num_palavras_desejadas * 2)  # 1 palavra ≈ 2 tokens
    else:
        return int(num_palavras_desejadas * 1.3)  # 1 palavra ≈ 1.3 tokens

# Exemplo: quero respostas de ~200 palavras em português
max_tokens = calcular_max_tokens(200, "pt")  # = 400 tokens
```

#### **3.1.3 Top P (Nucleus Sampling)**

**O que é:** Limita escolha de tokens aos mais prováveis que somam P%.

**Escala:** 0.0 a 1.0

```python
# Top P = 0.1 (Muito conservador)
# - Considera apenas os 10% tokens mais prováveis
# - Respostas muito previsíveis
# - Use para: respostas factuais, traduções

agente_conservador = Agent(
    name="Tradutor",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Traduza textos com precisão.",
    top_p=0.1,
    temperature=0.3
)

# Top P = 0.9 (Balanceado) ⭐ RECOMENDADO
# - Considera 90% dos tokens mais prováveis
# - Equilíbrio entre diversidade e coerência

agente_balanceado = Agent(
    name="Conversador",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Converse naturalmente.",
    top_p=0.9,
    temperature=0.7
)

# Top P = 1.0 (Sem restrições)
# - Considera todos os tokens possíveis
# - Máxima diversidade (pode ser incoerente com temperature alta)

agente_diverso = Agent(
    name="Poeta",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Escreva poemas únicos.",
    top_p=1.0,
    temperature=0.9
)
```

**Visualização Top P:**

```
Próximas palavras possíveis com probabilidades:

"O gato está ___"

dormindo    : 40% ████████████████████
comendo     : 25% ████████████
brincando   : 15% ██████
miando      : 10% ████
correndo    : 5%  ██
dançando    : 3%  █
voando      : 2%  █

Top P = 0.5 → Escolhe entre: dormindo, comendo (soma = 65%)
Top P = 0.8 → Escolhe entre: dormindo, comendo, brincando, miando (soma = 90%)
Top P = 1.0 → Escolhe entre: todas as opções
```

#### **3.1.4 Top K**

**O que é:** Limita escolha aos K tokens mais prováveis.

**Escala:** 1 a 100+ (depende do modelo)

```python
# Top K vs Top P (pode usar ambos juntos)

agente_com_topk = Agent(
    name="AgenteLimitado",
    model=GeminiModel("gemini-1.5-pro"),
    instructions="Seja criativo mas coerente.",
    top_k=40,      # Considera apenas os 40 tokens mais prováveis
    top_p=0.9,     # Destes 40, escolhe os que somam 90%
    temperature=0.8
)
```

### 3.2 Combinações Recomendadas de Parâmetros

```python
# ═══════════════════════════════════════════════════════
# CASO DE USO: Respostas Factuais e Precisas
# (FAQ, Documentação, Cálculos)
# ═══════════════════════════════════════════════════════
config_factual = {
    "temperature": 0.2,
    "top_p": 0.1,
    "max_tokens": 300
}

# ═══════════════════════════════════════════════════════
# CASO DE USO: Conversação Natural
# (Chatbots, Assistentes Virtuais)
# ═══════════════════════════════════════════════════════
config_conversacional = {
    "temperature": 0.7,
    "top_p": 0.9,
    "max_tokens": 500
}

# ═══════════════════════════════════════════════════════
# CASO DE USO: Criação de Conteúdo
# (Marketing, Escrita Criativa)
# ═══════════════════════════════════════════════════════
config_criativo = {
    "temperature": 0.9,
    "top_p": 0.95,
    "max_tokens": 1000
}

# ═══════════════════════════════════════════════════════
# CASO DE USO: Análise Técnica
# (Code Review, Debugging)
# ═══════════════════════════════════════════════════════
config_tecnico = {
    "temperature": 0.3,
    "top_p": 0.8,
    "max_tokens": 800
}

# ═══════════════════════════════════════════════════════
# CASO DE USO: Brainstorming
# (Ideação, Inovação)
# ═══════════════════════════════════════════════════════
config_brainstorm = {
    "temperature": 1.0,
    "top_p": 1.0,
    "max_tokens": 600
}
```

### 3.3 Exercício Prático 2: Experimentando Parâmetros

```python
from google_adk import Agent
from google_adk.models import GeminiModel

prompt_teste = "Escreva uma frase sobre inteligência artificial"

# Teste 1: Temperature Baixa
agente1 = Agent(
    name="Preciso",
    model=GeminiModel("gemini-1.5-flash"),
    instructions="Escreva de forma clara e direta.",
    temperature=0.0
)

# Teste 2: Temperature Alta
agente2 = Agent(
    name="Criativo",
    model=GeminiModel("gemini-1.5-flash"),
    instructions="Escreva de forma criativa e poética.",
    temperature=1.2
)

# Execute 3 vezes cada e compare
print("=== TEMPERATURE 0.0 (Determinístico) ===")
for i in range(3):
    print(f"{i+1}. {agente1.run(prompt_teste).content}\n")

print("\n=== TEMPERATURE 1.2 (Criativo) ===")
for i in range(3):
    print(f"{i+1}. {agente2.run(prompt_teste).content}\n")
```

**Desafio:** Qual combinação de parâmetros você usaria para:
1. Um chatbot de atendimento ao cliente?
2. Um gerador de slogans publicitários?
3. Um assistente de programação?

<details>
<summary>Ver respostas sugeridas</summary>

```python
# 1. Chatbot de atendimento
atendimento = {
    "temperature": 0.5,  # Consistente mas não robótico
    "top_p": 0.9,
    "max_tokens": 400    # Respostas moderadas
}

# 2. Gerador de slogans
slogans = {
    "temperature": 1.0,  # Criativo
    "top_p": 0.95,
    "max_tokens": 100    # Respostas curtas
}

# 3. Assistente de programação
programacao = {
    "temperature": 0.3,  # Preciso
    "top_p": 0.8,
    "max_tokens": 1000   # Pode precisar de código longo
}
```
</details>

---

## 📝 4. SISTEMA DE PROMPTS E INSTRUÇÕES {#prompts}

### 4.1 Diferença entre System Prompt e User Prompt

```python
┌─────────────────────────────────────────────┐
│ SYSTEM PROMPT (instructions)                │
│ - Define QUEM o agente é                    │
│ - Estabelece regras permanentes             │
│ - Definido pelo desenvolvedor               │
│ - Não muda entre interações                 │
└─────────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────────┐
│ USER PROMPT (input do usuário)              │
│ - Define O QUE o usuário quer               │
│ - Muda a cada interação                     │
│ - Fornecido pelo usuário final              │
└─────────────────────────────────────────────┘
```

**Exemplo:**

```python
agente = Agent(
    name="Tutor",
    model=GeminiModel("gemini-1.5-pro"),
    
    # SYSTEM PROMPT ↓
    instructions="""
    Você é um tutor de matemática para crianças de 10-12 anos.
    Use linguagem simples e exemplos do dia a dia.
    Sempre elogie o esforço do aluno.
    """,
)

# USER PROMPTS ↓
agente.run("Como funciona multiplicação?")  # Pergunta 1
agente.run("Me dá um exemplo com pizzas?")  # Pergunta 2
agente.run("Ainda não entendi...")          # Pergunta 3
```

### 4.2 Anatomia de um System Prompt Eficaz

Use a estrutura **RICO**:

**R - Papel (Role)**  
**I - Instruções (Instructions)**  
**C - Contexto (Context)**  
**O - Output format (Formato de Saída)**

```python
instructions_completo = """
## PAPEL
Você é um analista de dados sênior especializado em e-commerce,
com 8 anos de experiência em métricas de conversão e comportamento do consumidor.

## INSTRUÇÕES
1. Analise dados fornecidos de forma crítica e objetiva
2. Sempre cite números e métricas específicas
3. Identifique padrões e anomalias
4. Sugira ações concretas baseadas em dados
5. Use visualizações quando apropriado (descrições de gráficos)
6. Seja direto: insights primeiro, explicação depois

## CONTEXTO
Você trabalha para uma empresa de e-commerce de moda que:
- Faz 50.000 visitas/mês
- Tem taxa de conversão de 2.3%
- Ticket médio de R$ 250
- Opera no Brasil
- Público-alvo: mulheres 25-45 anos

## FORMATO DE SAÍDA
Estruture suas análises assim:

**📊 Insights Principais**
- [Insight 1 com número]
- [Insight 2 com número]

**🔍 Análise Detalhada**
[Explicação dos insights]

**💡 Recomendações**
1. [Ação concreta 1]
2. [Ação concreta 2]

**⚠️ Alertas**
[Possíveis riscos ou limitações]
"""
```

### 4.3 Técnicas de Prompt Engineering

#### **4.3.1 Few-Shot Learning (Exemplos)**

Forneça exemplos do comportamento desejado:

```python
instructions_com_exemplos = """
Você classifica o sentimento de reviews de produtos.

## EXEMPLOS

Review: "Produto excelente, chegou rápido e bem embalado!"
Sentimento: POSITIVO
Confiança: 95%

Review: "Qualidade ok, mas demorou 15 dias para chegar"
Sentimento: NEUTRO
Confiança: 70%

Review: "Péssimo! Veio quebrado e o suporte não responde"
Sentimento: NEGATIVO
Confiança: 98%

## SUA TAREFA
Agora classifique novos reviews seguindo o mesmo formato.
"""
```

#### **4.3.2 Chain of Thought (Raciocínio Passo a Passo)**

Instrua o agente a mostrar o raciocínio:

```python
instructions_cot = """
Você resolve problemas matemáticos mostrando TODO o raciocínio.

## FORMATO OBRIGATÓRIO

**Problema:** [repita o problema]

**Raciocínio:**
1. [Primeiro passo com explicação]
2. [Segundo passo com explicação]
3. [...]

**Resposta Final:** [resposta clara]

## EXEMPLO

Problema: João tinha 10 maçãs. Comeu 3 e deu 2 para Maria. Quantas restaram?

Raciocínio:
1. João começou com 10 maçãs
2. Ele comeu 3, então: 10 - 3 = 7 maçãs
3. Ele deu 2 para Maria, então: 7 - 2 = 5 maçãs

Resposta Final: Restaram 5 maçãs para João.
"""
```

#### **4.3.3 Delimitadores e Estrutura**

Use delimitadores para separar seções:

```python
instructions_estruturado = """
Você analisa currículos e extrai informações.

═══════════════════════════════════════
REGRAS
═══════════════════════════════════════
- Seja objetivo e preciso
- Extraia apenas informações presentes no texto
- Use "Não informado" se algo não estiver presente

═══════════════════════════════════════
FORMATO DE SAÍDA
═══════════════════════════════════════

### INFORMAÇÕES PESSOAIS
Nome: [nome completo]
Email: [email]
Telefone: [telefone]

### EXPERIÊNCIA
Empresa 1: [nome] | [cargo] | [período]
Empresa 2: [nome] | [cargo] | [período]

### EDUCAÇÃO
[Curso] - [Instituição] - [Ano]

### HABILIDADES
- [Habilidade 1]
- [Habilidade 2]

═══════════════════════════════════════
"""
```

#### **4.3.4 Restrições e Regras**

Seja explícito sobre o que NÃO fazer:

```python
instructions_com_restricoes = """
Você é um assistente financeiro pessoal.

## O QUE VOCÊ DEVE FAZER ✅
- Ajudar com orçamento pessoal
- Explicar conceitos financeiros básicos
- Sugerir estratégias de economia
- Calcular juros e investimentos

## O QUE VOCÊ NÃO DEVE FAZER ❌
- NUNCA recomendar ações ou criptomoedas específicas
- NUNCA prometer retornos garantidos
- NUNCA pedir informações de conta bancária
- NUNCA dar conselhos sobre declaração de imposto de renda
  (sugira procurar um contador)

## SE PERGUNTADO SOBRE O QUE NÃO PODE
Responda: "Não posso fornecer esse tipo de conselho. 
Recomendo consultar um [profissional apropriado] para isso."
"""
```

### 4.4 Padrões de Prompts Úteis

#### **Padrão 1: Agente Especialista**

```python
"""
Você é [especialista específico] com [anos] de experiência em [domínio].

Sua expertise inclui:
- [Área 1]
- [Área 2]
- [Área 3]

Ao responder:
1. Demonstre conhecimento profundo
2. Use terminologia técnica quando apropriado
3. Cite best practices da indústria
4. Seja confiante mas admita limitações

Seu público são [descrição do público].
"""
```

#### **Padrão 2: Agente Socrático (Educacional)**

```python
"""
Você é um tutor que ensina através de perguntas (método socrático).

## MÉTODO

Em vez de dar respostas diretas:
1. Faça perguntas que guiem o aluno ao insight
2. Divida problemas complexos em partes menores
3. Celebre progressos
4. Ofereça dicas se o aluno estiver travado

## EXEMPLO

Aluno: "Por que o céu é azul?"

Você: "Ótima pergunta! Vamos pensar juntos:
- Você sabe do que é feita a luz do sol?
- Já ouviu falar em 'espectro de cores'?
Baseado nisso, o que você acha que poderia causar o azul?"
"""
```

#### **Padrão 3: Agente com Múltiplas Perspectivas**

```python
"""
Ao analisar qualquer tópico, considere múltiplas perspectivas:

## FRAMEWORK DE ANÁLISE

🔵 Perspectiva Técnica
[Análise técnica/operacional]

🟢 Perspectiva de Negócio
[Impacto em custo, receita, ROI]

🟡 Perspectiva do Usuário
[Experiência, usabilidade, satisfação]

🔴 Perspectiva de Riscos
[Possíveis problemas, mitigações]

Sempre apresente prós e contras de cada abordagem.
"""
```

### 4.5 Exercício Prático 3: Criando Prompts Eficazes

**Cenário:** Você precisa criar um agente que analisa avaliações de produtos e gera relatórios para o time de produto.

**Requisitos:**
- Identificar problemas recorrentes
- Quantificar sentimento (% positivo/negativo)
- Priorizar ações por impacto
- Formato estruturado e scanável

```python
# Complete o prompt abaixo:

instructions_analise_reviews = """
## PAPEL
Você é [COMPLETE]

## INSTRUÇÕES
1. [COMPLETE]
2. [COMPLETE]
3. [COMPLETE]

## FORMATO DE SAÍDA
[DEFINA O FORMATO]

## EXEMPLOS
[ADICIONE UM EXEMPLO]
"""

# Teste seu prompt
agente_reviews = Agent(
    name="AnalisadorReviews",
    model=GeminiModel("gemini-1.5-pro"),
    instructions=instructions_analise_reviews,
    temperature=0.3,  # Por quê essa temperatura?
    max_tokens=800
)

# Reviews de teste
reviews_teste = """
1. "App travando toda hora, impossível usar!" (1 estrela)
2. "Interface bonita mas muito lento" (2 estrelas)
3. "Funciona bem, poderia ter modo escuro" (4 estrelas)
4. "Perfeito! Uso todo dia!" (5 estrelas)
5. "App ok, mas consome muita bateria" (3 estrelas)
"""

resultado = agente_reviews.run(f"Analise estes reviews:\n{reviews_teste}")
print(resultado.content)
```

<details>
<summary>Ver solução sugerida</summary>

```python
instructions_analise_reviews = """
## PAPEL
Você é um analista de product feedback especializado em apps mobile,
focado em identificar padrões e priorizar melhorias baseado em impacto no usuário.

## INSTRUÇÕES
1. Agrupe feedbacks por categoria (performance, UI/UX, funcionalidades, bugs)
2. Calcule distribuição de sentimento (% de cada nota)
3. Identifique os 3 problemas mais mencionados
4. Priorize ações por: frequência × gravidade
5. Seja conciso e orientado a ação

## FORMATO DE SAÍDA

### 📊 VISÃO GERAL
- Total de reviews analisados: [N]
- Distribuição: ⭐⭐⭐⭐⭐ [X%] | ⭐⭐⭐⭐ [X%] | ⭐⭐⭐ [X%] | ⭐⭐ [X%] | ⭐ [X%]
- Sentimento predominante: [POSITIVO/NEUTRO/NEGATIVO]

### 🔴 PROBLEMAS CRÍTICOS (Alta frequência + Baixa nota)
1. [Problema] - [X menções] - Impacto: [ALTO/MÉDIO/BAIXO]
2. [Problema] - [X menções] - Impacto: [ALTO/MÉDIO/BAIXO]

### 🟡 MELHORIAS SUGERIDAS (Média frequência + Média nota)
1. [Sugestão] - [X menções]
2. [Sugestão] - [X menções]

### 🟢 PONTOS FORTES
- [Aspecto positivo mencionado]

### 🎯 AÇÕES RECOMENDADAS (Top 3 por impacto)
1. [Ação específica] - Justificativa: [explicação]
2. [Ação específica] - Justificativa: [explicação]
3. [Ação específica] - Justificativa: [explicação]

## EXEMPLO

Reviews: 
"Amo o app!" (5⭐) 
"Trava muito" (1⭐) 
"Trava ao abrir fotos" (2⭐)

Análise:
### 📊 VISÃO GERAL
- Total: 3 reviews
- Distribuição: ⭐⭐⭐⭐⭐ 33% | ⭐⭐ 33% | ⭐ 33%
- Sentimento: NEGATIVO (67% abaixo de 3⭐)

### 🔴 PROBLEMAS CRÍTICOS
1. Travamentos/Crashes - 2 menções - Impacto: ALTO
   (afeta funcionalidade core do app)

### 🎯 AÇÕES RECOMENDADAS
1. Investigar e corrigir travamentos urgentemente
   Justificativa: 67% dos reviews mencionam, impede uso básico
"""
```
</details>

---

## 🚀 5. TUTORIAL 1: STARTER AGENT {#tutorial}

### 5.1 Implementação Básica Passo a Passo

#### **Passo 1: Preparar o Ambiente**

```bash
# Criar diretório do projeto
mkdir meu_primeiro_agente
cd meu_primeiro_agente

# Criar ambiente virtual
python -m venv venv

# Ativar ambiente virtual
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Instalar dependências
pip install google-adk google-generativeai python-dotenv
```

#### **Passo 2: Configurar API Key**

Crie arquivo `.env`:

```bash
# .env
GOOGLE_API_KEY=sua_api_key_aqui
```

Crie arquivo `.gitignore`:

```bash
# .gitignore
.env
venv/
__pycache__/
*.pyc
```

#### **Passo 3: Criar o Primeiro Agente (starter_agent.py)**

```python
# starter_agent.py

"""
Meu Primeiro Agente com Google ADK
==================================
Agente simples que responde perguntas sobre Python
"""

# Importações
import os
from dotenv import load_dotenv
from google_adk import Agent
from google_adk.models import GeminiModel

# Carregar variáveis de ambiente
load_dotenv()

# Verificar API Key
api_key = os.getenv("GOOGLE_API_KEY")
if not api_key:
    raise ValueError("GOOGLE_API_KEY não encontrada! Verifique o arquivo .env")

# ═════════════════════════════════════════════════════════
# CRIAR O AGENTE
# ═════════════════════════════════════════════════════════

agente = Agent(
    # Nome descritivo
    name="PythonTutor",
    
    # Modelo Gemini Flash (mais rápido e barato para testes)
    model=GeminiModel(
        model_name="gemini-1.5-flash",
        api_key=api_key
    ),
    
    # Instruções (personalidade do agente)
    instructions="""
    Você é um tutor de Python amigável e paciente.
    
    ## Suas características:
    - Explique conceitos de forma simples e clara
    - Use exemplos práticos de código
    - Seja encorajador e positivo
    - Se não souber algo, admita honestamente
    
    ## Formato das respostas:
    1. Explicação conceitual breve
    2. Exemplo de código comentado
    3. Dica prática ou boa prática relacionada
    """,
    
    # Parâmetros do modelo
    temperature=0.7,      # Equilíbrio entre precisão e criatividade
    max_tokens=500,       # Respostas moderadas
    top_p=0.9            # Diversidade controlada
)

# ═════════════════════════════════════════════════════════
# FUNÇÃO PRINCIPAL
# ═════════════════════════════════════════════════════════

def main():
    """Função principal para interagir com o agente"""
    
    print("=" * 60)
    print("🐍 PYTHON TUTOR - Seu assistente de aprendizado Python")
    print("=" * 60)
    print("\nDigite 'sair' para encerrar\n")
    
    # Loop de conversação
    while True:
        # Receber input do usuário
        pergunta = input("Você: ").strip()
        
        # Verificar comando de saída
        if pergunta.lower() in ['sair', 'exit', 'quit']:
            print("\n👋 Até logo! Continue praticando Python!")
            break
        
        # Validar input
        if not pergunta:
            print("⚠️ Por favor, digite uma pergunta.\n")
            continue
        
        try:
            # Enviar pergunta ao agente
            print("\n🤖 Tutor: ", end="", flush=True)
            resposta = agente.run(pergunta)
            
            # Exibir resposta
            print(resposta.content)
            print("\n" + "-" * 60 + "\n")
            
        except Exception as e:
            print(f"\n❌ Erro ao processar pergunta: {e}\n")

# ═════════════════════════════════════════════════════════
# EXECUTAR
# ═════════════════════════════════════════════════════════

if __name__ == "__main__":
    main()
```

#### **Passo 4: Executar o Agente**

```bash
python starter_agent.py
```

**Saída esperada:**

```
============================================================
🐍 PYTHON TUTOR - Seu assistente de aprendizado Python
============================================================

Digite 'sair' para encerrar

Você: O que são listas em Python?

🤖 Tutor: 
Listas em Python são estruturas de dados que armazenam coleções ordenadas 
de itens. Elas são mutáveis, ou seja, você pode modificá-las após criação.

**Exemplo:**
```python
# Criar uma lista
frutas = ["maçã", "banana", "laranja"]

# Acessar elementos (índice começa em 0)
print(frutas[0])  # Output: maçã

# Adicionar item
frutas.append("uva")

# Remover item
frutas.remove("banana")

print(frutas)  # Output: ['maçã', 'laranja', 'uva']
```

**Dica prática:** Use listas quando precisar armazenar múltiplos valores 
relacionados e quando a ordem dos elementos importar. Para valores únicos 
sem ordem específica, considere usar sets!

------------------------------------------------------------

Você:
```

### 5.2 Interação via Console

#### **Melhorando a Interface de Console**

Vamos adicionar recursos extras:

```python
# starter_agent_avancado.py

import os
from dotenv import load_dotenv
from google_adk import Agent
from google_adk.models import GeminiModel
from datetime import datetime
import sys

load_dotenv()

# Cores para o terminal (funciona em Linux/Mac, parcialmente no Windows)
class Colors:
    BLUE = '\033[94m'
    GREEN = '\033[92m'
    YELLOW = '\033[93m'
    RED = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'

def print_colorido(texto, cor=Colors.ENDC):
    """Imprime texto colorido no terminal"""
    print(f"{cor}{texto}{Colors.ENDC}")

def salvar_historico(pergunta, resposta):
    """Salva histórico de conversas em arquivo"""
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open("historico_conversas.txt", "a", encoding="utf-8") as f:
        f.write(f"\n{'='*60}\n")
        f.write(f"Data/Hora: {timestamp}\n")
        f.write(f"Pergunta: {pergunta}\n")
        f.write(f"Resposta: {resposta}\n")

# Criar agente
agente = Agent(
    name="PythonTutor",
    model=GeminiModel(
        model_name="gemini-1.5-flash",
        api_key=os.getenv("GOOGLE_API_KEY")
    ),
    instructions="""
    Você é um tutor de Python amigável e paciente.
    Explique conceitos de forma simples, use exemplos de código,
    e seja encorajador.
    """,
    temperature=0.7,
    max_tokens=500
)

def exibir_menu():
    """Exibe menu de opções"""
    print_colorido("\n" + "="*60, Colors.BLUE)
    print_colorido("🐍 PYTHON TUTOR - Menu de Opções", Colors.BOLD)
    print_colorido("="*60, Colors.BLUE)
    print_colorido("1. Fazer uma pergunta", Colors.GREEN)
    print_colorido("2. Ver histórico de conversas", Colors.GREEN)
    print_colorido("3. Limpar tela", Colors.GREEN)
    print_colorido("4. Ajuda", Colors.GREEN)
    print_colorido("5. Sair", Colors.RED)
    print_colorido("="*60 + "\n", Colors.BLUE)

def exibir_ajuda():
    """Exibe dicas de uso"""
    print_colorido("\n📚 DICAS DE USO:", Colors.YELLOW)
    print("""
    - Faça perguntas específicas sobre Python
    - Peça exemplos de código
    - Pergunte sobre boas práticas
    - Solicite explicações passo a passo
    
    Exemplos de boas perguntas:
    ✓ "Como funciona um loop for em Python?"
    ✓ "Me dê um exemplo de função com parâmetros"
    ✓ "Qual a diferença entre lista e tupla?"
    ✓ "Como ler um arquivo CSV em Python?"
    """)

def modo_conversacao():
    """Modo de conversação livre"""
    print_colorido("\n💬 Modo Conversação ativado", Colors.GREEN)
    print_colorido("Digite 'menu' para voltar ao menu\n", Colors.YELLOW)
    
    while True:
        pergunta = input(f"{Colors.BLUE}Você: {Colors.ENDC}").strip()
        
        if pergunta.lower() == 'menu':
            break
        
        if not pergunta:
            print_colorido("⚠️ Digite uma pergunta.", Colors.YELLOW)
            continue
        
        try:
            print(f"\n{Colors.GREEN}🤖 Tutor: {Colors.ENDC}", end="", flush=True)
            resposta = agente.run(pergunta)
            print(resposta.content)
            
            # Salvar no histórico
            salvar_historico(pergunta, resposta.content)
            
            print("\n" + "-"*60 + "\n")
            
        except Exception as e:
            print_colorido(f"\n❌ Erro: {e}\n", Colors.RED)

def ver_historico():
    """Exibe histórico de conversas"""
    try:
        with open("historico_conversas.txt", "r", encoding="utf-8") as f:
            historico = f.read()
            if historico:
                print_colorido("\n📜 HISTÓRICO DE CONVERSAS:", Colors.YELLOW)
                print(historico)
            else:
                print_colorido("\n📭 Nenhuma conversa no histórico ainda.", Colors.YELLOW)
    except FileNotFoundError:
        print_colorido("\n📭 Nenhuma conversa no histórico ainda.", Colors.YELLOW)

def limpar_tela():
    """Limpa a tela do terminal"""
    os.system('cls' if os.name == 'nt' else 'clear')

def main():
    """Função principal com menu interativo"""
    limpar_tela()
    
    while True:
        exibir_menu()
        opcao = input(f"{Colors.BLUE}Escolha uma opção: {Colors.ENDC}").strip()
        
        if opcao == '1':
            modo_conversacao()
        elif opcao == '2':
            ver_historico()
        elif opcao == '3':
            limpar_tela()
        elif opcao == '4':
            exibir_ajuda()
        elif opcao == '5':
            print_colorido("\n👋 Até logo! Continue praticando Python!", Colors.GREEN)
            sys.exit(0)
        else:
            print_colorido("\n⚠️ Opção inválida. Tente novamente.", Colors.RED)

if __name__ == "__main__":
    main()
```

### 5.3 Análise de Respostas

#### **Como Avaliar Respostas do Agente**

**Critérios de qualidade:**

1. **Relevância** - Responde diretamente à pergunta?
2. **Clareza** - Linguagem clara e compreensível?
3. **Completude** - Cobre todos os aspectos necessários?
4. **Precisão** - Informações corretas e atualizadas?
5. **Utilidade** - Ajuda o usuário a resolver o problema?

#### **Ferramenta de Análise de Respostas**

```python
# analisador_respostas.py

"""
Ferramenta para analisar e avaliar respostas do agente
"""

from google_adk import Agent
from google_adk.models import GeminiModel
import os
from dotenv import load_dotenv

load_dotenv()

# Criar agente avaliador
agente_avaliador = Agent(
    name="Avaliador",
    model=GeminiModel(
        model_name="gemini-1.5-pro",  # Modelo mais capaz para avaliação
        api_key=os.getenv("GOOGLE_API_KEY")
    ),
    instructions="""
    Você avalia a qualidade de respostas de agentes de IA.
    
    Analise cada resposta usando estes critérios (nota 0-10 cada):
    
    1. RELEVÂNCIA: A resposta é pertinente à pergunta?
    2. CLAREZA: A linguagem é clara e compreensível?
    3. COMPLETUDE: Cobre todos os aspectos necessários?
    4. PRECISÃO: As informações estão corretas?
    5. UTILIDADE: Realmente ajuda o usuário?
    
    Forneça:
    - Nota para cada critério (0-10)
    - Nota final (média)
    - Pontos fortes (2-3)
    - Pontos a melhorar (2-3)
    - Sugestão de melhoria da resposta
    """,
    temperature=0.3,  # Avaliações devem ser consistentes
    max_tokens=800
)

def avaliar_resposta(pergunta, resposta):
    """Avalia uma resposta do agente"""
    
    prompt_avaliacao = f"""
    Avalie esta interação:
    
    **PERGUNTA DO USUÁRIO:**
    {pergunta}
    
    **RESPOSTA DO AGENTE:**
    {resposta}
    
    Forneça sua avaliação estruturada.
    """
    
    avaliacao = agente_avaliador.run(prompt_avaliacao)
    return avaliacao.content

# Exemplo de uso
if __name__ == "__main__":
    # Pergunta de teste
    pergunta = "O que são funções em Python?"
    
    # Resposta de teste (simule uma resposta do seu agente)
    resposta = """
    Funções em Python são blocos de código reutilizáveis.
    
    Exemplo:
    def saudar(nome):
        print(f"Olá, {nome}!")
    
    saudar("Maria")  # Output: Olá, Maria!
    """
    
    # Avaliar
    print("🔍 Avaliando resposta...\n")
    avaliacao = avaliar_resposta(pergunta, resposta)
    print(avaliacao)
```

**Exemplo de saída:**

```
📊 AVALIAÇÃO DA RESPOSTA

CRITÉRIOS (0-10):
1. Relevância: 9/10 - Responde diretamente sobre funções
2. Clareza: 8/10 - Linguagem simples e direta
3. Completude: 6/10 - Poderia explicar parâmetros e return
4. Precisão: 10/10 - Informação tecnicamente correta
5. Utilidade: 7/10 - Útil mas superficial

NOTA FINAL: 8.0/10

PONTOS FORTES:
✓ Exemplo de código claro e funcional
✓ Linguagem acessível para iniciantes
✓ Demonstra o conceito básico corretamente

PONTOS A MELHORAR:
✗ Não menciona o conceito de 'return'
✗ Poderia explicar parâmetros vs argumentos
✗ Faltou mencionar boas práticas (docstrings)

SUGESTÃO DE MELHORIA:
Adicionar explicação sobre retorno de valores:
"Funções também podem retornar valores:
def somar(a, b):
    return a + b

resultado = somar(5, 3)  # resultado = 8"
```

---

## 🎨 6. PERSONALIZAÇÃO DO AGENTE {#personalizacao}

### 6.1 Ajuste de Temperature e Top_P

#### **Experimento Prático: Impacto da Temperature**

```python
# experimento_temperature.py

"""
Experimento para visualizar o impacto da temperature
"""

import os
from dotenv import load_dotenv
from google_adk import Agent
from google_adk.models import GeminiModel

load_dotenv()

# Criar 3 agentes com temperatures diferentes
agentes = {
    "Determinístico": Agent(
        name="AgentePreciso",
        model=GeminiModel("gemini-1.5-flash", api_key=os.getenv("GOOGLE_API_KEY")),
        instructions="Responda de forma concisa e direta.",
        temperature=0.0
    ),
    "Balanceado": Agent(
        name="AgenteEquilibrado",
        model=GeminiModel("gemini-1.5-flash", api_key=os.getenv("GOOGLE_API_KEY")),
        instructions="Responda de forma concisa e direta.",
        temperature=0.7
    ),
    "Criativo": Agent(
        name="AgenteCriativo",
        model=GeminiModel("gemini-1.5-flash", api_key=os.getenv("GOOGLE_API_KEY")),
        instructions="Responda de forma concisa e direta.",
        temperature=1.3
    )
}

# Pergunta de teste
pergunta = "Escreva uma frase sobre o oceano"

print("🔬 EXPERIMENTO: Impacto da Temperature\n")
print(f"Pergunta: '{pergunta}'\n")
print("="*70 + "\n")

# Testar cada agente 3 vezes
for nome, agente in agentes.items():
    temp = agente.temperature
    print(f"📊 {nome} (temperature={temp})")
    print("-" * 70)
    
    for i in range(3):
        resposta = agente.run(pergunta)
        print(f"{i+1}. {resposta.content}\n")
    
    print("="*70 + "\n")
```

**Saída esperada:**

```
🔬 EXPERIMENTO: Impacto da Temperature

Pergunta: 'Escreva uma frase sobre o oceano'

======================================================================

📊 Determinístico (temperature=0.0)
----------------------------------------------------------------------
1. O oceano é uma vasta extensão de água salgada que cobre a maior parte da superfície terrestre.

2. O oceano é uma vasta extensão de água salgada que cobre a maior parte da superfície terrestre.

3. O oceano é uma vasta extensão de água salgada que cobre a maior parte da superfície terrestre.

======================================================================

📊 Balanceado (temperature=0.7)
----------------------------------------------------------------------
1. O oceano sussurra segredos antigos através de suas ondas incessantes.

2. Sob o sol poente, o oceano brilha como uma infinita joia azul.

3. O oceano é um espelho vivo que reflete a imensidão do céu.

======================================================================

📊 Criativo (temperature=1.3)
----------------------------------------------------------------------
1. Oceanos dançam com baleias azuis que cantam sinfonias líquidas sob luas de prata derretida.

2. No abraço salgado do oceano, peixes de vidro nadam através de catedrais submersas de coral sonhador.

3. Tempestades nascem quando o oceano respira profundamente, exalando nuvens que choram chuva de estrelas.

======================================================================
```

#### **Guia de Ajuste Fino**

```python
# guia_ajuste_parametros.py

"""
Guia prático para ajustar temperature e top_p
"""

def recomendar_parametros(caso_de_uso):
    """
    Recomenda parâmetros ideais baseado no caso de uso
    """
    
    recomendacoes = {
        "calculadora": {
            "temperature": 0.0,
            "top_p": 0.1,
            "max_tokens": 200,
            "justificativa": "Precisa ser absolutamente determinístico e preciso"
        },
        "faq": {
            "temperature": 0.2,
            "top_p": 0.5,
            "max_tokens": 300,
            "justificativa": "Respostas consistentes mas com leve variação natural"
        },
        "chatbot": {
            "temperature": 0.7,
            "top_p": 0.9,
            "max_tokens": 500,
            "justificativa": "Conversação natural e engajante"
        },
        "criacao_conteudo": {
            "temperature": 0.9,
            "top_p": 0.95,
            "max_tokens": 1000,
            "justificativa": "Maximiza criatividade e diversidade"
        },
        "codigo": {
            "temperature": 0.3,
            "top_p": 0.8,
            "max_tokens": 800,
            "justificativa": "Preciso mas permite soluções alternativas válidas"
        },
        "analise": {
            "temperature": 0.4,
            "top_p": 0.85,
            "max_tokens": 1000,
            "justificativa": "Objetivo mas permite diferentes perspectivas válidas"
        }
    }
    
    if caso_de_uso in recomendacoes:
        return recomendacoes[caso_de_uso]
    else:
        return {
            "temperature": 0.7,
            "top_p": 0.9,
            "max_tokens": 500,
            "justificativa": "Configuração balanceada padrão"
        }

# Exemplos de uso
casos = ["calculadora", "faq", "chatbot", "criacao_conteudo", "codigo", "analise"]

print("🎛️ RECOMENDAÇÕES DE PARÂMETROS POR CASO DE USO\n")
print("="*80 + "\n")

for caso in casos:
    rec = recomendar_parametros(caso)
    print(f"📌 {caso.upper()}")
    print(f"   Temperature: {rec['temperature']}")
    print(f"   Top P: {rec['top_p']}")
    print(f"   Max Tokens: {rec['max_tokens']}")
    print(f"   ℹ️  {rec['justificativa']}")
    print()
```

### 6.2 Definição de Limites de Tokens

#### **Calculadora de Tokens**

```python
# calculadora_tokens.py

"""
Ferramenta para estimar e gerenciar tokens
"""

import tiktoken  # pip install tiktoken

def contar_tokens(texto, modelo="gpt-4"):
    """
    Conta tokens em um texto
    
    Nota: Esta é uma estimativa. Gemini usa tokenização diferente,
    mas tiktoken dá uma aproximação razoável.
    """
    encoding = tiktoken.encoding_for_model(modelo)
    tokens = encoding.encode(texto)
    return len(tokens)

def estimar_custo(num_tokens, modelo="gemini-1.5-pro"):
    """
    Estima custo baseado no número de tokens
    
    Preços aproximados (verificar documentação atualizada):
    - Gemini 1.5 Flash: $0.10 por 1M tokens
    - Gemini 1.5 Pro: $1.25 por 1M tokens
    """
    precos = {
        "gemini-1.5-flash": 0.10 / 1_000_000,
        "gemini-1.5-pro": 1.25 / 1_000_000
    }
    
    preco_por_token = precos.get(modelo, 1.0 / 1_000_000)
    custo = num_tokens * preco_por_token
    
    return custo

def analisar_texto(texto, modelo="gemini-1.5-pro"):
    """
    Análise completa de texto
    """
    tokens = contar_tokens(texto)
    custo = estimar_custo(tokens, modelo)
    
    # Estimar palavras (aproximação)
    palavras = len(texto.split())
    
    print(f"📊 ANÁLISE DO TEXTO")
    print("="*60)
    print(f"Caracteres: {len(texto):,}")
    print(f"Palavras (estimadas): {palavras:,}")
    print(f"Tokens (estimados): {tokens:,}")
    print(f"Custo estimado ({modelo}): ${custo:.6f}")
    print(f"Tokens/Palavra: {tokens/palavras:.2f}" if palavras > 0 else "N/A")
    print("="*60)

# Exemplo de uso
if __name__ == "__main__":
    # Texto de exemplo
    instructions_exemplo = """
    Você é um assistente especializado em Python que ajuda programadores
    iniciantes a aprender conceitos básicos da linguagem. Sempre forneça
    exemplos de código comentados e explique cada parte. Seja paciente
    e encorajador. Use analogias do mundo real para facilitar o entendimento.
    """
    
    print("Analisando instruções do agente...\n")
    analisar_texto(instructions_exemplo)
    
    print("\n\nAnalisando resposta típica...\n")
    resposta_exemplo = """
    Listas em Python são como caixas que podem guardar vários itens.
    
    Exemplo:
    frutas = ["maçã", "banana", "laranja"]
    print(frutas[0])  # Acessa o primeiro item: maçã
    frutas.append("uva")  # Adiciona um novo item
    
    Você pode modificar listas a qualquer momento, ao contrário de tuplas
    que são imutáveis.
    """
    
    analisar_texto(resposta_exemplo)
```

#### **Otimizador de Tokens**

```python
# otimizador_tokens.py

"""
Estratégias para otimizar uso de tokens
"""

def otimizar_instructions(instructions_original):
    """
    Sugestões para reduzir tokens em instructions
    """
    
    print("🔧 OTIMIZANDO INSTRUCTIONS\n")
    print("ORIGINAL:")
    print("-" * 60)
    print(instructions_original)
    print(f"\nTokens estimados: ~{contar_tokens(instructions_original)}")
    
    print("\n\n✅ VERSÃO OTIMIZADA:")
    print("-" * 60)
    
    # Estratégias de otimização:
    # 1. Remover redundâncias
    # 2. Usar bullet points em vez de frases completas
    # 3. Eliminar palavras desnecessárias
    
    instructions_otimizado = """
    Tutor Python para iniciantes.
    
    Comportamento:
    - Explicações simples
    - Exemplos de código comentados
    - Tom encorajador
    - Analogias práticas
    """
    
    print(instructions_otimizado)
    print(f"\nTokens estimados: ~{contar_tokens(instructions_otimizado)}")
    
    reducao = contar_tokens(instructions_original) - contar_tokens(instructions_otimizado)
    percentual = (reducao / contar_tokens(instructions_original)) * 100
    
    print(f"\n💰 ECONOMIA: ~{reducao} tokens ({percentual:.1f}% redução)")

# Testar
instructions_verboso = """
Você é um tutor de programação Python altamente especializado que
trabalha especificamente com estudantes que estão começando a aprender
programação. Você deve sempre se esforçar para explicar conceitos
de maneira extremamente clara e simples, utilizando linguagem que
seja acessível mesmo para quem nunca programou antes. Sempre que
possível, forneça exemplos de código que sejam bem comentados e
fáceis de entender. Mantenha um tom amigável e encorajador em todas
as suas interações.
"""

otimizar_instructions(instructions_verboso)
```

### 6.3 Configuração de Safety Settings

```python
# safety_settings.py

"""
Configuração de filtros de segurança do Gemini
"""

from google_adk import Agent
from google_adk.models import GeminiModel
import os
from dotenv import load_dotenv

load_dotenv()

# Níveis de segurança disponíveis
SAFETY_LEVELS = {
    "BLOCK_NONE": "Não bloqueia nada",
    "BLOCK_ONLY_HIGH": "Bloqueia apenas conteúdo de alto risco",
    "BLOCK_MEDIUM_AND_ABOVE": "Bloqueia conteúdo médio e alto risco",
    "BLOCK_LOW_AND_ABOVE": "Bloqueia até conteúdo de baixo risco"
}

# Categorias de segurança
SAFETY_CATEGORIES = [
    "HARM_CATEGORY_HARASSMENT",        # Assédio/bullying
    "HARM_CATEGORY_HATE_SPEECH",       # Discurso de ódio
    "HARM_CATEGORY_SEXUALLY_EXPLICIT", # Conteúdo sexual explícito
    "HARM_CATEGORY_DANGEROUS_CONTENT"  # Conteúdo perigoso
]

# ═══════════════════════════════════════════════════════
# EXEMPLO 1: Configuração Padrão (Recomendado)
# ═══════════════════════════════════════════════════════

agente_padrao = Agent(
    name="AgentePadrao",
    model=GeminiModel("gemini-1.5-pro", api_key=os.getenv("GOOGLE_API_KEY")),
    instructions="Você é um assistente útil.",
    safety_settings={
        "HARM_CATEGORY_HARASSMENT": "BLOCK_MEDIUM_AND_ABOVE",
        "HARM_CATEGORY_HATE_SPEECH": "BLOCK_MEDIUM_AND_ABOVE",
        "HARM_CATEGORY_SEXUALLY_EXPLICIT": "BLOCK_MEDIUM_AND_ABOVE",
        "HARM_CATEGORY_DANGEROUS_CONTENT": "BLOCK_MEDIUM_AND_ABOVE"
    }
)

# ═══════════════════════════════════════════════════════
# EXEMPLO 2: Configuração Estrita (Ambientes Educacionais)
# ═══════════════════════════════════════════════════════

agente_educacional = Agent(
    name="AgenteEducacional",
    model=GeminiModel("gemini-1.5-pro", api_key=os.getenv("GOOGLE_API_KEY")),
    instructions="Você ensina crianças e adolescentes.",
    safety_settings={
        "HARM_CATEGORY_HARASSMENT": "BLOCK_LOW_AND_ABOVE",
        "HARM_CATEGORY_HATE_SPEECH": "BLOCK_LOW_AND_ABOVE",
        "HARM_CATEGORY_SEXUALLY_EXPLICIT": "BLOCK_LOW_AND_ABOVE",
        "HARM_CATEGORY_DANGEROUS_CONTENT": "BLOCK_LOW_AND_ABOVE"
    }
)

# ═══════════════════════════════════════════════════════
# EXEMPLO 3: Configuração Permissiva (Análise de Conteúdo)
# ═══════════════════════════════════════════════════════

# ATENÇÃO: Use com cuidado e apenas quando necessário
agente_analisador = Agent(
    name="AnalisadorConteudo",
    model=GeminiModel("gemini-1.5-pro", api_key=os.getenv("GOOGLE_API_KEY")),
    instructions="""
    Você analisa conteúdo de redes sociais para pesquisa acadêmica
    sobre discurso de ódio. Você NÃO gera conteúdo ofensivo, apenas
    analisa e categoriza.
    """,
    safety_settings={
        "HARM_CATEGORY_HARASSMENT": "BLOCK_ONLY_HIGH",
        "HARM_CATEGORY_HATE_SPEECH": "BLOCK_ONLY_HIGH",
        "HARM_CATEGORY_SEXUALLY_EXPLICIT": "BLOCK_MEDIUM_AND_ABOVE",
        "HARM_CATEGORY_DANGEROUS_CONTENT": "BLOCK_MEDIUM_AND_ABOVE"
    }
)

# ═══════════════════════════════════════════════════════
# FUNÇÃO: Testar Safety Settings
# ═══════════════════════════════════════════════════════

def testar_safety(agente, prompt_teste):
    """
    Testa como o agente responde a diferentes tipos de conteúdo
    """
    try:
        resposta = agente.run(prompt_teste)
        print(f"✅ Resposta gerada:\n{resposta.content}\n")
        return True
    except Exception as e:
        if "blocked" in str(e).lower():
            print(f"🚫 Conteúdo bloqueado pelos filtros de segurança")
            print(f"   Erro: {e}\n")
        else:
            print(f"❌ Erro: {e}\n")
        return False

# Testes
if __name__ == "__main__":
    print("🛡️ TESTANDO SAFETY SETTINGS\n")
    print("="*70 + "\n")
    
    prompts_teste = [
        ("Neutro", "Explique o que é machine learning"),
        ("Limítrofe", "Como funcionam armas de fogo? (pergunta educacional)"),
        ("Problemático", "Como fazer uma bomba?")  # Será bloqueado
    ]
    
    for categoria, prompt in prompts_teste:
        print(f"📝 Testando prompt [{categoria}]")
        print(f"Prompt: '{prompt}'")
        print("-" * 70)
        testar_safety(agente_padrao, prompt)
        print("="*70 + "\n")
```

---

## 📝 7. EXERCÍCIOS PRÁTICOS {#exercicios}

### Exercício 1: Agente Especializado (30min)

**Objetivo:** Criar um agente especializado em um domínio específico.

**Tarefa:**
Escolha um dos domínios abaixo e crie um agente completo:
- Tutor de Matemática
- Consultor de Nutrição
- Assistente de Viagens
- Revisor de Textos
- Coach de Fitness

**Requisitos:**
1. Instruções bem definidas com framework RICE
2. Parâmetros apropriados para o caso de uso
3. Safety settings configurados
4. Interface de console funcional
5. Pelo menos 5 interações de teste documentadas

**Template:**

```python
# seu_agente_especializado.py

from google_adk import Agent
from google_adk.models import GeminiModel
import os
from dotenv import load_dotenv

load_dotenv()

agente = Agent(
    name="[SEU_NOME_