# LegalManagement.BlazorApp

## 📋 Descripción

Este proyecto construye la **interfaz interactiva de usuario** para el Sistema de Gestión Legal, utilizando **Blazor Server**. Proporciona una experiencia de usuario moderna y responsiva para tres tipos de usuarios principales: clientes, abogados y administradores.

## 🎯 Función Principal

La aplicación Blazor Server actúa como la capa de presentación del sistema, permitiendo:

- **Para Clientes**: Consultar expedientes, ver documentos, seguimiento de casos
- **Para Abogados**: Gestión completa de expedientes, actualización de estados, comunicación con clientes
- **Para Administradores**: Control total del sistema, gestión de usuarios, reportes y estadísticas

## ✨ Beneficios de Blazor Server

### 1. **Desarrollo con C#**
- Todo el código frontend y backend en C#
- Reutilización de lógica de negocio y modelos
- No necesita JavaScript para la lógica de la aplicación

### 2. **Interactividad en Tiempo Real**
- Actualizaciones automáticas mediante SignalR
- Experiencia de usuario fluida similar a SPA
- Sincronización instantánea de datos

### 3. **Rendimiento Optimizado**
- Procesamiento en el servidor
- Transferencia mínima de datos
- Ideal para aplicaciones empresariales complejas

### 4. **Seguridad Mejorada**
- Lógica de negocio protegida en el servidor
- Validación centralizada
- Menor superficie de ataque

### 5. **Mantenimiento Simplificado**
- Base de código unificada
- Menos complejidad técnica
- Actualizaciones centralizadas

## 🏗️ Arquitectura y Patrón MVVM

### Implementación del Patrón MVVM

El proyecto utiliza el patrón **Model-View-ViewModel (MVVM)** para separar responsabilidades:

```
┌─────────────────────────────────────────┐
│            VIEW (Razor)                 │
│  - Componentes .razor                   │
│  - Interfaz de usuario                  │
│  - Data binding                         │
└─────────────────┬───────────────────────┘
                  │
                  │ Data Binding
                  │
┌─────────────────▼───────────────────────┐
│         VIEWMODEL (Code-Behind)         │
│  - Lógica de presentación               │
│  - Propiedades observables              │
│  - Comandos y eventos                   │
└─────────────────┬───────────────────────┘
                  │
                  │ Servicios
                  │
┌─────────────────▼───────────────────────┐
│            MODEL (API/DTOs)             │
│  - Datos del dominio                    │
│  - Llamadas a la API                    │
│  - Entidades y DTOs                     │
└─────────────────────────────────────────┘
```

### Beneficios del MVVM en Blazor:

- **Separación de Concernimientos**: La UI está desacoplada de la lógica de negocio
- **Testabilidad**: ViewModels pueden ser probados sin la UI
- **Reutilización**: ViewModels pueden compartirse entre componentes
- **Mantenibilidad**: Cambios en la UI no afectan la lógica de negocio

## 🔗 Relación con la API (WebAPI)

### Arquitectura de Comunicación

```
┌──────────────────────────────────────────────────┐
│         Blazor Server Application                │
│                                                  │
│  ┌────────────────────────────────────────┐    │
│  │         Razor Components                │    │
│  └────────────────┬───────────────────────┘    │
│                   │                             │
│  ┌────────────────▼───────────────────────┐    │
│  │       HttpClient Services              │    │
│  │  - ExpedienteService                   │    │
│  │  - DocumentoService                    │    │
│  │  - UsuarioService                      │    │
│  └────────────────┬───────────────────────┘    │
└───────────────────┼──────────────────────────────┘
                    │
                    │ HTTP/HTTPS
                    │ (REST API Calls)
                    │
┌───────────────────▼──────────────────────────────┐
│        LegalManagement.WebAPI                    │
│                                                  │
│  ┌────────────────────────────────────────┐    │
│  │          Controllers                   │    │
│  │  - ExpedientesController               │    │
│  │  - DocumentosController                │    │
│  │  - UsuariosController                  │    │
│  └────────────────┬───────────────────────┘    │
│                   │                             │
│  ┌────────────────▼───────────────────────┐    │
│  │     Application Layer (CQRS)           │    │
│  └────────────────────────────────────────┘    │
└──────────────────────────────────────────────────┘
```

