# API REST de Gestión de Películas

Práctica realizada individualmente durante 2º de Desarrollo de Aplicaciones Multiplataforma (DAM), en la que he desarrollado una API REST con Java y Spring Boot para gestionar un catálogo de películas.

La aplicación utiliza JDBC para el acceso a datos y SQLite como sistema de persistencia. Está organizada en diferentes capas para separar las responsabilidades de cada parte de la aplicación.

## Tecnologías utilizadas

- Java 17+
- Spring Boot 3.5
- Spring Framework
- JDBC
- SQLite
- Maven
- Swagger / OpenAPI
- Jakarta Validation

## Funcionalidades

La API permite realizar las siguientes operaciones sobre el catálogo de películas:

| Método HTTP | Endpoint | Descripción |
|---|---|---|
| GET | `/api/v1/jdbc/peliculas` | Obtener todas las películas. |
| GET | `/api/v1/jdbc/peliculas/{codigo}` | Obtener una película por su código. |
| POST | `/api/v1/jdbc/peliculas` | Registrar una nueva película. |
| PUT | `/api/v1/jdbc/peliculas/{codigo}` | Actualizar una película existente. |
| DELETE | `/api/v1/jdbc/peliculas/{codigo}` | Eliminar una película. |

## Validación de datos

Se utilizan mecanismos de validación tanto en la aplicación como en la base de datos:

- Jakarta Validation para comprobar los datos recibidos en las peticiones.
- Restricciones SQL para garantizar la integridad de los datos almacenados.
- Comprobación del formato de los códigos de película y de los rangos permitidos para los campos numéricos.
- Uso de `PreparedStatement` para ejecutar consultas parametrizadas.

## Arquitectura

La aplicación sigue una arquitectura por capas, separando las responsabilidades de cada componente:

| Capa | Responsabilidad |
|---|---|
| Controller | Gestiona las peticiones HTTP y construye las respuestas de la API. |
| Service | Contiene la lógica de aplicación y coordina las operaciones. |
| Repository | Gestiona las operaciones de persistencia mediante JDBC. |
| Model | Contiene los modelos de dominio utilizados por la aplicación. |
| Mapper | Transforma los datos entre las distintas capas. |

### Flujo de una petición

Por ejemplo, al registrar una película:

1. El Controller recibe la petición HTTP y valida los datos de entrada.
2. Un mapper transforma los datos de la petición en un modelo para el Service.
3. El Service gestiona la operación y delega la persistencia al Repository.
4. El Repository ejecuta la consulta SQL mediante JDBC.
5. Los mappers transforman los datos entre las entidades de persistencia y los modelos de dominio.
6. El Controller construye la respuesta HTTP correspondiente.

## Acceso a datos

La aplicación utiliza JDBC tradicional para interactuar con SQLite mediante las siguientes clases:

- `Connection`: establece la conexión con la base de datos.
- `PreparedStatement`: prepara y ejecuta las consultas SQL.
- `ResultSet`: permite recorrer los resultados de las consultas.

Se utilizan operaciones SQL para insertar, consultar, actualizar y eliminar registros. También se contempla el uso de `INSERT ... ON CONFLICT DO NOTHING` para evitar la inserción de películas con códigos duplicados.

## Configuración y ejecución

### Requisitos

- Java 17 o superior.
- Maven.
- SQLite.

### Configuración

El perfil `jdbc` debe estar activo en `src/main/resources/application.properties`:

```properties
spring.profiles.active=jdbc
```

La base de datos SQLite debe estar disponible en la ruta configurada en el repositorio:

```text
data/peliculas.sqlite3
```

### Ejecución

Desde el directorio raíz de la aplicación:

```bash
mvn spring-boot:run
```

## Documentación de la API

La aplicación incorpora anotaciones de Swagger / OpenAPI para documentar las operaciones disponibles, sus parámetros y las posibles respuestas HTTP.

Entre los códigos de respuesta utilizados se encuentran:

- `200 OK`: operación realizada correctamente.
- `201 Created`: recurso creado correctamente.
- `204 No Content`: recurso eliminado correctamente.
- `400 Bad Request`: petición incorrecta.
- `404 Not Found`: recurso no encontrado.

---

Práctica desarrollada individualmente como parte del segundo curso del ciclo formativo de Desarrollo de Aplicaciones Multiplataforma (DAM).
