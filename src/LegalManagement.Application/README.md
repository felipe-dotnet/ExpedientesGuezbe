# LegalManagement.Application

## Descripción
Capa de aplicación que contiene la lógica de orquestación y casos de uso del sistema.

## Responsabilidades
- Definir e implementar los casos de uso de la aplicación
- Crear DTOs (Data Transfer Objects) para transferencia de datos
- Implementar validaciones de entrada
- Orquestar el flujo de datos entre capas
- Definir interfaces de servicios de aplicación
- Implementar mapeo entre entidades y DTOs

## Estructura
```
LegalManagement.Application/
├── UseCases/          # Casos de uso
├── DTOs/              # Data Transfer Objects
├── Validators/        # Validadores de entrada
├── Interfaces/        # Interfaces de servicios
├── Mappings/          # Perfiles de mapeo
└── Services/          # Servicios de aplicación
```

## Dependencias
- **LegalManagement.Domain**: Utiliza las entidades y reglas del dominio
- Sin dependencias de infraestructura o presentación

## Principios
- Contiene la lógica de aplicación, no la de negocio
- Orquesta el dominio pero no contiene reglas de negocio
