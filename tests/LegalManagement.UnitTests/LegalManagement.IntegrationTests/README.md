# LegalManagement.IntegrationTests

## Finalidad

Este directorio contiene las **pruebas de integración** del sistema LegalManagement. Las pruebas de integración verifican el comportamiento del sistema cuando múltiples componentes trabajan juntos, incluyendo integraciones con bases de datos, APIs externas y otros servicios.

### Áreas de Cobertura

- **API**: Pruebas de endpoints REST, validación de responses, autenticación y autorización
- **Infrastructure**: Verificación de repositorios, persistencia de datos, acceso a base de datos
- **Flujo End-to-End**: Escenarios completos que atraviesan múltiples capas del sistema

## Patrones y Herramientas Sugeridas

### Frameworks de Testing

- **xUnit**: Framework principal para escribir y ejecutar pruebas de integración
  ```csharp
  [Fact]
  public async Task Should_CreateAndRetrieveExpediente_When_ValidRequest()
  {
      // Integration test con base de datos real o en memoria
  }
  ```

### Testing de APIs

- **WebApplicationFactory**: Para pruebas de APIs con ASP.NET Core
  ```csharp
  public class ExpedientesApiTests : IClassFixture<WebApplicationFactory<Program>>
  {
      private readonly HttpClient _client;

      public ExpedientesApiTests(WebApplicationFactory<Program> factory)
      {
          _client = factory.CreateClient();
      }

      [Fact]
      public async Task GetExpediente_ReturnsSuccess()
      {
          var response = await _client.GetAsync("/api/expedientes/1");
          response.EnsureSuccessStatusCode();
      }
  }
  ```

### Base de Datos de Prueba

- **EF Core In-Memory**: Para pruebas rápidas sin dependencia de SQL Server
- **Testcontainers**: Para pruebas con bases de datos reales en contenedores Docker
  ```csharp
  var container = new PostgreSqlBuilder().Build();
  await container.StartAsync();
  ```

### Assertions

- **FluentAssertions**: Para validaciones expresivas en respuestas HTTP y datos
  ```csharp
  response.StatusCode.Should().Be(HttpStatusCode.OK);
  result.Data.Should().NotBeEmpty();
  ```

## Principios de Pruebas de Integración

### 1. Ambiente Aislado

- Cada test debe ejecutarse en un ambiente limpio
- Usar bases de datos temporales o en memoria
- Limpiar datos después de cada prueba

```csharp
public class DatabaseFixture : IAsyncLifetime
{
    public ApplicationDbContext DbContext { get; private set; }

    public async Task InitializeAsync()
    {
        var options = new DbContextOptionsBuilder<ApplicationDbContext>()
            .UseInMemoryDatabase(databaseName: Guid.NewGuid().ToString())
            .Options;

        DbContext = new ApplicationDbContext(options);
        await DbContext.Database.EnsureCreatedAsync();
    }

    public async Task DisposeAsync()
    {
        await DbContext.Database.EnsureDeletedAsync();
        await DbContext.DisposeAsync();
    }
}
```

### 2. Test Fixtures para Compartir Contexto

```csharp
public class ApiIntegrationTests : IClassFixture<WebApplicationFactory<Program>>, 
                                    IClassFixture<DatabaseFixture>
{
    // Tests comparten el mismo factory y fixture de DB
}
```

### 3. Nombres Descriptivos de Escenarios

- Describir el flujo completo que se está probando
- Ejemplo: `Should_CreateExpediente_AndRetrieveIt_ThroughApi`

### 4. AAA Pattern Extendido

