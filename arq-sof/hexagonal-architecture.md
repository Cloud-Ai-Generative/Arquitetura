📋 O que é?
Hexagonal Architecture (também chamada de Ports and Adapters) é um padrão arquitetural criado por Alistair Cockburn em 2005. A ideia principal é isolar a lógica de negócio (o núcleo da aplicação) de tudo que é externo: UI, banco de dados, APIs, filas, frameworks, etc.  

O aplicativo fica no centro (hexágono) e se comunica com o mundo externo apenas através de **portas** (interfaces) que são implementadas por **adaptadores**.

🎯 Objetivo principal
“Permitir que uma aplicação seja conduzida (driven) por usuários, programas, testes automatizados ou scripts, e que seja desenvolvida e testada isoladamente de seus dispositivos e bancos de dados finais.”

### 🧩 Estrutura visual
```plaintext
← Driving Adapters (Adapters de entrada)
Usuarios, Tests, CLI, API REST, GraphQL, gRPC, Mensageria etc.
↓↓↓↓↓
[ PORTS de entrada ]
↓↓↓↓↓
┌──────────────────────┐
│     APPLICATION       │ ← Núcleo (Use Cases, Regras de Negócio)
│      (Core)          │
└──────────────────────┘
↑↑↑↑↑
[ PORTS de saída ]
↑↑↑↑↑
→ Driven Adapters (Adapters de saída)
Banco de dados, APIs externas, filas, e-mail, cache, etc.

🔌 Portas (Ports)
- **Portas de Entrada** (Driving Ports)  
  Interfaces que o núcleo define e que os adaptadores de entrada implementam/consomem.  
  Ex: `UserRegistrationUseCase`, `OrderPlacementPort`

- **Portas de Saída** (Driven Ports)  
  Interfaces que o núcleo define e que os adaptadores de saída implementam.  
  Ex: `UserRepositoryPort`, `PaymentGatewayPort`, `NotificationPort`

🔧 Adaptadores (Adapters)
- **Primários / Driving** → REST controllers, CLI, testes, consumidores de mensagens
- **Secundários / Driven** → Repositórios JPA, clientes HTTP, produtores Kafka, etc.

✅ Vantagens
- Lógica de negócio 100% testável sem banco, sem servidor web, sem dependências externas
- Troca fácil de tecnologias (de PostgreSQL → MongoDB, de REST → GraphQL)
- Alta coesão e baixo acoplamento
- Ideal para evoluir o sistema por décadas

🔥 Estrutura típica de pastas (2025)

```plaintext
src/
├── application/         → Use cases + ports de entrada
├── domain/              → Entities, Value Objects, Domain Services, Domain Events
├── ports/               → Interfaces (entrada e saída) – às vezes separada
└── adapters/
    ├── inbound/         → Controllers REST, GraphQL, gRPC, CLI, mensageria
    └── outbound/        → Repositórios, clients HTTP, mensageria, etc.
⚡ Frameworks que amam Hexagonal

Java/Kotlin → Spring Boot, Quarkus, Micronaut
.NET → Minimal APIs + Carter/MediatR
TypeScript → NestJS (modo “standard” ou “monolith”), Ts.ED
Go → padrão da comunidade (chi, gin, fiber)
Python → FastAPI + clean architecture packages
