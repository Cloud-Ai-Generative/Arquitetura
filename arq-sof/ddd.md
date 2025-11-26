*[← Voltar ao Guia Anterior](./arquitetura-software.md)*

📋 O que é?
Domain-Driven Design (DDD) é uma abordagem de desenvolvimento de software criada por Eric Evans que coloca o domínio do negócio (o “problema real” que o sistema resolve) no centro de todo o projeto. O código deve refletir a linguagem, as regras e os processos do negócio — falados pelos especialistas do domínio (médicos, contadores, operadores logísticos, etc.).

🎯 Quando usar DDD?
- Domínio complexo e rico em regras de negócio
- Sistema vai viver e evoluir por muitos anos
- Equipe tem acesso real aos especialistas do domínio
- Não vale a pena em CRUDs simples ou projetos pequenos

🔑 Conceitos fundamentais (Building Blocks)

- **Ubiquitous Language**  
  Uma linguagem única e compartilhada entre devs e especialistas.  
  Ex: “Pedido”, “Fatura”, “Reserva”, “Sinistro” — mesmos nomes no código e nas reuniões.

- **Bounded Context**  
  Limite explícito onde um modelo/conceito tem significado específico.  
  Ex: “Cliente” no contexto de Vendas ≠ “Cliente” no contexto de Cobrança.

- **Aggregate**  
  Grupo de objetos tratados como uma unidade para consistência de dados.  
  Tem uma Aggregate Root (ex: `Pedido`) e só se acessa os filhos através dela.

- **Entity**  
  Objeto com identidade contínua (ex: `Cliente`, `Produto`).

- **Value Object**  
  Objeto sem identidade, definido só pelos seus atributos (ex: `Endereco`, `Dinheiro`).

- **Domain Events**  
  Fatos importantes que aconteceram no domínio.  
  Ex: `PedidoCriado`, `PagamentoConfirmado`, `EstoqueBaixado`.

- **Repository**  
  Coleção de aggregates (interface no domínio, implementação na infraestrutura).

- **Domain Services**  
  Operações que não cabem naturalmente em uma Entity ou Value Object.

- **Factory**  
  Responsável por criar aggregates complexos com consistência garantida.

- **Context Mapping**  
  Como os diferentes Bounded Contexts se comunicam (ACL, Published Language, Open-Host Service, etc.).

🌍 Camadas típicas em projetos DDD (muito próximo da Clean Architecture)
- **Domain Layer** → Entities, Value Objects, Domain Services, Events, Interfaces
- **Application Layer** → Application Services, DTOs, orquestração de use cases
- **Infrastructure Layer** → Repositórios concretos, adapters, persistência
- **Presentation/UI** → Controllers, GraphQL, CLI, etc.

✅ Vantagens reais
- Código que os especialistas do negócio conseguem entender
- Mudanças no domínio refletem rapidamente no código
- Alta coesão e baixo acoplamento entre contextos
- Facilita evolução longa do sistema
- Testes de domínio puros e focados nas regras de negócio

⚠️ Desafios e cuidados
- Curva de aprendizado alta
- Over-engineering em domínios simples
- Precisa de especialistas do domínio disponíveis
- Mais código inicial (mas paga a dívida técnica depois)

🔥 Ecossistema atual (2025)
- Linguagens mais usadas: C# (.NET), Java/Kotlin (Spring), TypeScript/Node (NestJS), Go
- Bibliotecas populares: Axon (Java), MediatR + FluentValidation (.NET), Wolkenkit, EventStore + Prooph

DDD não é uma receita pronta — é uma filosofia.  
Quando bem aplicada em domínios complexos, é uma das abordagens mais poderosas que existem.
