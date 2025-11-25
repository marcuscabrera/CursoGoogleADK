# MÓDULO 2: Ambiente e Configuração

**Nível:** Iniciante a Intermediário
**Tempo estimado de conclusão:** 2-3 horas
**Nível de dificuldade:** ⭐⭐☆☆☆  
**Pré-requisitos:** Conhecimento básico de linha de comando

---

## 📋 Visão Geral do Módulo

Neste módulo, você aprenderá a preparar seu ambiente de desenvolvimento para trabalhar com o Google ADK e Gemini 3 Pro. Ao final, você terá um ambiente completamente funcional e estará pronto para começar a desenvolver seus próprios agentes de IA.

---

## 1. REQUISITOS DE SISTEMA

### 1.1 Especificações Mínimas ue Recomendadas

#### **Requisitos Mínimos:**
- **Sistema Operacional:** Windows 10/11, macOS 10.15+, ou Linux (Ubuntu 20.04+)
- **Processador:** Dual-core 2.0 GHz
- **Memória RAM:** 4 GB
- **Espaço em Disco:** 2 GB livres
- **Conexão:** Internet estável (para download de dependências e chamadas à API)

#### **Requisitos Recomendados:**
- **Sistema Operacional:** Windows 11, macOS 12+, ou Linux (Ubuntu 22.04+)
- **Processador:** Quad-core 2.5 GHz ou superior
- **Memória RAM:** 8 GB ou mais
- **Espaço em Disco:** 5 GB livres
- **Conexão:** Banda larga (10 Mbps ou superior)

### 1.2 Python: Versão e Ferramentas

#### **Python 3.11 ou Superior**
O Google ADK requer Python 3.11 ou versões mais recentes devido a:
- Melhorias de performance significativas
- Suporte aprimorado a type hints (essencial para Pydantic)
- Recursos de sintaxe modernos utilizados pelo framework

**Verificando sua versão do Python:**
```bash
python --version
# ou
python3 --version
```

#### **Gerenciadores de Dependências**

**📦 pip (Gerenciador Padrão)**
- Vem instalado com Python
- Simples e direto para projetos básicos
- Ideal para iniciantes

**📦 Poetry (Recomendado para Projetos Profissionais)**
- Gerenciamento avançado de dependências
- Resolução automática de conflitos
- Criação e publicação de pacotes simplificada
- Arquivo `pyproject.toml` para configuração centralizada

**Instalando Poetry:**
```bash
# Linux/macOS/WSL
curl -sSL https://install.python-poetry.org | python3 -

# Windows (PowerShell)
(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | py -
```

### 1.3 IDEs Recomendadas

#### **🔵 Visual Studio Code (VS Code)**

**Por que escolher:**
- Gratuito e open-source
- Leve e rápido
- Excelente integração com Python e Git
- Marketplace rico em extensões

**Extensões Essenciais:**
1. **Python (Microsoft)** - Suporte completo para Python
   - IntelliSense, debugging, linting
   - Gerenciamento de ambientes virtuais
   
2. **Pylance** - Language server de alto desempenho
   - Type checking avançado
   - Auto-completação inteligente

3. **Jupyter** - Para notebooks interativos
   - Execução de células de código
   - Visualização de resultados

4. **GitLens** - Superpoderes para Git
   - Blame annotations
   - Histórico de commits

5. **Python Docstring Generator** - Automatiza documentação
   - Gera docstrings formatadas
   - Suporta múltiplos estilos

6. **Error Lens** - Destaque inline de erros
   - Visualização imediata de problemas
   - Integração com linters

**Configurações Recomendadas (settings.json):**
```json
{
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": true,
    "python.formatting.provider": "black",
    "editor.formatOnSave": true,
    "python.analysis.typeCheckingMode": "basic"
}
```

#### **🟢 PyCharm (JetBrains)**

**Por que escolher:**
- IDE profissional dedicada a Python
- Debugging avançado
- Refactoring inteligente
- Versão Community gratuita

**Plugins Úteis:**
1. **Requirements** - Gerenciamento de dependências
2. **.env files support** - Suporte para variáveis de ambiente
3. **Markdown** - Visualização e edição de documentação
4. **Rainbow Brackets** - Melhor visualização de código

