# Auditoría de Seguridad: GraphQL y WebSockets

Esta guía detalla los vectores de ataque y metodologías de prueba para aplicaciones web que utilizan **GraphQL** (Apollo, Relay, Hasura, NestJS GraphQL, Yoga) y **WebSockets** (Socket.io, WS, SignalR, ActionCable).

---

## 1. Auditoría de APIs GraphQL

### 1.1 Exposición de Introspección (Introspection Query)
- **Riesgo**: Si la introspección está habilitada en entorno de producción, un atacante puede obtener el esquema completo de la API, incluyendo tipos, queries, mutaciones, campos ocultos y argumentos deprecados.
- **Prueba**:
  ```json
  POST /graphql
  Content-Type: application/json

  {"query": "{ __schema { types { name fields { name type { name } } } } }"}
  ```
- **Remediación**: Deshabilitar la introspección en entornos de producción (ej. `introspection: false` en Apollo Server).

---

### 1.2 Inexistencia de Limites de Profundidad y Complejidad (Query Depth / Complexity DoS)
- **Riesgo**: GraphQL permite consultas anidadas recursivas o cíclicas. Un atacante puede enviar una consulta de 50 niveles de profundidad que agote el CPU o la memoria de la base de datos y el servidor.
- **Prueba de Profundidad (Query Depth)**:
  ```graphql
  query DeepQuery {
    user(id: 1) {
      friends {
        friends {
          friends {
            friends {
              name
            }
          }
        }
      }
    }
  }
  ```
- **Remediación**: Implementar middleware de validación de profundidad máxima de consultas (ej. `graphql-depth-limit`) y límites de complejidad de campos (ej. `graphql-validation-complexity`).

---

### 1.3 GraphQL Batching Attacks (Bypass de Rate Limiting)
- **Riesgo**: Si el motor GraphQL acepta arreglos de consultas en una sola petición HTTP POST, un atacante puede ejecutar cientos de intentos de autenticación (fuerza bruta) o consultas masivas en una sola transacción HTTP, salteándose los firewalls o middlewares de rate limiting HTTP.
- **Prueba**:
  ```json
  POST /graphql
  Content-Type: application/json

  [
    {"query": "mutation { login(username: \"admin\", password: \"pass1\") { token } }"},
    {"query": "mutation { login(username: \"admin\", password: \"pass2\") { token } }"},
    {"query": "mutation { login(username: \"admin\", password: \"pass3\") { token } }"}
  ]
  ```
- **Remediación**: Deshabilitar el batching de peticiones o aplicar rate limiting basado en el número de operaciones individuales contenidas en el cuerpo, no solo por petición HTTP.

---

### 1.4 Broken Object Level Authorization en Resolvedores Anidados (Nested Resolver IDOR)
- **Riesgo**: El resolvedor raíz (`user(id: 5)`) puede validar la sesión, pero un resolvedor secundario anidado (`orders { id total }` o `documents`) puede no validar si la orden pertenece al usuario autenticado.
- **Prueba**: Solicitar un recurso raíz propio, pero solicitar en campos anidados los IDs o relaciones de otros usuarios.
- **Remediación**: Aplicar middlewares de autorización (o Guards/Directivas de autorización) en CADA resolvedor individual, garantizando la verificación de propiedad del recurso (*Field-level / Resolver-level authorization*).

---

### 1.5 Inyección a través de Argumentos GraphQL
- **Riesgo**: Los argumentos pasados a consultas o mutaciones se interpolan directamente en consultas SQL/NoSQL o comandos del sistema dentro del resolvedor.
- **Prueba**: Probar inyección SQL o NoSQL dentro de argumentos de cadenas:
  ```graphql
  query {
    searchUsers(filter: "admin' OR '1'='1") { id email }
  }
  ```
- **Remediación**: Usar ORMs/query builders parametrizados dentro de los resolvedores y validar el formato de todos los escalares personalizados (Custom Scalars).

---

## 2. Auditoría de WebSockets

### 2.1 Cross-Site WebSocket Hijacking (CSWSH)
- **Riesgo**: El apretón de manos inicial de WebSocket (*HTTP Handshake*) incluye automáticamente las cookies del navegador. Si el servidor no valida la cabecera `Origin`, un sitio malicioso atacante puede abrir una conexión WebSocket hacia la app objetivo desde el navegador de la víctima y ejecutar acciones en su nombre.
- **Prueba**:
  - Iniciar la conexión WebSocket enviando un header `Origin: https://sitio-atacante.com`.
  - Si la conexión `101 Switching Protocols` se establece con éxito, el servidor es vulnerable a CSWSH.
- **Remediación**: Validar estrictamente la cabecera `Origin` en el servidor durante el evento de handshake HTTP.

---

### 2.2 Falta de Autenticación o Expiración en Conexiones Persistentes
- **Riesgo**:
  1. Conexiones WebSocket establecidas sin token de autenticación.
  2. Conexiones WebSocket que permanecen activas durante días sin re-validar la expiración del JWT o la invalidez de la sesión (ej. tras logout o cambio de contraseña).
- **Prueba**:
  - Establecer conexión WS sin enviar token o cookies.
  - Revocar la sesión/token en el backend y verificar si la conexión WebSocket abierta sigue procesando y respondiendo mensajes.
- **Remediación**: Exigir autenticación inicial (vía cookie segura o token en mensaje de inicio) y desconnectar proactivamente la socket cuando la sesión finaliza o el token expira.

---

### 2.3 Sanitización Insuficiente de Mensajes y Rate Limiting
- **Riesgo**: Los datos transmitidos vía WebSocket frames a menudo omiten los filtros habituales de XSS o inyección de código de los middlewares HTTP.
- **Prueba**:
  - Enviar payloads XSS (`<script>alert(1)</script>`) o inyecciones JSON en los mensajes WebSocket.
  - Enviar ráfagas masivas de mensajes (1000 msg/sec) por la socket para probar denegación de servicio a nivel de proceso Node/Python.
- **Remediación**: Sanitizar todos los datos recibidos por la socket antes de reflejarlos a otros clientes o guardarlos en DB, e implementar rate limiting por conexión de socket.
