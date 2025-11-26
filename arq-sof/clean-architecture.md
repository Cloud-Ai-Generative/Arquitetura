*[← Voltar ao Guia Anterior](./arquitetura-software.md)*

📋 O que é?

Clean Architecture é um padrão arquitetural proposto por Robert C. Martin (Uncle Bob) que organiza o código de forma a separar responsabilidades e tornar o sistema independente de frameworks, interfaces de usuário, bancos de dados e qualquer detalhe externo. O objetivo principal é criar software durável, testável e fácil de manter.

🧅 Camadas da Clean Architecture (do centro para fora)
1. **Entities (Entidades)**  
   Objetos de negócio com regras críticas e independentes de tudo.  
   Ex: `User`, `Order`, `Invoice` com métodos como `isValid()`, `calculateTotal()`.

2. **Use Cases / Application Services (Casos de Uso)**  
   Regras de negócio da aplicação (orquestração). Contêm a lógica do que o sistema faz.  
   Ex: `CreateUserUseCase`, `ProcessPaymentUseCase`, `GenerateReportUseCase`.

3. **Interface Adapters (Adaptadores de Interface)**  
   Converte dados entre as camadas internas e externas.  
   - Controllers (API REST, GraphQL, gRPC)  
   - Presenters/ViewModels  
   - Gateways (repositórios implementados, clients externos)

4. **Frameworks & Drivers (Camada mais externa)**  
   Tudo que é detalhe:  
   - Web frameworks (Spring, Express, FastAPI, Gin)  
   - Banco de dados (PostgreSQL, MongoDB)  
   - UI (React, Flutter)  
   - Dispositivos externos, filas, etc.

🔑 Princípios fundamentais
- **Regra da Dependência**: As dependências sempre apontam para dentro. Camadas externas dependem das internas, nunca o contrário.
- **Inversão de Dependência**: Camadas internas definem interfaces; camadas externas as implementam.
- Nada nas camadas internas pode saber sobre as externas (não pode importar Express, Spring, Entity Framework, etc.).

✅ Vantagens
- Totalmente testável (use cases são testados sem banco, sem web server)
- Independente de framework e banco de dados
- UI pode mudar facilmente (de REST para GraphQL, de web para mobile)
- Regras de negócio ficam isoladas e reutilizáveis
- Facilita equipes grandes (times podem trabalhar em camadas diferentes)

🔥 Estrutura típica de pastas (exemplo em projetos reais)
```plaintext
src/
├── domain/          → Entities + interfaces de repositórios
├── application/     → Use cases + DTOs de entrada/saída
├── infrastructure/  → Implementações (repos com TypeORM, Prisma, etc.)
└── interfaces/      → Controllers, middlewares, presenters (REST, CLI, etc.)

⚙️ Tecnologias que combinam muito bem
- Linguagens: TypeScript, Java, Kotlin, C#, Go, Python, Dart
- Frameworks que respeitam Clean: NestJS, Spring Boot (com configuração certa), Micronaut, Quarkus, Go com chi ou fiberior, .NET Minimal APIs

Clean Architecture é hoje um dos padrões mais adotados em sistemas corporativos e startups que querem longevidade no código.  
Quanto mais complexo o projeto, mais ela brilha.
