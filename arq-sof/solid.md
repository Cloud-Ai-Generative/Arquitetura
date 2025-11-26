*[← Voltar ao Guia Anterior](./arquitetura-software.md)*

📋 O que é?
SOLID é um acrônimo criado por Robert C. Martin (Uncle Bob) que reúne **cinco princípios de design orientado a objetos** cuja aplicação torna o código mais compreensível, flexível, manutenível e preparado para mudanças.

🔠 Os 5 princípios

- **S** — **Single Responsibility Principle** (Princípio da Responsabilidade Única)  
  Uma classe deve ter apenas **um motivo para mudar** — ou seja, apenas uma responsabilidade.  
  Ex: uma classe `User` não deve validar e-mail **e** salvar no banco ao mesmo tempo.

- **O** — **Open/Closed Principle** (Princípio Aberto/Fechado)  
  Classes devem estar **abertas para extensão**, mas **fechadas para modificação**.  
  Use interfaces, herança e polimorfismo em vez de ficar alterando código existente.

- **L** — **Liskov Substitution Principle** (Princípio da Substituição de Liskov)  
  Subtipos devem ser substituíveis por suas superclasses sem quebrar o comportamento do programa.  
  Ex: se `Pato` herda de `Ave`, `Pato` precisa poder “voar” (ou a hierarquia está errada).

- **I** — **Interface Segregation Principle** (Princípio da Segregação de Interfaces)  
  Clientes não devem ser forçados a depender de interfaces que não usam.  
  Prefira várias interfaces pequenas e específicas do que uma interface “gorda”.

- **D** — **Dependency Inversion Principle** (Princípio da Inversão de Dependência)  
  Dependa de **abstrações** (interfaces), não de implementações concretas.  
  É a base da injeção de dependência e da regra de dependência da Clean/Hexagonal.

✅ Benefícios reais quando aplicados juntos
- Código fácil de testar (mock/stub simples)
- Baixo acoplamento e alta coesão
- Mudanças locais (uma funcionalidade nova raramente quebra outra)
- Facilita refatoração e crescimento do sistema
- Torna naturais padrões como Strategy, Factory, Decorator, etc.

⚠️ Cuidados comuns
- Aplicar SOLID em CRUDs simples pode gerar over-engineering
- Não é dogma: às vezes violar levemente um princípio evita complexidade desnecessária
- Em linguagens funcionais (F#, Clojure, Elixir) alguns princípios mudam de forma

🔥 SOLID é a base de quase tudo que veio depois
Clean Architecture, Hexagonal, DDD tático e injeção de dependência **só funcionam bem porque SOLID está por baixo**.

Em 2025, saber SOLID deixou de ser diferencial e virou pré-requisito para qualquer dev sênior.