### Flujo de Datos:

1. **Usuario interactúa** con el componente Razor
2. **ViewModel procesa** el evento y llama al servicio HTTP
3. **HttpClient envía** la petición a la WebAPI
4. **WebAPI procesa** la solicitud (validación, autorización)
5. **Application Layer** ejecuta el comando/query (CQRS)
6. **API retorna** la respuesta (éxito o error)
7. **Blazor actualiza** la UI automáticamente

### Servicios de Integración:

- **ApiClientService**: Cliente HTTP configurado con autenticación
- **AuthenticationService**: Gestión de tokens JWT
- **StateManagement**: Almacenamiento de estado de la aplicación
- **SignalR Hub**: Notificaciones en tiempo real

## 📁 Estructura del Proyecto (Planificada)

```
LegalManagement.BlazorApp/
├── Components/
│   ├── Pages/              # Páginas principales
│   ├── Layout/             # Layouts y navegación
│   ├── Shared/             # Componentes reutilizables
│   └── Forms/              # Formularios complejos
├── Services/
│   ├── API/                # Servicios de comunicación con API
│   ├── State/              # Gestión de estado
│   └── Authentication/     # Servicios de autenticación
├── ViewModels/             # ViewModels para MVVM
├── Models/                 # DTOs y modelos de vista
├── wwwroot/                # Recursos estáticos
│   ├── css/
│   ├── js/
│   └── images/
├── Program.cs              # Configuración de la aplicación
└── appsettings.json        # Configuración
```

## 🚀 Características Principales

### Módulos Funcionales:

1. **Dashboard**
   - Panel de control personalizado por rol
   - Métricas y estadísticas en tiempo real
   - Notificaciones y alertas

2. **Gestión de Expedientes**
   - CRUD completo de expedientes
   - Búsqueda y filtrado avanzado
   - Visualización de cronología

3. **Gestión de Documentos**
   - Carga y descarga de archivos
   - Previsualización de documentos
   - Control de versiones

4. **Gestión de Usuarios**
   - Administración de cuentas
   - Asignación de roles y permisos
   - Auditoría de actividades

5. **Reportes**
   - Generación de reportes personalizados
   - Exportación a PDF/Excel
   - Gráficos y visualizaciones

## 🔐 Seguridad

- **Autenticación JWT**: Tokens seguros para cada sesión
- **Autorización basada en Roles**: Control de acceso granular
- **Validación Client-Side**: Validación inmediata en formularios
- **HTTPS Obligatorio**: Comunicación encriptada
- **CORS Configurado**: Protección contra peticiones no autorizadas

## 🛠️ Tecnologías y Dependencias

- **.NET 8.0**
- **Blazor Server**
- **Bootstrap 5** (UI Framework)
- **SignalR** (Real-time communication)
- **HttpClient** (API Communication)
- **FluentValidation** (Client-side validation)
- **Blazored.LocalStorage** (State management)

## 📊 Estado del Proyecto

**Status**: ⏳ En Planificación

### Próximos Pasos:
1. Configurar el proyecto Blazor Server
2. Implementar autenticación y autorización
3. Crear componentes base y layout
4. Desarrollar módulos principales
5. Integrar con la WebAPI
6. Implementar pruebas unitarias e integración

## 🔄 Integración con Otros Proyectos

- **LegalManagement.WebAPI**: Consume los endpoints REST
- **LegalManagement.Application**: Comparte DTOs y contratos
- **LegalManagement.Domain**: Utiliza las entidades del dominio

---

**Nota**: Este README será actualizado conforme se desarrolle el proyecto Blazor Server.
