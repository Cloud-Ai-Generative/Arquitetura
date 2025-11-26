*[← Voltar ao Guia Anterior](./arquitetura-software.md)*

🔥 DDD vs Clean Architecture – Qual a diferença real?

| Aspecto                      | Domain-Driven Design (DDD)                          | Clean Architecture (Uncle Bob)                      |
|------------------------------|-----------------------------------------------------|------------------------------------------------------|
| Foco principal               | O **domínio do negócio** e sua complexidade        | **Separação de responsabilidades** e independência técnica |
| Origem / Autor               | Eric Evans (2003)                                   | Robert C. Martin (Uncle Bob) – 2012                  |
| Quando brilha mais           | Domínios complexos e ricos (logística, finanças, saúde, seguros) | Qualquer sistema que precisa ser testável e durável |
| Escopo                       | Abordagem completa: modelagem, linguagem, contexto, arquitetura tática e estratégica | Padrão arquitetural (camadas + regra da dependência) |
| Obriga Bounded Context?      | Sim, é conceito central                             | Não exige (mas combina muito bem)                    |
| Obriga Entities/VO/Aggregates? | Sim, são building blocks fundamentais              | Não exige (você pode usar anêmico ou outro modelo)   |
| Camadas                      | Geralmente 4 camadas muito parecidas com Clean, mas o nome e o foco mudam um pouco | 4 camadas fixas: Entities → Use Cases → Interface Adapters → Frameworks & Drivers |
| Regra de dependência         | Existe, mas é menos rígida que na Clean             | Regra de ouro: dependências só apontam para dentro   |
| Ubiquitous Language          | Conceito central e obrigatório                      | Não é mencionado (mas é boa prática usar)            |
| Domain Events                | Pilar importante do DDD tático e estratégico        | Opcional (muitos projetos Clean usam também)         |
| Pode viver sem o outro?      | Sim. Você pode fazer DDD com Hexagonal, Onion, etc. | Sim. Você pode fazer Clean com modelo anêmico ou CRUD |
| Combinação mais comum em 2025| DDD + Clean Architecture (quase padrão em sistemas corporativos grandes) | Clean Architecture com ou sem DDD                   |

🎯 Resumo prático (regra de bolso)

- Se o seu domínio é **simples/CRUD** → Use Clean Architecture (ou até algo mais leve). DDD seria overkill.
- Se o domínio é **complexo e as regras de negócio são o coração do sistema** → Use DDD + Clean Architecture juntos.
- Na prática hoje: a maioria dos projetos corporativos sérios usa **os dois ao mesmo tempo**:
  - Clean Architecture define as camadas e a regra da dependência.
  - DDD define como modelar o domínio dentro da camada mais interna (Entities, Aggregates, Domain Events, Ubiquitous Language).

Resultado: código extremamente testável, durável, compreensível pelo negócio e que envelhece muito bem.

💡 Frase que resume tudo
“Clean Architecture te diz ONDE colocar as coisas.  
DDD te diz COMO modelar as coisas que vão dentro da camada mais importante.”
