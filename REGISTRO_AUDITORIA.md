# Registro de auditoría - SecureDocs

## 1. Obtener token de Administrador

En Postman crear:

```text
POST http://localhost:4000/api/auth/login
Content-Type: application/json
```

Body `raw` / `JSON`:

```json
{
  "email": "admin@securedocs.com",
  "password": "123"
}
```

Copiar el valor de `token`.

## 2. Generar eventos de auditoría

Todas las peticiones protegidas deben usar:

```text
Authorization: Bearer TOKEN
x-country: PERU
x-device: CORPORATIVO
x-time: 10:00
```

Ejecutar una petición permitida:

```text
GET http://localhost:4000/api/documentos/3
```

Para las siguientes dos peticiones, primero inicia sesión como Maria y copia su token:

```text
POST http://localhost:4000/api/auth/login
Content-Type: application/json
```

Body:

```json
{
  "email": "maria.rrhh@securedocs.com",
  "password": "123"
}
```

Petición denegada por RBAC:

```text
POST http://localhost:4000/api/documentos/1/aprobar
```

Ejecutar una petición denegada por ABAC usando el token de Maria:

```text
GET http://localhost:4000/api/documentos/1
```

## 3. Consultar el registro

Usar el token de Administrador:

```text
GET http://localhost:4000/api/auditoria
Authorization: Bearer TOKEN_DEL_ADMIN
```

Resultado esperado: `200 OK` con un arreglo `logs`.

## 4. Campos que debe mostrar la evidencia

Cada evento debe incluir:

```json
{
  "usuario": "Maria RRHH",
  "recurso": "/api/documentos/1",
  "accion": "READ",
  "fecha": "2026-09-23T10:45:00.000Z",
  "resultado": "DENEGADO",
  "motivo": "El departamento del usuario no coincide con el del documento"
}
```

La captura de Postman debe mostrar el endpoint, la respuesta `200 OK` y los registros con resultados `PERMITIDO` y `DENEGADO`.
