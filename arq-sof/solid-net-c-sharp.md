*[← Voltar ao Guia Anterior](./solid.md)*

🔠 Os 5 princípios (com exemplos reais em C# / .NET)

### S — Single Responsibility Principle (Responsabilidade Única)
Uma classe deve ter apenas **um motivo para mudar**.

**Errado (duas responsabilidades)**
```csharp
public class UserService
{
    public void Register(string email, string password) { ... }
    public void SendWelcomeEmail(string email) { ... }   // Não deveria estar aqui!
}
**Correto**
public class UserService
{
    public void Register(string email, string password) { ... }
}

public class EmailService
{
    public void SendWelcomeEmail(string email) { ... }
}
O — Open/Closed Principle (Aberto/Fechado)
Aberto para extensão, fechado para modificação.
Errado (modifica a classe toda vez)
public class DiscountCalculator
{
    public decimal Calculate(CustomerType type)
    {
        if (type == CustomerType.Regular) return 0.1m;
        if (type == CustomerType.VIP) return 0.2m;
        if (type == CustomerType.Employee) return 0.3m;  // Nova regra = nova modificação
        return 0;
    }
}

Correto (extensão sem modificar)
public interface IDiscountStrategy
{
    decimal ApplyDiscount(decimal price);
}

public class VipDiscount : IDiscountStrategy { ... }
public class EmployeeDiscount : IDiscountStrategy { ... }

public class DiscountCalculator
{
    private readonly IDiscountStrategy _strategy;
    public DiscountCalculator(IDiscountStrategy strategy) => _strategy = strategy;
    public decimal Calculate(decimal price) => _strategy.ApplyDiscount(price);
}

L — Liskov Substitution Principle (Substituição de Liskov)
Subclasses devem ser substituíveis pelas suas classes base sem quebrar o programa.
Errado (viola LSP)

public class Bird { public virtual void Fly() { ... } }
public class Penguin : Bird 
{ 
    public override void Fly() => throw new NotSupportedException("Pinguins não voam!");
}

Correto

public interface IFlyable { void Fly(); }
public class Sparrow : IFlyable { ... }
public class Penguin { }  // Não implementa IFlyable → hierarquia correta

I — Interface Segregation Principle (Segregação de Interfaces)
Nenhuma classe deve ser forçada a implementar métodos que não usa.
Errado (interface gorda)
public interface IWorker
{
    void Work();
    void Eat();
    void Sleep();
}

public class Robot : IWorker
{
    public void Work() { ... }
    public void Eat() => throw new NotSupportedException();   // Robô não come!
    public void Sleep() => throw new NotSupportedException();
}

Correto
public interface IWorkable { void Work(); }
public interface IMaintainable { void Eat(); void Sleep(); }

public class Human : IWorkable, IMaintainable { ... }
public class Robot : IWorkable { ... }   // Só implementa o que precisa

D — Dependency Inversion Principle (Inversão de Dependência)
Dependa de abstrações, não de implementações concretas.
Errado (acoplamento alto)

public class OrderService
{
    private readonly SqlServerDatabase _db = new SqlServerDatabase();  // Acoplado!

    public void Save(Order order) => _db.Save(order);
}

Correto (inversão via injeção)

public interface IOrderRepository
{
    void Save(Order order);
}

public class OrderService
{
    private readonly IOrderRepository _repository;

    public OrderService(IOrderRepository repository) => _repository = repository;

    public void Save(Order order) => _repository.Save(order);
}

// Agora você pode injetar SqlServer, MongoDB, InMemory, etc.

Aqui está o seu `solid.md` **completo e 100% formatado**, com **exemplos reais em C#/.NET** para cada princípio do SOLID — tudo no mesmo estilo limpo dos outros arquivos do seu repositório:

```markdown
📋 O que é?
SOLID é um acrônimo criado por Robert C. Martin (Uncle Bob) que reúne **cinco princípios de design orientado a objetos**. Quando aplicados juntos, tornam o código mais compreensível, flexível, manutenível e preparado para mudanças.

🔠 Os 5 princípios (com exemplos reais em C# / .NET)

### S — Single Responsibility Principle (Responsabilidade Única)
Uma classe deve ter apenas **um motivo para mudar**.

**Errado (duas responsabilidades)**
```csharp
public class UserService
{
    public void Register(string email, string password) { ... }
    public void SendWelcomeEmail(string email) { ... }   // Não deveria estar aqui!
}
```

**Correto**
```csharp
public class UserService
{
    public void Register(string email, string password) { ... }
}

public class EmailService
{
    public void SendWelcomeEmail(string email) { ... }
}
```

### O — Open/Closed Principle (Aberto/Fechado)
Aberto para **extensão**, fechado para **modificação**.

**Errado (modifica a classe toda vez)**
```csharp
public class DiscountCalculator
{
    public decimal Calculate(CustomerType type)
    {
        if (type == CustomerType.Regular) return 0.1m;
        if (type == CustomerType.VIP) return 0.2m;
        if (type == CustomerType.Employee) return 0.3m;  // Nova regra = nova modificação
        return 0;
    }
}
```

**Correto (extensão sem modificar)**
```csharp
public interface IDiscountStrategy
{
    decimal ApplyDiscount(decimal price);
}

public class VipDiscount : IDiscountStrategy { ... }
public class EmployeeDiscount : IDiscountStrategy { ... }

public class DiscountCalculator
{
    private readonly IDiscountStrategy _strategy;
    public DiscountCalculator(IDiscountStrategy strategy) => _strategy = strategy;
    public decimal Calculate(decimal price) => _strategy.ApplyDiscount(price);
}
```

### L — Liskov Substitution Principle (Substituição de Liskov)
Subclasses devem ser substituíveis pelas suas classes base sem quebrar o programa.

**Errado (viola LSP)**
```csharp
public class Bird { public virtual void Fly() { ... } }
public class Penguin : Bird 
{ 
    public override void Fly() => throw new NotSupportedException("Pinguins não voam!");
}
```

**Correto**
```csharp
public interface IFlyable { void Fly(); }
public class Sparrow : IFlyable { ... }
public class Penguin { }  // Não implementa IFlyable → hierarquia correta
```

### I — Interface Segregation Principle (Segregação de Interfaces)
Nenhuma classe deve ser forçada a implementar métodos que não usa.

**Errado (interface gorda)**
```csharp
public interface IWorker
{
    void Work();
    void Eat();
    void Sleep();
}

public class Robot : IWorker
{
    public void Work() { ... }
    public void Eat() => throw new NotSupportedException();   // Robô não come!
    public void Sleep() => throw new NotSupportedException();
}
```

**Correto**
```csharp
public interface IWorkable { void Work(); }
public interface IMaintainable { void Eat(); void Sleep(); }

public class Human : IWorkable, IMaintainable { ... }
public class Robot : IWorkable { ... }   // Só implementa o que precisa
```

### D — Dependency Inversion Principle (Inversão de Dependência)
Dependa de **abstrações**, não de implementações concretas.

**Errado (acoplamento alto)**
```csharp
public class OrderService
{
    private readonly SqlServerDatabase _db = new SqlServerDatabase();  // Acoplado!

    public void Save(Order order) => _db.Save(order);
}
```

**Correto (inversão via injeção)**
```csharp
public interface IOrderRepository
{
    void Save(Order order);
}

public class OrderService
{
    private readonly IOrderRepository _repository;

    public OrderService(IOrderRepository repository) => _repository = repository;

    public void Save(Order order) => _repository.Save(order);
}

// Agora você pode injetar SqlServer, MongoDB, InMemory, etc.
```

✅ Benefícios reais com SOLID + .NET
- Testes unitários triviais (xUnit + Moq viram brincadeira)
- Facilita uso do ASP.NET Core DI container
- Projetos crescem por anos sem virar “big ball of mud”
- Refatoração segura e constante

⚠️ Dica de senior
Em projetos .NET reais em 2025, SOLID é aplicado automaticamente quando você usa:
- MediatR (CQRS)
- Minimal APIs + Application Services
- Entity Framework Core com Repository Pattern
- Clean Architecture / Vertical Slice

SOLID não é opcional — é a **base de tudo** que veio depois (Clean, Hexagonal, DDD, etc.).

Em 2025, quem não domina SOLID ainda está no nível júnior/médio.  
Domine esses 5 princípios e você vai escrever código que envelhece como vinho.
```

Pode colar direto no seu `solid.md` — vai ficar perfeito ao lado dos outros arquivos!  
Se quiser também a versão com exemplos em **TypeScript/NestJS** ou **Java/Spring**, é só pedir!
