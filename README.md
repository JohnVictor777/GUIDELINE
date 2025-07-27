# 🧭 Guideline de Desenvolvimento em C#

> Boas práticas, estrutura, estilo e padrões para projetos desenvolvidos em C#. Adaptado para projetos individuais ou colaborativos.

---

## 📁 Estrutura do Projeto

```

/SeuProjeto
│
├── /src                # Código-fonte principal
│   ├── /Models         # Modelos de dados (DTOs, entidades)
│   ├── /Services       # Serviços e regras de negócio
│   ├── /Controllers    # Controladores (Web APIs, MVC)
│   └── /Utils          # Utilitários e helpers
│
├── /tests              # Testes automatizados (xUnit, NUnit ou MSTest)
├── /docs               # Documentação
├── .gitignore
├── README.md
├── GUIDELINE.md
└── SeuProjeto.sln

````

---

## 🔤 Convenções de Nomeação

| Tipo               | Convenção     | Exemplo                   |
|--------------------|---------------|---------------------------|
| Classes            | PascalCase    | `ClienteService`          |
| Métodos            | PascalCase    | `CadastrarCliente()`      |
| Variáveis locais   | camelCase     | `valorTotal`              |
| Parâmetros         | camelCase     | `clienteId`               |
| Interfaces         | Prefixo `I`   | `IClienteRepository`      |
| Constantes         | UPPER_CASE    | `TAXA_DESCONTO`           |

---

## 🧼 Boas Práticas

### ✔️ Organização e Padrões

- Uma classe = uma responsabilidade (SRP - SOLID)
- Prefira **injeção de dependência** a instanciar diretamente
- Divida responsabilidades usando **interfaces** e **camadas claras**

### ❌ Evite

```csharp
ClienteService servico = new ClienteService(); // Má prática
````

### ✅ Prefira

```csharp
public class ClienteController
{
    private readonly IClienteService _clienteService;

    public ClienteController(IClienteService clienteService)
    {
        _clienteService = clienteService;
    }
}
```

---

## 🚨 Tratamento de Erros

### ❌ Evite capturar `Exception` genérica:

```csharp
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

### ✅ Prefira exceções específicas + logging:

```csharp
catch (SqlException ex)
{
    _logger.LogError(ex, "Erro ao acessar o banco de dados.");
}
```

---

## 🔄 Assincronicidade

* Use `async`/`await` em chamadas que envolvem I/O (ex: banco de dados, APIs).
* Evite `.Result` e `.Wait()` para não causar deadlocks.
* Em bibliotecas, utilize `.ConfigureAwait(false)` quando apropriado.

```csharp
public async Task<IEnumerable<Cliente>> ListarClientesAsync()
{
    return await _clienteRepository.ObterTodosAsync();
}
```

---

## 📄 Documentação de Código

Use **comentários XML** para métodos públicos e APIs.

```csharp
/// <summary>
/// Cadastra um novo cliente.
/// </summary>
/// <param name="cliente">Objeto cliente a ser salvo.</param>
public void CadastrarCliente(Cliente cliente)
{
    // ...
}
```

---

## 🔧 Injeção de Dependência

Configure serviços no `Program.cs` ou `Startup.cs`:

```csharp
services.AddScoped<IClienteService, ClienteService>();
services.AddScoped<IClienteRepository, ClienteRepository>();
```

---

## 🧪 Testes Automatizados

* Use **xUnit**, **NUnit** ou **MSTest**
* Nomeie os testes com o padrão: `Metodo_Cenario_ResultadoEsperado`

```csharp
[Fact]
public void CalcularDesconto_ClienteVip_DeveRetornarDescontoMaior()
{
    // Arrange

    // Act

    // Assert
}
```

---

## 🧰 Ferramentas Recomendadas

| Propósito         | Ferramenta            |
| ----------------- | --------------------  |
| Testes            | xUnit, NUnit ou MSTest|
| Logging           | Serilog / NLog        |
| DI e Configuração | Microsoft.Extensions  |
| Swagger           | Swashbuckle           |
| Análise Estática  | SonarQube / Roslyn    |

---

## 🌐 Documentação de API com Swagger

Adicione suporte com o NuGet:

```bash
dotnet add package Swashbuckle.AspNetCore
```

Registre no `Program.cs`:

```csharp
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
```

---

## 🧾 .gitignore (exemplo)

```gitignore
bin/
obj/
.vs/
*.user
*.suo
*.cache
*.log
*.db
*.DS_Store
appsettings.*.json
```

---

## 🌱 Git Flow

### Branches

| Tipo    | Padrão                    |
| ------- | ------------------------- |
| Feature | `feature/nome-da-feature` |
| Bugfix  | `bugfix/corrigir-x`       |
| Release | `release/v1.0`            |
| Hotfix  | `hotfix/ajuste-x`         |

### Commits

| Prefixo     | Descrição                   |
| ----------- | --------------------------- |
| `feat:`     | Nova funcionalidade         |
| `fix:`      | Correção de bug             |
| `refactor:` | Refatoração de código       |
| `docs:`     | Atualização de documentação |
| `test:`     | Adição ou ajuste de testes  |

---

## 🤝 Contribuindo

1. Faça um fork do projeto
2. Crie uma nova branch: `git checkout -b feature/nova-feature`
3. Commit suas alterações: `git commit -m 'feat: adicionar nova feature'`
4. Push para o branch: `git push origin feature/nova-feature`
5. Abra um Pull Request

---

## 📚 Referências

* [C# Coding Standards (by K.Taranov)](https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards.md)
* [Documentação Oficial C# - Microsoft](https://learn.microsoft.com/dotnet/csharp/)
* [Guia de Estilo para C# - Microsoft](https://learn.microsoft.com/dotnet/standard/design-guidelines/)

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).
