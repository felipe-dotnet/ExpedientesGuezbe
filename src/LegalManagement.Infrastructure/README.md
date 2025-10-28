# LegalManagement.Infrastructure

## Descripción

Esta capa implementa los detalles técnicos y el acceso a datos del sistema de gestión jurídica. Es responsable de la persistencia de datos, integraciones con sistemas externos, y la implementación concreta de las interfaces definidas en la capa de dominio, siguiendo los principios de Clean Architecture.

## Responsabilidades

### Acceso a Datos
- **Entity Framework Core**: Implementación de repositorios usando EF Core como ORM
- **SQL Server**: Base de datos relacional para almacenamiento persistente del sistema jurídico
- **DbContext**: Configuración del contexto de base de datos
- **Migrations**: Gestión de cambios en el esquema de base de datos

### Configuraciones de Entidades
- Mapeo de entidades de dominio a tablas de base de datos
- Configuración de relaciones, índices y restricciones
- Implementación de conversiones de tipos personalizados
- Configuración de propiedades de auditoría

### Implementación de Repositorios
- Repositorios concretos que implementan interfaces del dominio
- Optimización de consultas con LINQ
- Manejo de transacciones
- Implementación de patrones Unit of Work

### Integraciones Externas
- Servicios de notificación (email, SMS)
- Integraciones con sistemas de gestión documental
- APIs de terceros relacionadas con servicios jurídicos
- Servicios de autenticación y autorización externos

## Principios de Clean Architecture

Esta capa depende de:
- **LegalManagement.Domain**: Para acceder a entidades y interfaces
- **LegalManagement.Application**: Para implementar interfaces de infraestructura

Esta capa NO debe:
- Contener lógica de negocio
- Exponer detalles de implementación a capas superiores
- Depender de la capa de presentación

## Estructura

```
LegalManagement.Infrastructure/
├── Persistence/
│   ├── Configurations/      # Configuraciones de Entity Framework
│   ├── Repositories/        # Implementación de repositorios
│   └── ApplicationDbContext.cs
├── ExternalServices/        # Integraciones con servicios externos
├── Identity/                # Implementación de autenticación
└── Migrations/              # Migraciones de base de datos
```

## Tecnologías

- **Entity Framework Core**: ORM para acceso a datos
- **SQL Server**: Sistema de gestión de base de datos
- **Dapper** (opcional): Para consultas optimizadas
- **MediatR**: Para manejo de eventos de dominio

## Dependencias

- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

## Notas

Esta capa implementa el patrón Repository y Unit of Work, siguiendo los principios de Clean Architecture donde las dependencias apuntan hacia adentro (hacia el dominio).
