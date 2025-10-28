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

### src/LegalManagement.WebAPI

**Capa de Presentación (API REST)** - Contiene los controladores y endpoints de la API REST para el sistema jurídico. Esta capa expone los servicios de dominio y aplicación a través de una interfaz HTTP, manejando la autenticación, validación de entrada, y sistema de notificaciones. Sirve como punto de entrada principal para las solicitudes del cliente y coordina la comunicación entre las capas internas y el mundo exterior, siguiendo los principios RESTful y utilizando tecnologías como ASP.NET Core, JWT, Swagger/OpenAPI, y SignalR.

Para más detalles, consultar el [README de WebAPI](./src/LegalManagement.WebAPI/README.md).

### src/LegalManagement.BlazorApp

**Capa de Presentación (Interfaz de Usuario)** - Aplicación Blazor Server que construye la interfaz interactiva de usuario para clientes, abogados y administradores. Utiliza el patrón MVVM (Model-View-ViewModel) para separar la lógica de presentación de la UI, proporcionando una experiencia moderna y responsiva. Se comunica con la WebAPI mediante HttpClient para consumir los servicios REST del sistema.

#### Características Principales:
- **Blazor Server**: Desarrollo full-stack en C# con interactividad en tiempo real mediante SignalR
- **Patrón MVVM**: Separación clara entre UI, lógica de presentación y datos
- **Integración con API**: Consumo de endpoints REST de LegalManagement.WebAPI
- **Roles de Usuario**: Interfaces personalizadas para clientes, abogados y administradores
- **Experiencia Moderna**: UI responsiva con validación en tiempo real y actualizaciones automáticas

Para más detalles sobre los beneficios de Blazor, arquitectura MVVM y relación con la API, consultar el [README de BlazorApp](./src/LegalManagement.BlazorApp/README.md).