```csharp
[Fact]
public async Task Should_CreateAndQueryExpediente_Through_CompleteFlow()
{
    // Arrange - Preparar datos y configuración
    var request = new CreateExpedienteRequest
    {
        Numero = "EXP-2024-001",
        Descripcion = "Caso legal importante"
    };

    // Act - Ejecutar flujo completo
    var createResponse = await _client.PostAsJsonAsync("/api/expedientes", request);
    var expedienteId = await createResponse.Content.ReadFromJsonAsync<Guid>();
    var getResponse = await _client.GetAsync($"/api/expedientes/{expedienteId}");
    var expediente = await getResponse.Content.ReadFromJsonAsync<ExpedienteDto>();

    // Assert - Verificar resultado del flujo
    createResponse.StatusCode.Should().Be(HttpStatusCode.Created);
    getResponse.StatusCode.Should().Be(HttpStatusCode.OK);
    expediente.Numero.Should().Be("EXP-2024-001");
    expediente.Descripcion.Should().Be("Caso legal importante");
}
```

### 5. Manejo de Transacciones

```csharp
public class RepositoryIntegrationTests : IDisposable
{
    private readonly ApplicationDbContext _dbContext;
    private readonly IDbContextTransaction _transaction;

    public RepositoryIntegrationTests()
    {
        _dbContext = CreateDbContext();
        _transaction = _dbContext.Database.BeginTransaction();
    }

    public void Dispose()
    {
        _transaction.Rollback();
        _transaction.Dispose();
        _dbContext.Dispose();
    }
}
```

### 6. Configuración de Servicios de Prueba

```csharp
public class CustomWebApplicationFactory : WebApplicationFactory<Program>
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.ConfigureServices(services =>
        {
            // Reemplazar servicios reales con versiones de prueba
            services.RemoveAll<ApplicationDbContext>();
            services.AddDbContext<ApplicationDbContext>(options =>
            {
                options.UseInMemoryDatabase("TestDatabase");
            });
        });
    }
}
```

## Estructura de Carpetas Sugerida

```
LegalManagement.IntegrationTests/
├── API/
│   ├── Controllers/
│   │   ├── ExpedientesControllerTests.cs
│   │   └── DocumentosControllerTests.cs
│   └── Middleware/
├── Infrastructure/
│   ├── Repositories/
│   │   ├── ExpedienteRepositoryTests.cs
│   │   └── DocumentoRepositoryTests.cs
│   └── Persistence/
├── EndToEnd/
│   ├── ExpedienteWorkflowTests.cs
│   └── DocumentManagementFlowTests.cs
├── Fixtures/
│   ├── DatabaseFixture.cs
│   ├── WebApplicationFixture.cs
│   └── TestDataBuilder.cs
└── Common/
    ├── TestHelpers/
    └── Extensions/
```

## Ejemplo de Prueba de Integración API

```csharp
using System.Net;
using System.Net.Http.Json;
using Microsoft.AspNetCore.Mvc.Testing;
using Xunit;
using FluentAssertions;
using LegalManagement.API;
using LegalManagement.Application.DTOs;

namespace LegalManagement.IntegrationTests.API.Controllers
{
    public class ExpedientesControllerTests : IClassFixture<WebApplicationFactory<Program>>
    {
        private readonly HttpClient _client;

        public ExpedientesControllerTests(WebApplicationFactory<Program> factory)
        {
            _client = factory.CreateClient();
        }

        [Fact]
        public async Task Should_CreateExpediente_When_ValidRequest()
        {
            // Arrange
            var request = new CreateExpedienteRequest
            {
                Numero = "EXP-2024-100",
                Descripcion = "Nuevo expediente legal",
                ClienteId = Guid.NewGuid()
            };

            // Act
            var response = await _client.PostAsJsonAsync("/api/expedientes", request);
            var expedienteId = await response.Content.ReadFromJsonAsync<Guid>();

            // Assert
            response.StatusCode.Should().Be(HttpStatusCode.Created);
            expedienteId.Should().NotBeEmpty();
        }

        [Fact]
        public async Task Should_GetExpediente_When_Exists()
        {
            // Arrange - Primero crear un expediente
            var createRequest = new CreateExpedienteRequest
            {
                Numero = "EXP-2024-101",
                Descripcion = "Expediente para consulta",
                ClienteId = Guid.NewGuid()
            };
            var createResponse = await _client.PostAsJsonAsync("/api/expedientes", createRequest);
            var expedienteId = await createResponse.Content.ReadFromJsonAsync<Guid>();

            // Act - Luego recuperarlo
            var getResponse = await _client.GetAsync($"/api/expedientes/{expedienteId}");
            var expediente = await getResponse.Content.ReadFromJsonAsync<ExpedienteDto>();

            // Assert
            getResponse.StatusCode.Should().Be(HttpStatusCode.OK);
            expediente.Should().NotBeNull();
            expediente.Id.Should().Be(expedienteId);
            expediente.Numero.Should().Be("EXP-2024-101");
        }

        [Fact]
        public async Task Should_ReturnBadRequest_When_InvalidData()
        {
            // Arrange
            var request = new CreateExpedienteRequest
            {
                Numero = null, // Número inválido
                Descripcion = "Descripción"
            };

            // Act
            var response = await _client.PostAsJsonAsync("/api/expedientes", request);

            // Assert
            response.StatusCode.Should().Be(HttpStatusCode.BadRequest);
        }
    }
}
```

