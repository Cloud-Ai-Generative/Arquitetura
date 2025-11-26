*[← Voltar ao Guia Anterior](./arquitetura-software.md)*

🔥 # DDD vs Clean Architecture vs Hexagonal — Qual a diferença real?

| Critério                          | Domain-Driven Design (DDD)                         | Clean Architecture (Uncle Bob)                     | Hexagonal Architecture (Ports & Adapters)          |
|-----------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|
| Autor / Ano                       | Eric Evans – 2003                                  | Robert C. Martin – 2012                            | Alistair Cockburn – 2005                           |
| Tipo                              | Abordagem completa de modelagem + arquitetura     | Padrão arquitetural (camadas concêntricas)         | Padrão arquitetural (portas e adaptadores)         |
| Foco principal                    | **Domínio complexo** e Ubiquitous Language        | **Separação de responsabilidades** e independência| **Isolamento do núcleo** via portas e adaptadores |
| Obriga modelagem rica (Entities, VO, Aggregates)? | Sim, é o coração do DDD                          | Não (pode usar modelo anêmico se quiser)           | Não (mas quase todo mundo usa com DDD)             |
| Obriga Bounded Context?           | Sim, conceito central                              | Não exige                                          | Não exige                                          |
| Obriga Domain Events?             | Muito recomendado (estratégico e tático)           | Opcional                                           | Opcional                                           |
| Regra de dependência              | Existe, mas menos rígida                           | Regra de ouro: só para dentro                      | Regra forte: núcleo não conhece o mundo externo    |
| Camadas / Organização             | Geralmente 4 camadas (muito parecida com Clean)    | 4 círculos fixos (Entities → Use Cases → Adapters → Frameworks) | Núcleo + Ports (entrada/saída) + Adapters          |
| Nome das portas/interfaces        | Repositories, Services, etc.                       | Interface Adapters (gateways, presenters)          | Driving Ports (entrada) e Driven Ports (saída)     |
| Como os adaptadores são chamados  | Não padronizado (varia)                            | Interface Adapters                                 | Driving Adapters (primários) e Driven Adapters (secundários) |
| Estrutura de pastas típica 2025   | `domain/`, `application/`, `infrastructure/`       | `entities/`, `use-cases/`, `interface-adapters/`, `frameworks-drivers/` | `application/`, `domain/`, `ports/`, `adapters/inbound/` e `outbound/` |
| É possível usar sem os outros?    | Sim                                                | Sim                                                | Sim                                                |
| Combinação mais comum hoje        | DDD + Clean OU DDD + Hexagonal                     | Clean (com ou sem DDD)                             | Hexagonal (quase sempre com DDD)                   |
| Frase que resume                  | “O software deve falar a linguagem do negócio”    | “Dependências apontam para dentro”                 | “O núcleo não sabe quem está do lado de fora”      |

### Resumo prático (regra de bolso 2025)

| Situação                                      | Melhor escolha                                   |
|-----------------------------------------------|--------------------------------------------------|
| CRUD simples ou MVP                           | Clean ou Hexagonal puro (DDD seria overkill)     |
| Domínio simples, mas quero código limpo       | Clean Architecture                               |
| Domínio complexo (regras ricas, longa vida)   | DDD + Clean **OU** DDD + Hexagonal               |
| Equipe Java/Spring ou .NET grande             | DDD + Clean (padrão corporativo)                 |
| Equipe TypeScript/NestJS ou Kotlin            | DDD + Hexagonal (NestJS incentiva isso)          |
| Preciso trocar banco/UI/tecnologia com frequência | Hexagonal (mais explícita nas portas)           |

### Conclusão que 99% dos arquitetos sérios usam hoje
Na prática em 2025, **as três coisas andam juntas**:

1. **Hexagonal ou Clean** → definem a estrutura de camadas/pastas e a regra de dependência  
2. **DDD** → define COMO modelar a camada mais interna (Entities, Aggregates, Domain Events, Ubiquitous Language)

Resultado: sistemas corporativos extremamente testáveis, manuteníveis por décadas e que o time de negócio realmente entende.

Escolha o nome que sua equipe preferir — o importante é aplicar os princípios certos.
