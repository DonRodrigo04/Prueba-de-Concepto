# Servicio de Gestión de Productos

## Descripción

Este proyecto implementa un servicio de gestión de productos utilizando arquitectura hexagonal (ports and adapters) y el patrón CQRS (Command Query Responsibility Segregation). Está desarrollado con Spring Boot y sigue los principios de Domain-Driven Design (DDD).

## Arquitectura

El proyecto sigue una arquitectura hexagonal con tres capas principales:

- **Dominio**: Contiene la lógica de negocio, entidades y reglas.
- **Aplicación**: Orquesta los casos de uso del sistema.
- **Infraestructura**: Implementa las interfaces del dominio y proporciona adaptadores para recursos externos.

### Patrones de Diseño Implementados

- **Arquitectura Hexagonal**: Separación clara entre dominio, aplicación e infraestructura.
- **CQRS**: Separación entre operaciones de lectura (queries) y escritura (commands).
- **Repository**: Abstracción de la persistencia de datos.
- **Mediator**: Implementado mediante los buses de comandos y consultas.
- **Event Sourcing**: Captura de cambios relevantes como eventos de dominio.

## Requisitos Previos

- Java 21 o superior
- Maven 3.8+
- H2 (incluido para desarrollo)
- IntelliJ IDEA (recomendado) o cualquier otro IDE con soporte para Java/Spring

### 4. Base de Datos

Por defecto, la aplicación usa H2 (base de datos en memoria) para desarrollo.

Para acceder a la consola H2:
- URL: http://localhost:8080/h2-console
- JDBC URL: jdbc:h2:mem:productdb
- Usuario: sa
- Contraseña: password
- 
## Estructura del Proyecto

```
com.company.productservice/
├── domain/
│   ├── model/
│   │   └── Product.java
│   └── repository/
│       └── ProductRepository.java
├── application/
│   ├── common/
│   │   ├── Command.java
│   │   ├── Query.java
│   │   ├── handlers/
│   │   └── bus/
│   ├── events/
│   │   ├── DomainEvent.java
│   │   ├── ProductCreatedEvent.java
│   │   └── ProductStockUpdatedEvent.java
│   ├── command/
│   │   ├── models/
│   │   │   ├── CreateProductCommand.java
│   │   │   └── UpdateProductStockCommand.java
│   │   └── handlers/
│   └── query/
│       ├── models/
│       │   ├── ProductDto.java
│       │   └── GetProductByIdQuery.java
│       └── handlers/
└── infrastructure/
    ├── persistence/
    │   ├── JpaProductRepository.java
    │   └── JpaProductRepositoryAdapter.java
    ├── events/
    │   └── SpringDomainEventPublisher.java
    ├── api/
    │   └── rest/
    │       └── ProductController.java
    └── config/
        ├── ApplicationConfig.java
        └── DataInitializer.java
```

## API REST

### Endpoints Disponibles

| Método HTTP | Ruta | Descripción |
|-------------|------|-------------|
| POST | /api/products | Crear nuevo producto |
| GET | /api/products/{id} | Obtener producto por ID |
| GET | /api/products | Listar productos con filtros opcionales |
| PATCH | /api/products/{id}/price | Actualizar precio de un producto |
| PATCH | /api/products/{id}/stock | Actualizar stock de un producto |
| DELETE | /api/products/{id} | Eliminar un producto |

### Documentación de la API

La documentación completa de la API está disponible en:
- Swagger UI: http://localhost:8080/api/swagger-ui.html
- OpenAPI JSON: http://localhost:8080/api/api-docs

## Ejemplos de Uso

### Crear un Producto

```bash
curl -X POST http://localhost:8080/api/products \
     -H "Content-Type: application/json" \
     -d '{
           "name": "Smartphone XYZ",
           "description": "Un smartphone de última generación",
           "sku": "PROD-001",
           "price": 699.99,
           "stockQuantity": 100,
           "category": "Electrónica"
         }'
```

### Buscar Productos por Categoría

```bash
curl -X GET "http://localhost:8080/api/products?category=Electrónica"
```

## Ejecución de Pruebas

```bash
# Ejecutar todas las pruebas
mvn test

# Ejecutar solo pruebas unitarias
mvn test -Dtest=*Test

# Ejecutar solo pruebas de integración
mvn verify -DskipUnitTests
```

## CI/CD

El proyecto incluye configuración para GitHub Actions que automatiza:
- Compilación y pruebas
- Análisis de código con SonarCloud
- Construcción y publicación de imagen Docker
- Despliegue en entornos de desarrollo y producción

## Contribuciones

1. Haz un fork del repositorio
2. Crea una rama para tu característica (`git checkout -b feature/amazing-feature`)
3. Realiza tus cambios y haz commit (`git commit -m 'Add some amazing feature'`)
4. Empuja la rama (`git push origin feature/amazing-feature`)
5. Abre un Pull Request

## Licencia

Este proyecto está licenciado bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

## Contacto

Equipo de Desarrollo - desarrollo@example.com

Link del Proyecto: [https://github.com/tu-usuario/product-service](https://github.com/tu-usuario/product-service)
