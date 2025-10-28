# LegalManagement.Domain

## Descripción
Capa de dominio del sistema de gestión de expedientes legales.

## Responsabilidades
- Definir las entidades del dominio y sus reglas de negocio
- Establecer las interfaces de los repositorios
- Definir los objetos de valor (Value Objects)
- Implementar las excepciones del dominio
- Contener la lógica de negocio central independiente de infraestructura

## Estructura
```
LegalManagement.Domain/
├── Entities/          # Entidades del dominio
├── ValueObjects/      # Objetos de valor
├── Interfaces/        # Interfaces de repositorios
├── Exceptions/        # Excepciones del dominio
└── Services/          # Servicios de dominio
```

## Principios
- Esta capa NO debe tener dependencias externas
- Es el corazón de la aplicación
- Contiene la lógica de negocio pura
