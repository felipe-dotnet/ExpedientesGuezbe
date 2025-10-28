# LegalManagement.UnitTests

## Finalidad

Este directorio contiene las **pruebas unitarias** del sistema LegalManagement. Las pruebas unitarias se enfocan en verificar el comportamiento de componentes individuales de forma aislada, sin dependencias externas.

### Áreas de Cobertura

- **Domain**: Validación de entidades, value objects, reglas de negocio y lógica del dominio
- **Application**: Pruebas de casos de uso, comandos, queries y validadores

## Patrones y Herramientas Sugeridas

### Frameworks de Testing

- **xUnit**: Framework principal para escribir y ejecutar pruebas
  ```csharp
  [Fact]
  public void Should_CreateExpediente_When_ValidData()
  {
      // Arrange, Act, Assert
  }
  ```

### Mocking

- **Moq**: Para crear mocks de interfaces y dependencias
  ```csharp
  var mockRepository = new Mock<IExpedienteRepository>();
  mockRepository.Setup(x => x.GetByIdAsync(It.IsAny<Guid>()))
                .ReturnsAsync(expediente);
  ```

### Assertions

- **FluentAssertions**: Para assertions más legibles y expresivas
  ```csharp
  result.Should().NotBeNull();
  result.IsSuccess.Should().BeTrue();
  result.Value.Should().BeOfType<ExpedienteDto>();
  ```

## Principios de Pruebas Limpias

### 1. AAA Pattern (Arrange-Act-Assert)

```csharp
[Fact]
public void Should_ValidateExpediente_When_NumberIsUnique()
{
    // Arrange
    var validator = new ExpedienteValidator();
    var expediente = new Expediente("EXP-2024-001", "Caso Legal");
    
    // Act
    var result = validator.Validate(expediente);
    
    // Assert
    result.IsValid.Should().BeTrue();
}
```

### 2. Nombres Descriptivos

- Usar convención: `Should_[ExpectedBehavior]_When_[Condition]`
- Ejemplo: `Should_ThrowException_When_ExpedienteNumberIsNull`

### 3. Una Aserción Lógica por Test

- Cada test debe verificar un solo comportamiento
- Enfocarse en un escenario específico

### 4. Independencia

- Los tests no deben depender entre sí
- Cada test debe poder ejecutarse en cualquier orden

### 5. FIRST Principles

- **F**ast: Pruebas rápidas (sin I/O, sin DB)
- **I**ndependent: Sin dependencias entre tests
- **R**epeatable: Mismos resultados en cualquier entorno
- **S**elf-validating: Pass o Fail automático
- **T**imely: Escritas junto al código de producción

### 6. Test Doubles

- **Mocks**: Para verificar interacciones
- **Stubs**: Para proveer datos de prueba
- **Fakes**: Implementaciones simplificadas

## Estructura de Carpetas Sugerida

```
LegalManagement.UnitTests/
├── Domain/
│   ├── Entities/
│   ├── ValueObjects/
│   └── Services/
├── Application/
│   ├── Commands/
│   ├── Queries/
│   └── Validators/
└── Common/
    └── TestHelpers/
```

## Ejemplo de Test Unitario

```csharp
using Xunit;
using Moq;
using FluentAssertions;
using LegalManagement.Domain.Entities;
using LegalManagement.Application.Commands;

namespace LegalManagement.UnitTests.Application.Commands
{
    public class CreateExpedienteCommandHandlerTests
    {
        [Fact]
        public async Task Should_CreateExpediente_When_ValidDataProvided()
        {
            // Arrange
            var mockRepository = new Mock<IExpedienteRepository>();
            var handler = new CreateExpedienteCommandHandler(mockRepository.Object);
            var command = new CreateExpedienteCommand
            {
                Numero = "EXP-2024-001",
                Descripcion = "Caso de prueba"
            };

            // Act
            var result = await handler.Handle(command, CancellationToken.None);

            // Assert
            result.Should().NotBeNull();
            result.IsSuccess.Should().BeTrue();
            mockRepository.Verify(x => x.AddAsync(It.IsAny<Expediente>()), Times.Once);
        }

        [Fact]
        public void Should_ThrowException_When_NumeroIsNull()
        {
            // Arrange
            var mockRepository = new Mock<IExpedienteRepository>();
            var handler = new CreateExpedienteCommandHandler(mockRepository.Object);
            var command = new CreateExpedienteCommand
            {
                Numero = null,
                Descripcion = "Caso de prueba"
            };

            // Act
            Func<Task> act = async () => await handler.Handle(command, CancellationToken.None);

            // Assert
            act.Should().ThrowAsync<ArgumentNullException>();
        }
    }
}
```

## Comandos Útiles

```bash
# Ejecutar todas las pruebas unitarias
dotnet test

# Ejecutar con cobertura de código
dotnet test /p:CollectCoverage=true

# Ejecutar pruebas específicas
dotnet test --filter "FullyQualifiedName~CreateExpedienteCommandHandlerTests"
```

## Referencias

- [xUnit Documentation](https://xunit.net/)
- [Moq Documentation](https://github.com/moq/moq4)
- [FluentAssertions Documentation](https://fluentassertions.com/)
- [Clean Code Testing Principles](https://github.com/testdouble/contributing-tests/wiki/Test-Driven-Development)
