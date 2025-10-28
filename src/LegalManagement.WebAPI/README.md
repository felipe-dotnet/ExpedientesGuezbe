# LegalManagement.WebAPI

## Descripción

Esta capa contiene los controladores y endpoints de la API REST para el sistema jurídico ExpedientesGuezbe. Sirve como punto de entrada principal para todas las solicitudes HTTP del sistema.

## Responsabilidades

### 1. Exposición de Servicios
- **Servicios de Dominio**: Expone la lógica de negocio encapsulada en los servicios del dominio
- **Servicios de Aplicación**: Proporciona acceso a los casos de uso y operaciones de aplicación
- **Integración de Capas**: Coordina las llamadas entre las capas de aplicación, dominio e infraestructura

### 2. Gestión de Autenticación
- Implementación de middleware de autenticación y autorización
- Validación de tokens JWT
- Control de acceso basado en roles (RBAC)
- Gestión de políticas de seguridad

### 3. Validación de Entrada
- Validación de modelos de entrada (DTOs)
- Validación de parámetros de consulta y rutas
- Manejo de reglas de validación personalizadas
- Respuestas estandarizadas de errores de validación

### 4. Sistema de Notificaciones
- Gestión de notificaciones en tiempo real
- Integración con servicios de mensajería
- Manejo de eventos del sistema
- Publicación de cambios a clientes conectados

## Estructura Típica

```
LegalManagement.WebAPI/
├── Controllers/
│   ├── ExpedientesController.cs
│   ├── ClientesController.cs
│   └── NotificacionesController.cs
├── Middleware/
│   ├── AuthenticationMiddleware.cs
│   └── ExceptionHandlingMiddleware.cs
├── Filters/
│   └── ValidationFilter.cs
├── DTOs/
│   ├── Request/
│   └── Response/
└── Program.cs
```

## Tecnologías Utilizadas

- ASP.NET Core Web API
- Swagger/OpenAPI para documentación
- JWT para autenticación
- FluentValidation para validación de entrada
- SignalR para notificaciones en tiempo real

## Principios de Diseño

- **RESTful API**: Implementación de principios REST para comunicación HTTP
- **Separación de Responsabilidades**: La API no contiene lógica de negocio
- **Inyección de Dependencias**: Uso extensivo de DI para desacoplamiento
- **Documentación Automática**: Swagger para documentación interactiva de endpoints
