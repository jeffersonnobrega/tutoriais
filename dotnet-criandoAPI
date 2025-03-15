# 📘 Tutorial: Criando uma API .NET com SQL Server e Deploy no Azure

## 🏗️ Passo 1: Criando o Projeto

```bash
dotnet new webapi -n FinancialAPI
cd FinancialAPI
```

O comando `dotnet new webapi` cria um novo projeto Web API em .NET, enquanto `-n FinancialAPI` define o nome do projeto.

---

## 📁 Passo 2: Configurando o Banco de Dados (SQL Server Local)

Edite o arquivo `appsettings.json` para adicionar a string de conexão ao SQL Server Express:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.\\SQLEXPRESS;Database=FinancialDB;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

Isso configura o banco de dados local usando o SQL Server Express.

---

## 📌 Passo 3: Criando o Modelo (Model)

O **Model** representa os dados que serão armazenados no banco:

```csharp
public class Expense
{
    public int Id { get; set; }
    public string Description { get; set; }
    public decimal Amount { get; set; }
    public DateTime Date { get; set; }
}
```

---

## 🔄 Passo 4: Criando o Contexto do Banco de Dados

O **DbContext** gerencia a conexão com o banco:

```csharp
using Microsoft.EntityFrameworkCore;

public class FinancialDbContext : DbContext
{
    public FinancialDbContext(DbContextOptions<FinancialDbContext> options) : base(options) { }
    public DbSet<Expense> Expenses { get; set; }
}
```

---

## 🛠️ Passo 5: Criando a Controller

A **Controller** expõe os endpoints da API:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

[Route("api/[controller]")]
[ApiController]
public class ExpensesController : ControllerBase
{
    private readonly FinancialDbContext _context;
    
    public ExpensesController(FinancialDbContext context)
    {
        _context = context;
    }

    // GET: api/expenses
    [HttpGet]
    public async Task<ActionResult<IEnumerable<Expense>>> GetExpenses()
    {
        return await _context.Expenses.ToListAsync();
    }

    // POST: api/expenses
    [HttpPost]
    public async Task<ActionResult<Expense>> PostExpense(Expense expense)
    {
        _context.Expenses.Add(expense);
        await _context.SaveChangesAsync();
        return CreatedAtAction(nameof(GetExpenses), new { id = expense.Id }, expense);
    }
}
```

---

## 🚀 Passo 6: Executando a API

```bash
dotnet run
```

A API estará disponível em `http://localhost:5000`.

---

## 🛳️ Passo 7: Criando o Container Docker

Crie um arquivo `Dockerfile` na raiz do projeto:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
COPY . /app
WORKDIR /app
ENTRYPOINT ["dotnet", "FinancialAPI.dll"]
```

Construa e rode o container:

```bash
docker build -t financial-api .
docker run -p 5000:5000 financial-api
```

---

## 🌍 Passo 8: Deploy no Azure

```bash
az login
az group create --name FinancialAPIGroup --location eastus
az container create --resource-group FinancialAPIGroup --name financialapi --image seu-usuario/financial-api --dns-name-label financialapi --ports 80
```

Acesse em `http://financialapi.eastus.azurecontainer.io`

---

## 🔄 Passo 9: CI/CD com GitHub Actions

Crie o arquivo `.github/workflows/deploy.yml`:

```yaml
name: Deploy para Azure

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build Docker Image
        run: docker build -t financial-api .
      - name: Push para Azure
        run: az acr login --name seuacr && docker push seuacr.azurecr.io/financial-api
```

---

## 🎯 Conclusão

Parabéns! 🎉 Agora você tem uma API completa com SQL Server, Docker e CI/CD no Azure! 🚀