**Configurações Recomendadas:**
- Ativar "Code Completion" para type hints
- Configurar formatador Black
- Habilitar "Auto-import" para otimização

---

## 2. CONFIGURAÇÃO DO AMBIENTE

### 2.1 Instalação do Python

#### **Windows:**

**Método 1: Instalador Oficial**
1. Acesse: https://www.python.org/downloads/
2. Baixe o instalador do Python 3.11+ para Windows
3. **IMPORTANTE:** Marque "Add Python to PATH" durante a instalação
4. Clique em "Install Now"
5. Aguarde a conclusão e clique em "Close"

**Verificação:**
```cmd
python --version
```

**⚠️ Problema Comum:** Python não reconhecido no CMD
- **Solução:** Adicione manualmente ao PATH:
  - Painel de Controle → Sistema → Configurações Avançadas
  - Variáveis de Ambiente → Path → Editar
  - Adicionar: `C:\Users\SeuUsuário\AppData\Local\Programs\Python\Python311`

#### **macOS:**

**Método 1: Homebrew (Recomendado)**
```bash
# Instalar Homebrew (se não tiver)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instalar Python
brew install python@3.11
```

**Método 2: Instalador Oficial**
1. Acesse: https://www.python.org/downloads/macos/
2. Baixe o instalador .pkg
3. Execute e siga as instruções

**Verificação:**
```bash
python3 --version
```

#### **Linux (Ubuntu/Debian):**

```bash
# Atualizar repositórios
sudo apt update

# Instalar Python 3.11
sudo apt install python3.11 python3.11-venv python3-pip

# Verificar instalação
python3.11 --version
```

**Para outras distribuições Linux:**
- **Fedora/RHEL:** `sudo dnf install python3.11`
- **Arch:** `sudo pacman -S python`

### 2.2 Criação de Ambiente Virtual

#### **Por que usar Ambientes Virtuais?**

Um ambiente virtual é um diretório isolado que contém:
- Uma instalação específica do Python
- Bibliotecas e dependências isoladas
- Proteção contra conflitos entre projetos

**Benefícios:**
✅ Isolamento completo de dependências  
✅ Diferentes versões de bibliotecas por projeto  
✅ Facilita reprodutibilidade  
✅ Evita poluir instalação global do Python

#### **Método 1: venv (Padrão do Python)**

**Criando o ambiente:**
```bash
# Windows
python -m venv adk_env

# macOS/Linux
python3.11 -m venv adk_env
```

**Ativando o ambiente:**
```bash
# Windows (CMD)
adk_env\Scripts\activate.bat

# Windows (PowerShell)
adk_env\Scripts\Activate.ps1

# macOS/Linux
source adk_env/bin/activate
```

**Quando ativado, você verá:**
```bash
(adk_env) C:\seu\projeto>
```

**Desativando:**
```bash
deactivate
```

#### **Método 2: Poetry**

```bash
# Inicializar projeto
poetry init

# Criar ambiente automaticamente
poetry install

# Ativar shell do Poetry
poetry shell
```

**⚠️ Problema Comum (Windows PowerShell):**
```
Erro: não é possível carregar o arquivo... porque a execução de scripts foi desabilitada
```

**Solução:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 2.3 Instalação de Dependências

#### **Estrutura do requirements.txt**

O arquivo `requirements.txt` lista todas as bibliotecas necessárias:

```txt
# Core Google ADK
google-adk>=0.1.0

# Modelo Gemini
google-generativeai>=0.3.0

# Frameworks de suporte
pydantic>=2.0.0
python-dotenv>=1.0.0

# Ferramentas adicionais
requests>=2.31.0
aiohttp>=3.9.0

# Para desenvolvimento (opcional)
pytest>=7.4.0
black>=23.0.0
pylint>=3.0.0
```

#### **Instalação passo a passo:**

**1. Navegue até o diretório do projeto:**
```bash
cd caminho/para/google_adk_crash_course
```

**2. Certifique-se de que o ambiente virtual está ativado**

**3. Instale as dependências:**

