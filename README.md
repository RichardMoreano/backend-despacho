# Springboot-API-REST-DESPACHO

API REST para gestionar despachos (microservicio de despachos).

Este servicio proporciona endpoints para crear, listar, obtener, actualizar y eliminar registros de despacho.

## Contenido
- Descripción
- Requisitos
- Ejecutar localmente (Maven)
- Ejecutar con Docker
- Endpoints
- Ejemplos de uso (curl)
- Notas

## Descripción

Microservicio Spring Boot que expone una API REST en `/api/v1/despachos` para administrar recursos `Despacho`.

## Requisitos
- Java 17+ instalado
- Maven (si no usa los wrappers `mvnw`/`mvnw.cmd`)
- Docker y docker-compose (opcional)

Este proyecto usa los wrappers `mvnw` / `mvnw.cmd`, por lo que no es estrictamente necesario tener Maven globalmente instalado.

## Ejecutar localmente (Maven)

1. Abrir una terminal en la carpeta del proyecto:

   back-Despachos_SpringBoot/Springboot-API-REST-DESPACHO

2. Ejecutar con el wrapper Maven:

```bash
./mvnw spring-boot:run
```

o, si tiene Maven instalado globalmente:

```bash
mvn spring-boot:run
```

Por defecto la aplicación se expondrá en el puerto configurado en `src/main/resources/application.properties` (comúnmente 8081).

## Ejecutar con Docker

El repositorio incluye un `dockerfile` en este módulo. Para construir y ejecutar la imagen:

```bash
# Construir la imagen (desde la carpeta del proyecto que contiene el Dockerfile)
docker build -t despachos-api:latest .

# Ejecutar el contenedor (mapea el puerto 8081)
docker run --rm -p 8081:8081 despachos-api:latest
```

Si usa `docker-compose` en la raíz del workspace, puede que ya exista un servicio configurado; revise `docker-compose.yml` en la raíz.

## Entidad Despacho (modelo)

Campos principales del recurso `Despacho`:

- idDespacho: Long (generado automáticamente)
- fechaDespacho: LocalDate (ISO date, ej. 2026-05-18)
- patenteCamion: String
- intento: int
- idCompra: Long
- direccionCompra: String
- valorCompra: Long
- despachado: boolean

Ejemplo JSON de un Despacho:

```json
{
  "fechaDespacho": "2026-05-18",
  "patenteCamion": "ABC123",
  "intento": 1,
  "idCompra": 42,
  "direccionCompra": "Calle Falsa 123",
  "valorCompra": 15000,
  "despachado": false
}
```

## Endpoints

Base path: `/api/v1/despachos`

- POST /api/v1/despachos
  - Crear un nuevo despacho.
  - Request body: JSON del Despacho (sin `idDespacho`).
  - Response: 201 Created con Location header apuntando al nuevo recurso y el cuerpo con el despacho creado.

- GET /api/v1/despachos
  - Obtener la lista de todos los despachos.
  - Response: 200 OK con un array de objetos Despacho.

- GET /api/v1/despachos/{idDespacho}
  - Obtener un despacho por su id.
  - Response: 200 OK con el objeto Despacho, o 404 si no existe.

- PUT /api/v1/despachos/{idDespacho}
  - Actualizar un despacho existente.
  - Request body: JSON del Despacho con los nuevos valores.
  - Response: 200 OK con el despacho actualizado, o 404 si no existe.

- DELETE /api/v1/despachos/{idDespacho}
  - Eliminar un despacho por su id.
  - Response: 204 No Content si la eliminación fue exitosa, o 404 si no existe.

## Ejemplos (curl)

# Crear un despacho
curl -X POST http://localhost:8081/api/v1/despachos \
  -H "Content-Type: application/json" \
  -d '{"fechaDespacho":"2026-05-18","patenteCamion":"ABC123","intento":1,"idCompra":42,"direccionCompra":"Calle Falsa 123","valorCompra":15000,"despachado":false}'

# Listar despachos
curl http://localhost:8081/api/v1/despachos

# Obtener por id
curl http://localhost:8081/api/v1/despachos/1

# Actualizar despacho
curl -X PUT http://localhost:8081/api/v1/despachos/1 \
  -H "Content-Type: application/json" \
  -d '{"fechaDespacho":"2026-05-19","patenteCamion":"XYZ999","intento":2,"idCompra":42,"direccionCompra":"Av Siempreviva 742","valorCompra":15500,"despachado":true}'

# Eliminar despacho
curl -X DELETE http://localhost:8081/api/v1/despachos/1