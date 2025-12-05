*[← Voltar ao Guia Anterior](./computacao-nuvem.md*

Desenho de solução:

![Arquitetura Multi-Cloud(images/arq-multi-cloud.jpg)

# ☁️📊 Explicação sobre o Desenho de Solução Multi-cloud

## 🌟 Visão Geral
Estamos considerando uma arquitetura **Multi-cloud** que contempla **AWS e Azure** e vamos falar de conceitos de serviços na nuvem, não entrando muito no detalhe do nome de cada serviço, pois cada nuvem tem sua particularidade e serviço exclusivo. O desenho mostra o serviço utilizado no detalhe.

## 📚 Base Teórica - 12 Factor App
Das explicações abaixo listamos algumas funcionalidades baseadas em **[12 Factor App (Aplicação 12 fatores)](https://12factor.net/pt_br/)**. Atente-se a numeração em verde com fonte branca indicando 10 pontos para explicação:

---

### 🟢 1️⃣ **Jornada do Usuário**
📱💻 Aqui temos a jornada inicial do usuário que pode optar por uma experiência em **mobile ou desktop**.

### 🟢 2️⃣ **Resolução de DNS**
🔗🌐 Usuário vai chamar uma URL, por exemplo `meuportal.com.br` e aqui temos um **resolvedor de DNS** que vai chamar um serviço de **Content Delivery Network (CDN)**.

### 🟢 3️⃣ **Edge & Segurança (CDN)**
🛡️⚡ O serviço de **Content Delivery Network (CDN)** que distribui e faz um **cache do tráfego dos micro-front-ends (MFEs)**, aqui utilizaremos:
- 🔒 Criptografia com certificados
- 🛡️ Proteção contra ataques DDoS
- 🔥 Firewall

### 🟢 4️⃣ **Camada de Frontend (Micro-front-ends)**
🎨🔄 Temos três MFEs hospedados em **buckets estáticos** com stack **Angular** (sugestão que poderia ser em **React**):

#### 📍 **MFE do Portal**
- Tela inicial com opções para interação
- Tela de processamento em Lote
- 🔐 Autenticação por e-mail e senha (CIAM)
- 👥 Perfis: Usuário padrão ou Administrador

#### 📊 **MFE Principal**
- Tela de processamento em Lote
- Filtros por seleção manual e outros filtros

#### 📝 **MFE Itens**
- Tela de edição de lotes (individuais ou em lote)

✅ **12 Factor - II. Dependencies**: Cobrimos o princípio de dependências explicitamente declaradas.

#### 4.1 **🔄 Browser Runtime**
💡 Sugestão para Browser Runtime baseado em **Angular com Módulo Federation**:
- 🐚 **Shell App** (portal) que injeta outros MFEs
- 🔗 Injeção dinâmica de MFE principal e MFE itens

### 🟢 5️⃣ **Engenharia de Plataforma e DevOps**
⚙️🚀 Aqui cobrimos 3 pontos do 12 Factor:

✅ **I. Code-Base**: Base de código versionada  
✅ **X. Dev Prod Parity**: Ambientes próximos de produção  
✅ **V. Build Release Run**: Separação de estágios  

#### 🛠️ **Ferramentas Sugeridas:**
- 🔄 CI/CD: **GitHub** ou **Azure DevOps**
- 🏗️ Provisionamento: **Terraform** (IaC)
- 🔧 Testes: CLI de cada nuvem

### 🟢 6️⃣ **Camada de Agregação (BFF - Backend for Frontend)**
🔄🎯 O ideal é que cada MFE tem seu **Backend for Frontend (BFF)** que:
- Faz chamadas às APIs de backend
- Monta retornos para renderização no MFE
- Protegido por **Gateway externo de API**

#### 🏗️ **3 BFFs:**
1. **BFF do Portal** - Valida autenticação, tokens, permissões
2. **BFF do MFE Principal**
3. **BFF do MFE Itens**

#### ⚡ **Tecnologia:**
- 🚀 Funções sem servidor (Serverless)
- 🐍 Stack Python (runtime rápido)
- ✅ **12 Factor - II. Dependencies**: Dependências declaradas

### 🟢 7️⃣ **Microserviços de Backend**
🏗️⚙️ Serviços de backend protegidos por **Gateway interno de API**:

#### 🔄 **Orquestração:**
- ⚖️ Application Load Balancer (ALB) - camada 7
- 🐳 Cluster Kubernetes (3 serviços)
- 🔄 Containers para portabilidade entre nuvens

#### 💻 **Stack:**
- 🟣 Stack .NET (C#) para backends
- ✅ **12 Factor - IX. Disposability** e **VII. Port binding**

#### 🏗️ **Serviços:**
1. **👤 User Service** - Informações do usuário logado
2. **📊 Principal Service** - Dados principais de lotes
3. **📝 Itens Service** - Dados de lotes + arquitetura orientada a eventos

#### 🔄 **Processamento Assíncrono:**
- 📤 Fila: `evento-atualizar-item`
- 🐍 Função sem servidor (Python) - lê da fila
- 🔄 5 tentativas de processamento
- 📥 DLQ (Dead Letter Queue) para falhas

#### 🗄️ **Armazenamento:**
- 💾 Banco de dados relacional (ou NoSQL)
- ✅ **12 Factor - IV. Backing services**
- 💡 Sugestão: Serviço de cache para leitura

### 🟢 8️⃣ **Observabilidade**
👁️📊 Monitoramento e observabilidade:
- 📈 Métricas e logs
- 🕵️ Tracing
- 👤 Real User Monitoring
- 🔧 Alternativas: Grafana + Prometheus ou Datadog

✅ **12 Factor - XI. Logs**: Logs como fluxo de eventos

### 🟢 9️⃣ **Segurança**
🔐🛡️ Camada de segurança:

#### 🛡️ **Componentes:**
- 🔥 Firewall
- 👥 **CIAM** - Provedor de autenticação para usuários externos
- 👮 **IAM/ACL** - Controle de acesso (grupos e permissões)
- 🔑 **Secrets Manager** - Gerenciamento de segredos
  - Variáveis de ambiente
  - Connection strings
  - Credenciais

### 🟢 🔟 **FinOps**
💰📊 Estimativa de custos:
- 🧮 Baseada nas calculadoras oficiais das nuvens
- 📈 Cálculo básico para planejamento futuro
- 🔄 Possibilidade de detalhamento posterior

---

## 👨💻 **Autor**
**Wellington Dimas Cruz**  
🔗 LinkedIn: [https://www.linkedin.com/in/wellington-cruz-arquiteto-solucoes/](https://www.linkedin.com/in/wellington-cruz-arquiteto-solucoes/)  
🐙 GitHub: [https://github.com/Cloud-Ai-Generative](https://github.com/Cloud-Ai-Generative)

---

## 📋 **Resumo Técnico**
☁️ **Explicação Técnica do Desenho de Solução Multi-cloud**  
Baseado nos princípios do **[12 Factor App](https://12factor.net/)**

### 🎯 **Principais Características:**
1. 🌐 **Multi-cloud** (AWS + Azure)
2. 🏗️ **Arquitetura moderna** com Micro-frontends
3. 🔄 **Serverless** onde aplicável
4. 🐳 **Containers** e Kubernetes
5. 🔐 **Segurança em múltiplas camadas**
6. 👁️ **Observabilidade completa**
7. 💰 **Gestão de custos (FinOps)**

### ✅ **Conformidade com 12 Factor App:**
1. 📁 Codebase
2. 📦 Dependencies
3. ⚙️ Config
4. 🗄️ Backing services
5. 🔄 Build, release, run
6. 🚀 Processes
7. 🔗 Port binding
8. 📈 Concurrency
9. ⏱️ Disposability
10. 🔄 Dev/prod parity
11. 📝 Logs
12. 👨💼 Admin processes