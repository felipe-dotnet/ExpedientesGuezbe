# LegalManagement.Application

## Propósito

Esta capa contiene la **lógica de aplicación** del sistema de gestión jurídica ExpedientesGuezbe, implementando los casos de uso siguiendo los principios de **Clean Architecture**.

## Responsabilidades

### 1. Casos de Uso (Use Cases)
- Orquestación de la lógica de negocio para operaciones específicas
- Coordinación entre entidades del dominio y servicios externos
- Implementación de reglas de aplicación (no de dominio)

### 2. Interfaces y Contratos
- Definición de interfaces para repositorios
- Contratos de servicios externos
- DTOs (Data Transfer Objects) para entrada/salida

### 3. Patrones Implementados

#### CQRS (Command Query Responsibility Segregation)
- **Commands**: Operaciones que modifican el estado del sistema
- **Queries**: Operaciones de solo lectura
- Separación clara entre escritura y lectura

#### Mediator Pattern
- Desacoplamiento entre emisores de comandos/consultas y sus manejadores
- Centralización del flujo de control de la aplicación

#### Repository Pattern
- Abstracción del acceso a datos
- Independencia de la capa de persistencia

### 4. Servicios de Aplicación
- Validación de datos de entrada
- Transformación entre DTOs y entidades de dominio
- Manejo de transacciones
- Logging y auditoría

## Estructura Propuesta

```
LegalManagement.Application/
├── Common/
│   ├── Interfaces/          # Interfaces de repositorios y servicios
│   ├── Behaviors/           # Comportamientos transversales (validación, logging)
│   └── Mappings/            # Perfiles de AutoMapper
├── UseCases/
│   ├── Expedientes/
│   │   ├── Commands/        # Comandos para expedientes
│   │   └── Queries/         # Consultas para expedientes
│   ├── Casos/
│   │   ├── Commands/
│   │   └── Queries/
│   └── Documentos/
│       ├── Commands/
│       └── Queries/
└── DTOs/                    # Data Transfer Objects
    ├── Requests/
    └── Responses/
```

## Dependencias

- **LegalManagement.Domain**: Esta capa depende del dominio para acceder a entidades y reglas de negocio
- **No debe depender** de capas de infraestructura o presentación

## Principios de Clean Architecture Aplicados

1. **Independencia de Frameworks**: La lógica de aplicación no depende de frameworks específicos
2. **Testabilidad**: Todos los casos de uso son fácilmente testeables de forma unitaria
3. **Independencia de UI**: La lógica no conoce detalles de la interfaz de usuario
4. **Independencia de Base de Datos**: Trabajo con abstracciones (interfaces) no implementaciones
5. **Regla de Dependencia**: Las dependencias apuntan hacia adentro (hacia el dominio)

## Beneficios

- ✅ **Mantenibilidad**: Código organizado y fácil de mantener
- ✅ **Escalabilidad**: Fácil agregar nuevos casos de uso
- ✅ **Testabilidad**: Alta cobertura de pruebas unitarias
- ✅ **Flexibilidad**: Cambios en infraestructura no afectan la lógica de aplicación
- ✅ **Claridad**: Separación clara de responsabilidades
