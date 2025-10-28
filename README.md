# ExpedientesGuezbe

Sistema de gestión jurídica desarrollado con Clean Architecture en .NET.

## Estructura del Proyecto

### src/LegalManagement.Domain

**Capa de Dominio** - Contiene las entidades de negocio y lógica de dominio del sistema jurídico. Esta capa es el corazón de la aplicación y define las reglas de negocio sin depender de tecnologías externas.

Para más detalles, consultar el [README de Domain](./src/LegalManagement.Domain/README.md).

### src/LegalManagement.Application

**Capa de Aplicación** - Implementa los casos de uso y la lógica de aplicación del sistema jurídico, orquestando las operaciones entre el dominio y las capas externas. Aplica patrones como CQRS (Command Query Responsibility Segregation), Mediator y Repository siguiendo los principios de Clean Architecture para mantener la independencia del dominio y garantizar la testabilidad y mantenibilidad del código.

Para más detalles, consultar el [README de Application](./src/LegalManagement.Application/README.md).

### src/LegalManagement.Infrastructure

**Capa de Infraestructura** - Implementa los detalles técnicos y el acceso a datos del sistema de gestión jurídica. Utiliza Entity Framework Core como ORM para interactuar con SQL Server, maneja integraciones con servicios externos, y proporciona implementaciones concretas de las interfaces definidas en las capas de dominio y aplicación, siguiendo los principios de Clean Architecture. Esta capa es responsable de la persistencia de datos, configuración de entidades, implementación de repositorios, y manejo de servicios de infraestructura como notificaciones y autenticación.

Para más detalles, consultar el [README de Infrastructure](./src/LegalManagement.Infrastructure/README.md).
