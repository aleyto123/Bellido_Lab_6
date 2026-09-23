# Diagrama de Arquitectura - SecureDocs

**Estudiante:** Bellido Rony  
**Curso:** Desarrollo de Soluciones en la Nube

## Arquitectura general

```mermaid
flowchart LR
    U[Usuario] --> F[Frontend React + Vite]
    F -->|Axios + JWT + cabeceras ABAC| API[API REST Express]

    API --> AUTH[Autenticación JWT]
    AUTH -->|Usuario autenticado| AUTHZ[Autorización híbrida]

    AUTHZ --> RBAC[Middleware RBAC]
    RBAC -->|Rol + permiso| ABAC[Motor ABAC]
    ABAC -->|Usuario + recurso + acción + entorno| POL[Políticas centralizadas]

    API --> DOC[Servicio de documentos]
    API --> USERS[Servicio de usuarios]
    API --> AUDIT[Servicio de auditoría]

    DOC --> DB[(SQLite)]
    USERS --> DB
    AUDIT --> DB
    POL --> DB

    ENV[Entorno de petición\nPaís · hora · IP · dispositivo] --> ABAC
```

## Flujo de autorización

```mermaid
sequenceDiagram
    participant U as Usuario
    participant F as Frontend
    participant A as API REST
    participant JWT as Autenticación JWT
    participant R as RBAC
    participant B as ABAC
    participant D as Documento
    participant L as Auditoría

    U->>F: Solicita una operación
    F->>A: Petición con Bearer Token
    A->>JWT: Verifica token y estado ACTIVO
    JWT-->>A: Usuario autenticado
    A->>R: Comprueba permiso del rol

    alt RBAC denegado
        R-->>A: DENEGAR
        A->>L: Registra rechazo RBAC
        A-->>F: HTTP 403
    else RBAC permitido
        R->>B: Evalúa usuario, recurso, acción y entorno
        alt ABAC denegado
            B-->>A: DENEGAR + motivo
            A->>L: Registra rechazo ABAC
            A-->>F: HTTP 403
        else ABAC permitido
            B-->>A: PERMITIR
            A->>D: Ejecuta operación
            D-->>A: Resultado
            A->>L: Registra acceso permitido
            A-->>F: Respuesta HTTP
        end
    end
```

## Componentes

| Componente | Responsabilidad |
|---|---|
| Frontend React | Login, dashboard, documentos, usuarios, auditoría y simulador ABAC. |
| Axios | Inyecta JWT y las cabeceras `x-country`, `x-device`, `x-time` y `x-forwarded-for`. |
| Autenticación JWT | Identifica al usuario y valida que su cuenta esté activa. |
| RBAC | Comprueba los permisos asignados al rol: leer, crear, modificar, eliminar, aprobar y auditar. |
| Motor ABAC | Evalúa las políticas centralizadas de departamento, nivel, propiedad, horario, país, dispositivo, estado e invitados. |
| Servicio de documentos | Ejecuta las operaciones CRUD y aprobación de documentos. |
| Servicio de usuarios | Administra usuarios, roles, departamentos, nivel de seguridad y estado. |
| Auditoría | Registra usuario, recurso, acción, fecha, resultado y motivo. |
| SQLite | Persistencia local de usuarios, roles, permisos, documentos y auditoría. |

## Ubicación en el código

- Frontend: `frontend/src`
- API y rutas: `backend/src/routes`
- Autenticación: `backend/src/middlewares/auth.middleware.ts`
- RBAC: `backend/src/middlewares/rbac.middleware.ts`
- ABAC: `backend/src/middlewares/abac.middleware.ts` y `backend/src/policies`
- Auditoría: `backend/src/middlewares/audit.middleware.ts`
- Base de datos: `backend/database.sqlite` y `backend/schema.sql`
