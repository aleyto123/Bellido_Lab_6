# Guion para video de demostración - SecureDocs

**Estudiante:** Bellido Rony  
**Curso:** Desarrollo de Soluciones en la Nube

## Duración recomendada

Entre 4 y 6 minutos.

## 1. Preparar el sistema

Abrir dos terminales.

Terminal 1:

```powershell
cd backend
npm install
npm run seed
npm run dev
```

Terminal 2:

```powershell
cd frontend
npm install
npm run dev
```

Abrir en el navegador:

```text
http://localhost:5173
```

Si el frontend está configurado para otro puerto, abrir el puerto mostrado por Vite.

## 2. Presentar el objetivo

Decir en el video:

> SecureDocs es un sistema de gestión documental que combina RBAC y ABAC. RBAC valida los permisos del rol y ABAC valida las condiciones del usuario, el documento y el entorno.

## 3. Login rápido

1. Mostrar la pantalla de inicio de sesión.
2. Presionar **Administrador** en accesos de demostración.
3. Mostrar el dashboard.
4. Señalar el nombre, rol y nivel de seguridad del usuario.
5. Mostrar que aparecen las opciones **Usuarios** y **Auditoría** porque el Administrador tiene esos permisos RBAC.

## 4. Mostrar documentos permitidos

1. Entrar a **Documentos**.
2. Mostrar que la lista contiene únicamente documentos permitidos por ABAC.
3. Abrir el documento **Presupuesto Corporativo** o **Proyecto Reserva**.
4. Abrir el modal **Policy Engine Decoder**.
5. Mostrar las políticas aprobadas.

## 5. Demostrar denegación ABAC

En la barra **Simulador ABAC**:

1. Seleccionar dispositivo **Personal**.
2. Mantener país **Perú**.
3. Mantener la hora `10:00`.
4. Intentar consultar un documento de nivel 4 o 5.
5. Mostrar que el acceso es denegado porque requiere dispositivo corporativo.

Después:

1. Seleccionar dispositivo **Corporativo**.
2. Cambiar la hora a `19:00`.
3. Intentar consultar nuevamente el documento confidencial.
4. Mostrar que el acceso es denegado por horario.

Decir:

> El rol puede tener permiso RBAC, pero ABAC bloquea la operación porque el contexto no cumple las políticas.

## 6. Demostrar denegación RBAC

1. Cerrar sesión.
2. Iniciar sesión como **Empleado**.
3. Entrar a **Documentos**.
4. Intentar aprobar un documento pendiente.
5. Mostrar la respuesta `403 Acceso denegado por RBAC`.

Decir:

> El Empleado puede consultar y modificar documentos, pero no tiene el permiso RBAC para aprobarlos.

## 7. Demostrar invitado

1. Cerrar sesión.
2. Iniciar sesión como **Invitado**.
3. Consultar **Comunicado Público**.
4. Mostrar que el acceso es permitido.
5. Intentar consultar un documento confidencial.
6. Mostrar que el acceso es denegado.

Decir:

> El Invitado solo puede acceder a documentos públicos, de nivel bajo y con estado publicado.

## 8. Mostrar auditoría

1. Iniciar sesión como Administrador.
2. Entrar a **Auditoría**.
3. Mostrar registros `PERMITIDO` y `DENEGADO`.
4. Mostrar las columnas de usuario, recurso, acción, fecha, resultado y motivo.

Decir:

> Cada intento de acceso queda registrado para garantizar trazabilidad.

## 9. Mostrar Test Suite

1. Entrar a **Test suite**.
2. Mostrar los 12 escenarios obligatorios.
3. Explicar que las evidencias principales se ejecutan en Postman y que esta vista resume la matriz de pruebas del laboratorio.

## 10. Cierre del video

Decir:

> SecureDocs implementa autenticación JWT, autorización RBAC, evaluación ABAC, gestión documental y auditoría. La decisión final solo permite una operación cuando RBAC y ABAC son permitidos.

## Evidencias que deben verse en la grabación

- Backend iniciado correctamente.
- Login exitoso.
- Dashboard.
- Documentos permitidos.
- Política bloqueada por dispositivo u horario.
- Operación denegada por RBAC.
- Acceso permitido y denegado para Invitado.
- Registro de auditoría.
- Test Suite con los 12 escenarios.
