*[← Voltar ao Guia Anterior](./solid.md)*

Domínio do exemplo: **Sistema de Pedidos de uma Loja Online**

### S — Single Responsibility Principle (Responsabilidade Única)

**Problema real (anti-pattern comum)**
```csharp
public class OrderService
{
    public void CreateOrder(Order order) { /* lógica */ }
    public void CalculateTotal(Order order) { /* lógica */ }
    public void SaveToDatabase(Order order) { /* EF Core */ }
    public void SendConfirmationEmail(Order order) { /* SMTP */ }
    public void GenerateInvoicePdf(Order order) { /* PDF library */ }
    // 5 motivos para essa classe mudar → SRP violado!
}
```

**Solução correta (cada classe tem 1 responsabilidade)**
```csharp
public class OrderService                → Orquestra o fluxo
public class OrderCalculator             → Calcula totais, descontos, frete
public class OrderRepository             → Persistência (EF Core)
public class EmailNotificationService    → Envia e-mails
public class InvoiceGenerator            → Gera PDF/NFe
```

### O — Open/Closed Principle (Aberto/Fechado)

**Sem OCP — você modifica essa classe toda vez que surge um novo tipo de desconto**
```csharp
public class DiscountService
{
    public decimal ApplyDiscount(Order order)
    {
        switch (order.Customer.Type)
        {
            case CustomerType.Regular:     return order.Total * 0.05m;
            case CustomerType.VIP:         return order.Total * 0.15m;
            case CustomerType.Employee:    return order.Total * 0.25m;
            case CustomerType.BlackFriday: return order.Total * 0.40m; // Nova regra = nova modificação!
            default: return 0;
        }
    }
}
```

**Com OCP — extensão sem modificação**
```csharp
public interface IDiscountRule
{
    bool IsApplicable(Order order);
    decimal CalculateDiscount(Order order);
}

public class VipDiscountRule : IDiscountRule { ... }
public class EmployeeDiscountRule : IDiscountRule { ... }
public class BlackFridayDiscountRule : IDiscountRule { ... }  // Nova regra = nova classe!

public class DiscountService
{
    private readonly IEnumerable<IDiscountRule> _rules;
    public DiscountService(IEnumerable<IDiscountRule> rules) => _rules = rules;

    public decimal ApplyDiscount(Order order)
        => _rules.Where(r => r.IsApplicable(order))
                 .MaxBy(r => r.CalculateDiscount(order))?.CalculateDiscount(order) ?? 0;
}
```

### L — Liskov Substitution Principle (Substituição de Liskov)

**Violação clássica do quadrado/retângulo (e do pinguim)**
```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
    public int Area => Width * Height;
}

public class Square : Rectangle
{
    public override int Width { set => base.Width = base.Height = value; }
    public override int Height { set => base.Width = base.Height = value; }
}

// Quebra testes que esperam comportamento independente!
```

**Correto — hierarquia coerente**
```csharp
public abstract class Shape
{
    public abstract decimal CalculateArea();
}

public class Rectangle : Shape { ... }
public class Circle : Shape { ... }
public class Triangle : Shape { ... }

// Ninguém espera que Circle tenha Width/Height → LSP preservado
```

### I — Interface Segregation Principle (Segregação de Interfaces)

**Interface gorda que força implementação inútil**
```csharp
public interface IOrderProcessor
{
    void ValidateOrder();
    void ProcessPayment();
    void ReserveInventory();
    void SendConfirmationEmail();
    void GenerateFiscalDocument();   // Só lojas no Brasil precisam!
}

public class InternationalOrderProcessor : IOrderProcessor
{
    public void GenerateFiscalDocument() => throw new NotSupportedException();
}
```

**Interfaces específicas — ninguém implementa o que não usa**
```csharp
public interface IOrderValidator { void Validate(Order order); }
public interface IPaymentProcessor { Task ProcessPayment(Order order); }
public interface IInventoryManager { void ReserveItems(Order order); }
public interface INotificationService { void SendConfirmation(Order order); }
public interface IFiscalDocumentGenerator { void Generate(Order order); } // Opcional!

// Classe brasileira
public class BrazilianOrderProcessor : IOrderValidator, IPaymentProcessor, 
                                        IInventoryManager, INotificationService, 
                                        IFiscalDocumentGenerator { ... }

// Classe EUA
public class USOrderProcessor : IOrderValidator, IPaymentProcessor, 
                                 IInventoryManager, INotificationService { ... }
```

### D — Dependency Inversion Principle (Inversão de Dependência)

**Acoplamento alto — impossível testar ou trocar banco**
```csharp
public class OrderService
{
    private readonly SqlServerConnection _db = new SqlServerConnection();

    public void Save(Order order)
    {
        // Direto no concreto → acoplado para sempre
        _db.Execute("INSERT INTO Orders ...");
    }
}
```

**Inversão correta — depende só de abstração**
```csharp
public interface IOrderRepository
{
    Task SaveAsync(Order order);
    Task<Order?> GetByIdAsync(Guid id);
}

public class OrderService
{
    private readonly IOrderRepository _repository;
    private readonly INotificationService _notification;

    public OrderService(IOrderRepository repository, INotificationService notification)
    {
        _repository = repository;
        _notification = notification;
    }

    public async Task ProcessOrderAsync(Order order)
    {
        // Lógica de negócio pura
        await _repository.SaveAsync(order);
        await _notification.SendConfirmation(order);
    }
}

// No Program.cs (ASP.NET Core)
builder.Services.AddScoped<IOrderRepository, SqlServerOrderRepository>();
// Ou: .AddScoped<IOrderRepository, PostgreSqlOrderRepository>();
// Ou: .AddScoped<IOrderRepository, InMemoryOrderRepository>(); // para testes!
```

### Projeto real em 2025 (estrutura de pastas .NET com SOLID + Clean)

```plaintext
src/
├── Domain/                 → Entities, Value Objects, Interfaces de domínio
├── Application/            → Use Cases, DTOs, Interfaces (IOrderRepository)
├── Infrastructure/         → EF Core, Repositórios concretos, Services externos
└── API/                    → Controllers, Program.cs, injeção de dependência
```

### Conclusão de quem já aplicou em produção

Quem domina SOLID em .NET:
- Escreve testes unitários em 5 minutos
- Muda de SQL Server para PostgreSQL com 1 linha no Program.cs
- Adiciona uma nova regra de negócio sem medo de quebrar o sistema
- Entrega refatorações sem medo de regressão

SOLID não é teoria.  
É o que separa código júnior de código sênior que dura 10+ anos.

Em 2025, SOLID + MediatR + FluentValidation + Minimal APIs = o padrão ouro do ecossistema .NET.
```
