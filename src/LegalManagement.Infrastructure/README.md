# LegalManagement.Infrastructure

## Descripción
Capa de infraestructura que implementa las interfaces definidas en las capas superiores y gestiona el acceso a recursos externos.

## Responsabilidades
- Implementar los repositorios definidos en el dominio
- Gestionar el acceso a bases de datos
- Implementar servicios de infraestructura externos
- Configurar Entity Framework Core y migraciones
- Gestionar la persistencia de datos
- Implementar servicios de email, archivos, etc.

## Estructura
```
LegalManagement.Infrastructure/
├── Data/              # Contexto de base de datos y configuraciones
├── Repositories/      # Implementaciones de repositorios
├── Services/          # Servicios de infraestructura
├── Configurations/    # Configuraciones de entidades EF Core
├── Migrations/        # Migraciones de base de datos
└── External/          # Integraciones con servicios externos
```

## Dependencias
- **LegalManagement.Domain**: Implementa las interfaces del dominio
- **LegalManagement.Application**: Puede implementar interfaces de aplicación
- Entity Framework Core
- Bibliotecas de terceros necesarias

## Tecnologías
- Entity Framework Core
- SQL Server / PostgreSQL
- Dapper (opcional para consultas complejas)
