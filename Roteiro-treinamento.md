### Roteiro Detalhado – Treinamento "Desenvolvendo Agentes de IA com o Google ADK e Gemini 3 Pro"

***

#### **Módulo 1: Introdução ao Google ADK e Agentes de IA**

- **Objetivo:**
    - Apresentar o conceito, propósito e diferenciais do Google ADK.
    - Discutir arquitetura, recursos principais e integração com modelos (Gemini, OpenAI, Anthropic).

- **Atividades:**
    - Aula teórica sobre o que é o Google ADK e seus principais recursos.
    - Discussão em grupo: vantagens do framework versus soluções convencionais.
    - Análise do README.md do repositório; identificação dos tópicos chave.

- **Instruções para o instrutor:**
    - Utilize slides para ilustrar arquitetura, recursos e ecossistema.
    - Estimule perguntas como: "Quais características do ADK podem acelerar projetos de IA na sua empresa?".
    - Sugira desafios: peça aos participantes que listem possíveis aplicações multiagente para seu contexto.

- **Materiais:**
    - Slides introdutórios.
    - README.md do repositório.
    - Links oficiais ([Google ADK Documentation](https://google.github.io/adk-docs/)).

***

#### **Módulo 2: Ambiente e Configuração**

- **Objetivo:**
    - Garantir que todos possuem ambiente pronto para prática.
    - Familiarizar com estrutura dos tutoriais e navegação pelo repositório.

- **Atividades:**
    - Demonstração ao vivo da instalação do Python 3.11+ e obtenção da API Key no Google AI Studio.
    - Execução dos requisitos de ambientes por meio do arquivo requirements.txt.
    - Tour pelo diretório do crash course, apresentando a organização dos tutoriais.

- **Instruções para o instrutor:**
    - Mostre passo a passo como instalar dependências.
    - Oriente sobre como verificar se tudo está rodando corretamente.
    - Pergunte: "Qual dificuldade vocês têm ao configurar ambientes Python?".

- **Materiais:**
    - Guia prático de instalação.
    - requirements.txt de cada tutorial.
    - Notebook de apoio.

***

#### **Módulo 3: Criando o Primeiro Agente**

- **Objetivo:**
    - Ensinar a criar pacotes básicos do Google ADK e implementar um agente simples com Gemini 3 Pro.

- **Atividades:**
    - Apresentação teórica do fluxo de agentes no ADK.
    - Execução e análise do tutorial 1_starter_agent.
    - Exercício: modificar uma configuração do agente e observar diferenças na resposta.

- **Instruções para o instrutor:**
    - Explique cada etapa do código, referenciando a documentação.
    - Sugira: "Tentem alternar entre modelos Gemini e outros disponíveis e comparem as respostas".

- **Materiais:**
    - Código-fonte 1_starter_agent.
    - Notebook ou ambiente IDE preparado.
    - Slides com exemplos visuais.

***

#### **Módulo 4: Modelos, Ferramentas e Plugins**

- **Objetivo:**
    - Demonstrar como usar e trocar modelos (OpenAI, Anthropic) e integrar ferramentas/plugins ao agente.

- **Atividades:**
    - Apresentação teórica sobre o conceito “model-agnostic”.
    - Execução dos tutoriais 2_model_agnostic_agent, 3_structured_output_agent e 4_tool_using_agent.
    - Exercício prático: integrar uma ferramenta customizada ou plugin.

- **Instruções para o instrutor:**
    - Compare saídas dos agentes com diferentes modelos.
    - Proponha o desafio: "Escolha uma ferramenta e integre ao agente, documentando o processo".
    - Exemplifique possíveis erros e como solucioná-los.

- **Materiais:**
    - Tutoriais: 2_model_agnostic_agent, 3_structured_output_agent, 4_tool_using_agent.
    - Slides explicativos.
    - Links para documentação dos plugins utilizados.

***

#### **Módulo 5: Funcionalidades Avançadas**

- **Objetivo:**
    - Ensinar recursos de memória, callbacks e abordagem multiagente sequencial, paralela ou em loop.

- **Atividades:**
    - Execução dos tutoriais: 5_memory_agent (gestão de contexto e persistência), 6_callbacks (monitoramento e ciclos de vida).
    - Discussão sobre casos práticos com uso de memória.
    - Demonstração dos padrões multiagente: 8_simple_multi_agent e 9_multi_agent_patterns.

- **Instruções para o instrutor:**
    - Pedir que os participantes criem um agente que armazene e recupere contexto de conversas.
    - Propor como desafio: "Implemente um loop de refinamento utilizando o padrão Loop Agent".

- **Materiais:**
    - Tutoriais: 5_memory_agent, 6_callbacks, 8_simple_multi_agent, 9_multi_agent_patterns.
    - Slides esquemáticos de processos multiagente.
    - Exercícios de codificação.

***

#### **Módulo 6: Padrões de Aplicações Multiagente**

- **Objetivo:**
    - Aplicar padrões práticos e avançados para operações multiagente e analisar desempenho.

- **Atividades:**
    - Análise do tutorial 9_multi_agent_patterns.
    - Exercício prático: criação de workflows baseados em agentes sequenciais, paralelos ou loops.
    - Discussão sobre métricas de desempenho e estratégias de ajuste.

- **Instruções para o instrutor:**
    - Mostre diferentes cenários de aplicação dos padrões multiagente.
    - Estimule reflexões: "Que vantagens/mudanças o padrão paralelo traz ao fluxo da aplicação?".

- **Materiais:**
    - tutorial 9_multi_agent_patterns.
    - Slides sobre padrões de workflow.
    - Links para métricas e boas práticas de avaliação.

***

#### **Módulo 7: Segurança, Deployment e Boas Práticas**

- **Objetivo:**
    - Capacitar sobre princípios de segurança, deployment e melhores práticas de manutenção.

- **Atividades:**
    - Apresentação das ações de segurança embutidas no ADK.
    - Demonstração do deployment utilizando containers.
    - Exercício: preparar um agente para deployment seguro.

- **Instruções para o instrutor:**
    - Apresente exemplos de falhas comuns de segurança e como mitigá-las.
    - Oriente sobre checklist de deployment e documentação de código.
    - Estimule perguntas: "Quais aspectos de segurança vocês consideram críticos ao publicar um agente?".

- **Materiais:**
    - Slides sobre segurança e conformidade.
    - Scripts de exemplo para deployment.
    - Checklist de boas práticas.

***

#### **Metodologias de Ensino Comuns**

- Aulas expositivas com slides e demonstrações ao vivo.
- Atividades práticas baseadas em tutoriais do repositório.
- Discussões em grupo e estudos de caso.
- Exercícios orientados e desafios práticos.
- Sessões de dúvidas e feedback contínuo.

***

#### **Materiais Gerais de Apoio**

- Slides detalhados para cada módulo.
- Vídeos e screencasts demonstrativos dos principais tutoriais.
- Códigos e README dos tutoriais do repositório ([1_starter_agent] ... [9_multi_agent_patterns]).
- Links:
    - [Google ADK Documentation](https://google.github.io/adk-docs/)
    - [Google AI Studio](https://aistudio.google.com/)
    - [Gemini API Reference](https://ai.google.dev/docs)
    - [Pydantic Documentation](https://docs.pydantic.dev/)
- Apostilas resumo e conjuntos de exercícios práticos.

***

#### **Avaliação**

- Questionários curtos ao final de cada módulo.
- Testes práticos: executar, modificar e documentar tutoriais.
- Projeto final em grupo: desenvolvimento de agente multiagente funcional.
- Relatórios reflexivos individuais.

***

Este roteiro assegura uma formação extremamente prática, orientada por exemplos reais e alinhada à documentação oficial do Google ADK e Gemini 3 Pro, preparando os participantes para aplicar agentes de IA em variados contextos de negócio, pesquisa ou produto.[1]

[1](https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/ai_agent_framework_crash_course/google_adk_crash_course)