## Ejemplo de Prueba de Repositorio

```csharp
using Microsoft.EntityFrameworkCore;
using Xunit;
using FluentAssertions;
using LegalManagement.Domain.Entities;
using LegalManagement.Infrastructure.Persistence;
using LegalManagement.Infrastructure.Repositories;

namespace LegalManagement.IntegrationTests.Infrastructure.Repositories
{
    public class ExpedienteRepositoryTests : IAsyncLifetime
    {
        private ApplicationDbContext _dbContext;
        private ExpedienteRepository _repository;

        public async Task InitializeAsync()
        {
            var options = new DbContextOptionsBuilder<ApplicationDbContext>()
                .UseInMemoryDatabase(databaseName: Guid.NewGuid().ToString())
                .Options;

            _dbContext = new ApplicationDbContext(options);
            _repository = new ExpedienteRepository(_dbContext);
            
            await _dbContext.Database.EnsureCreatedAsync();
        }

        public async Task DisposeAsync()
        {
            await _dbContext.Database.EnsureDeletedAsync();
            await _dbContext.DisposeAsync();
        }

        [Fact]
        public async Task Should_SaveAndRetrieveExpediente()
        {
            // Arrange
            var expediente = new Expediente
            {
                Id = Guid.NewGuid(),
                Numero = "EXP-2024-200",
                Descripcion = "Expediente de prueba",
                FechaCreacion = DateTime.UtcNow
            };

            // Act
            await _repository.AddAsync(expediente);
            await _dbContext.SaveChangesAsync();
            
            var retrieved = await _repository.GetByIdAsync(expediente.Id);

            // Assert
            retrieved.Should().NotBeNull();
            retrieved.Id.Should().Be(expediente.Id);
            retrieved.Numero.Should().Be("EXP-2024-200");
        }
    }
}
```

## Comandos Útiles

```bash
# Ejecutar todas las pruebas de integración
dotnet test --filter Category=Integration

# Ejecutar con logging detallado
dotnet test --logger "console;verbosity=detailed"

# Ejecutar pruebas específicas de API
dotnet test --filter "FullyQualifiedName~API"

# Ejecutar con cobertura
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover
```

## Consideraciones Importantes

### Performance
- Las pruebas de integración son más lentas que las unitarias
- Usar bases de datos en memoria cuando sea posible
- Parallelizar tests cuando no compartan estado

### Datos de Prueba
- Usar builders o factories para crear datos de prueba consistentes
- Evitar magic numbers y strings hardcodeados
- Considerar usar bibliotecas como Bogus para generar datos

### CI/CD
- Configurar variables de entorno para diferentes ambientes
- Usar contenedores para pruebas con servicios externos
- Separar tests rápidos de lentos en pipelines

## Referencias

- [xUnit Documentation](https://xunit.net/)
- [ASP.NET Core Integration Tests](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests)
- [Testcontainers for .NET](https://dotnet.testcontainers.org/)
- [FluentAssertions Documentation](https://fluentassertions.com/)
- [EF Core Testing](https://learn.microsoft.com/en-us/ef/core/testing/)