**Usando pip:**
```bash
pip install -r requirements.txt
```

**Usando Poetry:**
```bash
poetry add google-adk google-generativeai pydantic python-dotenv
```

**4. Verifique a instalação:**
```bash
pip list
# ou
poetry show
```

#### **Instalação por Tutorial Individual**

Cada tutorial pode ter dependências específicas:

```bash
# Exemplo: Tutorial 1
cd 1_starter_agent
pip install -r requirements.txt
```

**📊 Tempo estimado de instalação:** 2-5 minutos (dependendo da conexão)

### 2.4 Obtendo API Keys do Google AI Studio

#### **Passo 1: Acessar Google AI Studio**

1. Acesse: **https://aistudio.google.com/**
2. Faça login com sua conta Google
3. Aceite os termos de serviço (se solicitado)

#### **Passo 2: Criar API Key**

1. No menu lateral, clique em **"Get API Key"**
2. Clique em **"Create API Key"**
3. Selecione um projeto existente ou crie um novo:
   - **Criar novo projeto:** Clique em "Create project in new project"
   - **Usar existente:** Selecione da lista

4. Sua API Key será gerada automaticamente
5. **IMPORTANTE:** Copie imediatamente e armazene com segurança

**Exemplo de API Key:**
```
AIzaSyD1234567890abcdefghijklmnopqrstuv
```

#### **Passo 3: Configurar Limites e Quotas**

1. Acesse o **Google Cloud Console**: https://console.cloud.google.com/
2. Navegue até **APIs & Services → Credentials**
3. Encontre sua API Key e configure:
   - **Restrições de aplicativo:** Endereços IP (opcional)
   - **Restrições de API:** Limitar a Gemini API
   - **Quotas:** Verifique limites gratuitos

**📝 Limites do Tier Gratuito (sujeito a alterações):**
- 60 requisições por minuto
- 1,500 requisições por dia
- Acesso aos modelos Gemini Pro

**🔗 Links Úteis:**
- Documentação Gemini API: https://ai.google.dev/docs
- Pricing: https://ai.google.dev/pricing
- Quickstart: https://ai.google.dev/tutorials/python_quickstart

### 2.5 Configuração de Variáveis de Ambiente

#### **Por que usar Variáveis de Ambiente?**

✅ **Segurança:** API Keys não ficam no código  
✅ **Flexibilidade:** Fácil mudança entre ambientes  
✅ **Boas Práticas:** Separação de configuração e código

#### **Método 1: Arquivo .env (Recomendado)**

**1. Criar arquivo .env na raiz do projeto:**
```bash
# Windows
type nul > .env

# macOS/Linux
touch .env
```

**2. Adicionar configurações:**
```env
# API Keys
GOOGLE_API_KEY=AIzaSyD1234567890abcdefghijklmnopqrstuv

# Configurações do Modelo
MODEL_NAME=gemini-pro
TEMPERATURE=0.7
MAX_TOKENS=1024

# Configurações de Ambiente
ENVIRONMENT=development
LOG_LEVEL=INFO
```

**3. Adicionar .env ao .gitignore:**
```bash
echo ".env" >> .gitignore
```

**⚠️ CRÍTICO:** Nunca commite arquivos .env com API Keys!

**4. Carregar no código Python:**
```python
from dotenv import load_dotenv
import os

# Carregar variáveis
load_dotenv()

# Acessar variáveis
api_key = os.getenv("GOOGLE_API_KEY")
model_name = os.getenv("MODEL_NAME", "gemini-pro")  # valor padrão
```

#### **Método 2: Variáveis de Sistema**

**Windows (CMD):**
```cmd
setx GOOGLE_API_KEY "AIzaSyD1234567890abcdefghijklmnopqrstuv"
```

**Windows (PowerShell):**
```powershell
[System.Environment]::SetEnvironmentVariable('GOOGLE_API_KEY', 'AIzaSyD1234567890abcdefghijklmnopqrstuv', 'User')
```

**macOS/Linux (temporário):**
```bash
export GOOGLE_API_KEY="AIzaSyD1234567890abcdefghijklmnopqrstuv"
```

**macOS/Linux (permanente):**
```bash
# Adicionar ao ~/.bashrc ou ~/.zshrc
echo 'export GOOGLE_API_KEY="AIzaSyD1234567890abcdefghijklmnopqrstuv"' >> ~/.bashrc
source ~/.bashrc
```

#### **Verificação:**
```python
import os
print(os.getenv("GOOGLE_API_KEY"))  # Deve exibir sua chave
```

---

## 3. ESTRUTURA DO PROJETO

### 3.1 Organização de Diretórios

```
google_adk_crash_course/
│
├── 1_starter_agent/           # Tutorial básico
│   ├── starter_agent.py       # Código principal
│   ├── requirements.txt       # Dependências específicas
│   └── README.md             # Instruções
│
├── 2_model_agnostic_agent/    # Troca de modelos
│   ├── model_agnostic.py
│   ├── requirements.txt
│   └── README.md
│
├── 3_structured_output_agent/ # Outputs estruturados
│   ├── structured_output.py
│   ├── requirements.txt
│   └── README.md
│
├── 4_tool_using_agent/        # Integração com ferramentas
│   ├── tool_agent.py
│   ├── requirements.txt
│   └── README.md
│
├── 5_memory_agent/            # Gestão de memória
│   ├── memory_agent.py
│   ├── requirements.txt
│   └── README.md
│
├── 6_callbacks/               # Callbacks e monitoramento
│   ├── callbacks_agent.py
│   ├── requirements.txt
│   └── README.md
│
├── 7_streaming_agent/         # Respostas em streaming
│   ├── streaming_agent.py
│   ├── requirements.txt
│   └── README.md
│
├── 8_simple_multi_agent/      # Sistema multiagente básico
│   ├── multi_agent.py
│   ├── requirements.txt
│   └── README.md
│
├── 9_multi_agent_patterns/    # Padrões avançados
│   ├── sequential_pattern.py
│   ├── parallel_pattern.py
│   ├── loop_pattern.py
│   ├── requirements.txt
│   └── README.md
│
├── common/                    # Código compartilhado
│   ├── __init__.py
│   ├── config.py             # Configurações globais
│   ├── utils.py              # Funções utilitárias
│   └── models.py             # Modelos Pydantic
│
├── tests/                     # Testes automatizados
│   ├── test_agents.py
│   └── test_utils.py
│
├── docs/                      # Documentação
│   ├── architecture.md
│   └── best_practices.md
│
├── .env.example              # Template de variáveis
├── .gitignore                # Arquivos ignorados
├── requirements.txt          # Dependências globais
├── pyproject.toml           # Configuração Poetry
└── README.md                # Documentação principal
```

#### **Descrição das Pastas Principais:**

**📁 Tutoriais (1_* a 9_*):**
- Cada pasta é autocontida
- Progressão de complexidade
- README específico com instruções

**📁 common/:**
- Código reutilizável
- Configurações centralizadas
- Evita duplicação

**📁 tests/:**
- Testes unitários e de integração
- Validação automática
- Exemplos de uso

**📁 docs/:**
- Documentação técnica
- Guias e referências
- Diagramas de arquitetura

### 3.2 Guia de Navegação pelo Repositório

#### **Fluxo de Aprendizado Recomendado:**

**Fase 1: Fundamentos (Tutoriais 1-3)**
```
1_starter_agent → 2_model_agnostic_agent → 3_structured_output_agent
```

**Fase 2: Recursos Avançados (Tutoriais 4-7)**
```
4_tool_using_agent → 5_memory_agent → 6_callbacks → 7_streaming_agent
```

**Fase 3: Sistemas Multiagente (Tutoriais 8-9)**
```
8_simple_multi_agent → 9_multi_agent_patterns
```

#### **Arquivos Importantes:**

**📄 README.md (raiz):**
- Visão geral do projeto
- Quick start
- Links para documentação

**📄 requirements.txt (global):**
- Dependências comuns a todos tutoriais
- Versões estáveis testadas

**📄 .env.example:**
```env
# Copie para .env e preencha com suas chaves
GOOGLE_API_KEY=your_api_key_here
OPENAI_API_KEY=your_openai_key_here  # Opcional
ANTHROPIC_API_KEY=your_anthropic_key_here  # Opcional
```

### 3.3 Boas Práticas de Versionamento com Git

#### **Configuração Inicial do Git**

```bash
# Configurar identidade
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"

# Verificar configuração
git config --list
```

#### **Inicializando Repositório**

```bash
# Clonar repositório do curso
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/ai_agent_framework_crash_course/google_adk_crash_course

# Ou criar repositório próprio
git init
git remote add origin https://github.com/seu-usuario/seu-projeto.git
```

#### **Workflow Recomendado: Git Flow Simplificado**

**Branches:**
```
main (produção) → develop (desenvolvimento) → feature/* (funcionalidades)
```

**Criando uma feature:**
```bash
# Criar e mudar para branch
git checkout -b feature/meu-primeiro-agente

# Fazer alterações...

# Adicionar arquivos
git add .

# Commit com mensagem descritiva
git commit -m "feat: adiciona agente básico com Gemini"

# Enviar para remoto
git push origin feature/meu-primeiro-agente
```

#### **Convenções de Commit (Conventional Commits)**

**Formato:**
```
<tipo>(<escopo>): <descrição curta>

<corpo opcional>

<rodapé opcional>
```

**Tipos principais:**
- `feat:` Nova funcionalidade
- `fix:` Correção de bug
- `docs:` Documentação
- `style:` Formatação
- `refactor:` Refatoração
- `test:` Testes
- `chore:` Manutenção

**Exemplos:**
```bash
git commit -m "feat(agent): adiciona suporte a memória persistente"
git commit -m "fix(api): corrige timeout em chamadas ao Gemini"
git commit -m "docs(readme): atualiza instruções de instalação"
git commit -m "refactor(tools): simplifica integração com plugins"
```

#### **.gitignore Essencial**

```gitignore
# Ambientes virtuais
venv/
adk_env/
env/
.venv/

# Variáveis de ambiente
.env
.env.local

# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python

# IDEs
.vscode/
.idea/
*.swp
*.swo

# Logs
*.log
logs/

# Dados sensíveis
*.key
*.pem
secrets/

# OS
.DS_Store
Thumbs.db
```

#### **Comandos Úteis**

```bash
# Ver status
git status

# Ver histórico
git log --oneline --graph

# Desfazer último commit (mantém alterações)
git reset --soft HEAD~1

# Atualizar do remoto
git pull origin develop

# Criar tag de versão
git tag -a v1.0.0 -m "Versão 1.0.0"
git push origin v1.0.0
```

---

## 4. RESULTADOS ESPERADOS

### 4.1 Ambiente de Desenvolvimento Funcional

Ao concluir este módulo, você terá:

✅ **Python 3.11+ instalado e configurado**
- Verificável via `python --version`
- Accessível globalmente no terminal

✅ **Ambiente virtual criado e ativado**
- Isolamento completo de dependências
- Prompt do terminal mostrando `(adk_env)`

✅ **Todas as dependências instaladas**
- Google ADK e bibliotecas relacionadas
- Sem erros de importação

✅ **API Keys configuradas**
- Google AI Studio API Key funcional
- Variáveis de ambiente carregando corretamente

✅ **IDE configurada**
- Extensões/plugins instalados
- IntelliSense funcionando
- Debugging configurado

### 4.2 Capacidade de Executar Tutoriais Básicos

**Teste de Validação - Tutorial 1:**

```bash
# Navegar para o tutorial
cd 1_starter_agent

# Executar
python starter_agent.py
```

**Output esperado:**
```
Initializing Google ADK Agent...
Agent created successfully!
Sending query to Gemini Pro...

Response:
[Resposta contextual e coerente do modelo]

Execution completed successfully!
```

**Checklist de Validação:**
- [ ] Script executa sem erros
- [ ] Recebe resposta do modelo Gemini
- [ ] Tempo de resposta razoável (< 10 segundos)
- [ ] Logs aparecem corretamente

### 4.3 Familiaridade com Estrutura e Ferramentas

**Conhecimentos Adquiridos:**

**📂 Navegação:**
- Localizar rapidamente tutoriais
- Identificar arquivos de configuração
- Entender hierarquia de pastas

**🔧 Ferramentas:**
- Usar pip/poetry eficientemente
- Gerenciar ambientes virtuais
- Configurar variáveis de ambiente

**📝 Documentação:**
- Ler e interpretar READMEs
- Seguir guias de instalação
- Consultar documentação oficial

**🐛 Troubleshooting:**
- Identificar erros comuns
- Aplicar soluções conhecidas
- Buscar ajuda efetivamente

---

## 5. DICAS DE SOLUÇÃO DE PROBLEMAS

### 5.1 Problemas com Python

**❌ Erro: "Python não é reconhecido"**

**Sintomas:**
```bash
'python' não é reconhecido como um comando interno ou externo
```

**Soluções:**
1. Verifique se Python está no PATH
2. Reinstale marcando "Add to PATH"
3. Use `python3` em vez de `python` (macOS/Linux)
4. Adicione manualmente ao PATH do sistema

---

**❌ Erro: "Versão do Python incorreta"**

**Sintomas:**
```bash
ERROR: Python 3.11 or higher is required
```

**Soluções:**
1. Instale Python 3.11+
2. Use `python3.11` explicitamente
3. Configure alias: `alias python=python3.11`

---

### 5.2 Problemas com Ambiente Virtual

**❌ Erro: "venv não encontrado"**

**Sintomas:**
```bash
No module named 'venv'
```

**Solução (Linux):**
```bash
sudo apt install python3.11-venv
```

---

**❌ Erro: "Permissão negada ao ativar"**

**Solução (Windows PowerShell):**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

### 5.3 Problemas com Dependências

**❌ Erro: "pip install falha"**

**Sintomas:**
```bash
ERROR: Could not find a version that satisfies the requirement
```

**Soluções:**
1. Atualize pip: `pip install --upgrade pip`
2. Verifique conectividade: `ping pypi.org`
3. Use mirror alternativo: `pip install --index-url https://pypi.org/simple/`
4. Instale com `--no-cache-dir`

---

**❌ Erro: "Conflito de versões"**

**Sintomas:**
```bash
ERROR: pydantic 2.0.0 has requirement typing-extensions>=4.6.1, but you have typing-extensions 4.5.0
```

**Soluções:**
1. Desinstale e reinstale: `pip uninstall pydantic && pip install pydantic`
2. Use Poetry para resolver automaticamente
3. Force versão específica: `pip install pydantic==2.5.0`

---

### 5.4 Problemas com API Keys

**❌ Erro: "Invalid API Key"**

**Sintomas:**
```python
google.api_core.exceptions.PermissionDenied: 403 API key not valid
```

**Soluções:**
1. Verifique se copiou a chave completa
2. Confirme que .env está na raiz do projeto
3. Recarregue variáveis: `load_dotenv(override=True)`
4. Verifique restrições no Google Cloud Console

---

**❌ Erro: "Quota exceeded"**

**Sintomas:**
```python
google.api_core.exceptions.ResourceExhausted: 429 Quota exceeded
```

**Soluções:**
1. Aguarde reset do limite (geralmente 1 minuto)
2. Verifique quotas no Google Cloud Console
3. Implemente rate limiting no código
4. Considere upgrade de plano

---

### 5.5 Problemas com IDE

**❌ IntelliSense não funciona (VS Code)**

**Soluções:**
1. Selecione interpretador correto: `Ctrl+Shift+P` → "Python: Select Interpreter"
2. Recarregue janela: `Ctrl+Shift+P` → "Reload Window"
3. Instale Pylance
4. Verifique settings.json

---

**❌ Debugging não inicia (PyCharm)**

**Soluções:**
1. Configure Python Interpreter nas preferências
2. Crie configuração de run/debug
3. Verifique caminho do script
4. Desative firewall temporariamente

---

### 5.6 Problemas de Rede

**❌ Timeout em chamadas à API**

**Soluções:**
```python
import google.generativeai as genai

# Aumentar timeout
genai.configure(api_key=os.getenv("GOOGLE_API_KEY"), timeout=60)

# Adicionar retry logic
from tenacity import retry, wait_exponential, stop_after_attempt

@retry(wait=wait_exponential(min=1, max=10), stop=stop_after_attempt(3))
def call_gemini_with_retry():
    # Sua chamada aqui
    pass
```

---

### 5.7 Checklist de Troubleshooting

Antes de buscar ajuda, verifique:

- [ ] Python 3.11+ instalado corretamente
- [ ] Ambiente virtual ativado
- [ ] Todas as dependências instaladas
- [ ] API Key válida e configurada
- [ ] Arquivo .env no local correto
- [ ] Conexão com internet estável
- [ ] Firewall não bloqueando Python
- [ ] Versões compatíveis de bibliotecas
- [ ] Logs de erro completos disponíveis

---

## 6. EXERCÍCIOS PRÁTICOS

### Exercício 1: Configuração Completa
**Objetivo:** Validar ambiente funcional

**Tarefas:**
1. Instale Python 3.11
2. Crie ambiente virtual `adk_training`
3. Instale dependências do tutorial 1
4. Execute com sucesso `starter_agent.py`

**Entrega:** Screenshot do terminal com output

---

### Exercício 2: Múltiplas API Keys
**Objetivo:** Gerenciar múltiplos serviços

**Tarefas:**
1. Obtenha API Keys de Google, OpenAI e Anthropic
2. Configure no .env
3. Crie script que alterna entre modelos

**Código base:**
```python
import os
from dotenv import load_dotenv

load_dotenv()

models = {
    "gemini": os.getenv("GOOGLE_API_KEY"),
    "gpt": os.getenv("OPENAI_API_KEY"),
    "claude": os.getenv("ANTHROPIC_API_KEY")
}

for name, key in models.items():
    status = "✓ Configurado" if key else "✗ Faltando"
    print(f"{name}: {status}")
```

---

### Exercício 3: Estrutura de Projeto
**Objetivo:** Organizar projeto próprio

**Tarefas:**
1. Crie estrutura inspirada no crash course
2. Adicione .gitignore apropriado
3. Faça primeiro commit seguindo Conventional Commits

---

## 7. RECURSOS ADICIONAIS

### Documentação Oficial
- 📘 **Google ADK:** https://google.github.io/adk-docs/
- 📘 **Gemini API:** https://ai.google.dev/docs
- 📘 **Pydantic:** https://docs.pydantic.dev/
- 📘 **Python-dotenv:** https://pypi.org/project/python-dotenv/

### Tutoriais e Guias
- 🎓 **Python Virtual Environments:** https://realpython.com/python-virtual-environments-a-primer/
- 🎓 **Git Flow:** https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
- 🎓 **Conventional Commits:** https://www.conventionalcommits.org/

### Comunidade
- 💬 **Stack Overflow:** Tag [google-adk]
- 💬 **GitHub Discussions:** Repositório oficial
- 💬 **Discord:** Comunidade de desenvolvedores AI

---

## 8. CONCLUSÃO DO MÓDULO

Parabéns! Você completou o Módulo 2 e agora possui:

✅ Um ambiente de desenvolvimento profissional  
✅ Conhecimento das ferramentas essenciais  
✅ Capacidade de executar e modificar tutoriais  
✅ Base sólida para módulos avançados

**Próximos Passos:**
- **Módulo 3:** Criando o Primeiro Agente
- **Módulo 4:** Modelos, Ferramentas e Plugins

**Checkpoint de Validação:**
Execute este script final de validação:

```python
import sys
import os

print("=== Validação de Ambiente ===\n")

# Python version
print(f"✓ Python: {sys.version}")

# Environment
if os.getenv("GOOGLE_API_KEY"):
    print("✓ API Key configurada")
else:
    print("✗ API Key não encontrada")

# Imports
try:
    import google.generativeai as genai
    print("✓ Google Generative AI importado")
except ImportError:
    print("✗ Falha ao importar Google Generative AI")

try:
    from pydantic import BaseModel
    print("✓ Pydantic importado")
except ImportError:
    print("✗ Falha ao importar Pydantic")

print("\n=== Ambiente validado com sucesso! ===")
```

---