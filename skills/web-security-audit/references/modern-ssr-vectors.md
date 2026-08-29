# Auditoría de Vectores Avanzados: SSR, Server Actions, Prototype Pollution y SSTI

Esta guía complementa la auditoría en aplicaciones modernas desarrolladas con frameworks SSR (Next.js, Nuxt, SvelteKit, Remix), arquitecturas basadas en Node.js y motores de plantillas.

---

## 1. Next.js App Router, React Server Components (RSC) y Server Actions

### 1.1 Exposición Insegura de Server Actions (`"use server"`)
- **Riesgo**: En Next.js / React, marcar una función con la directiva `"use server"` crea automáticamente un endpoint HTTP POST expuesto públicamente. Si la función no realiza verificación explícita de autenticación y autorización en su primera línea, cualquier usuario anónimo puede invocarla directamente mediante peticiones HTTP personalizadas.
- **Patrón Vulnerable**:
  ```typescript
  // app/actions.ts
  "use server";
  export async function deleteUser(userId: string) {
    // ❌ VULNERABLE: No comprueba la sesión del usuario actual
    await db.user.delete({ where: { id: userId } });
  }
  ```
- **Auditoría SAST**:
  - Buscar todos los archivos con `"use server"`.
  - Comprobar que cada función exportada valide la sesión/rol (ej. `const session = await auth(); if (!session) throw new Error(...)`).
- **Prueba DAST**:
  - Interceptar peticiones POST de Server Actions (cabeceras `Next-Action` o `Action-Id`) y reenviarlas modificando los argumentos del cuerpo JSON o sin token de sesión.

---

### 1.2 Exfiltración de Secretos del Servidor a Componentes de Cliente
- **Riesgo**: Pasar objetos de datos complejos o variables de entorno desde un Server Component a un Client Component (`"use client"`) puede incluir accidentalmente API keys del servidor, hashes de contraseñas o datos sensibles en el payload de hidratación JSON enviado al navegador.
- **Remediación**: Usar el paquete `server-only` (`import 'server-only'`) en módulos que manejen secretos para evitar que sean importados accidentalmente en el bundle cliente.

---

## 2. Prototype Pollution (Client-side y Server-side - SSPP)

### 2.1 Server-Side Prototype Pollution (Node.js SSPP)
- **Riesgo**: Ocurre cuando una función de fusión profunda (*deep merge*), clonación o asignación de objetos permite claves como `__proto__`, `constructor` o `prototype` sin validar. Un atacante puede contaminar el prototipo global `Object.prototype`, modificando el comportamiento de todo el proceso Node.js.
- **Impactos de SSPP**:
  1. **Bypass de Autenticación / Lógica**: Inyectar propiedades como `isAdmin: true` en el prototipo global para que todos los objetos sin esa propiedad evalúen `user.isAdmin === true`.
  2. **Remote Code Execution (RCE)**: Contaminar opciones pasadas a `child_process.exec` o `child_process.fork` (ej. `NODE_OPTIONS`, `shell`, `execPath`).
- **Patrón Vulnerable SAST**:
  ```javascript
  // ❌ VULNERABLE: Fusión de objetos sin sanitizar claves
  function merge(target, source) {
    for (let key in source) {
      if (typeof source[key] === 'object') {
        if (!target[key]) target[key] = {};
        merge(target[key], source[key]); // Permite key == "__proto__"
      } else {
        target[key] = source[key];
      }
    }
  }
  ```
- **Prueba DAST (vía JSON payload)**:
  ```json
  POST /api/user/profile
  Content-Type: application/json

  {
    "name": "Alice",
    "__proto__": {
      "isAdmin": true
    }
  }
  ```
- **Remediación**: Usar `Object.create(null)` para objetos mapa, utilizar `Map` nativo, aplicar `Object.freeze(Object.prototype)` o usar librerías actualizadas (ej. `lodash` >= 4.17.21).

---

## 3. Server-Side Template Injection (SSTI)

- **Riesgo**: Cuando la aplicación concatena datos de entrada del usuario directamente dentro de la plantilla que procesa un motor como Jinja2, EJS, Nunjucks, Pug, Handlebars o Thymeleaf, el motor evalúa la entrada como código.
- **Patrón Vulnerable SAST (EJS / Nunjucks)**:
  ```javascript
  // ❌ VULNERABLE: El usuario controla el string de la plantilla
  const ejs = require('ejs');
  app.get('/hello', (req, res) => {
    const html = ejs.render(`<h1>Hola ${req.query.name}</h1>`); // SSTI
    res.send(html);
  });
  ```
- **Pruebas DAST (Payloads de prueba por motor)**:
  * **Jinja2 / Twig / Nunjucks**: `{{7*7}}` -> si responde `49`, hay SSTI.
  * **Ejecución de código en Jinja2 (Python)**: `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}`
  * **EJS (Node.js)**: `<%= process.mainModule.require('child_process').execSync('id') %>`
- **Remediación**: NUNCA pasar la cadena de la plantilla dinámicamente desde la entrada del usuario. Usar archivos de plantilla estáticos y pasar los datos como contexto de variables: `ejs.renderFile('template.ejs', { name: req.query.name })`.

---

## 4. DOM Clobbering

- **Riesgo**: Ocurre en aplicaciones JavaScript de cliente cuando el HTML inyectado (o contenido de WYSIWYG) contiene elementos HTML con atributos `id` o `name` que sobrescriben variables globales o propiedades de `window` / `document`.
- **Ejemplo**:
  Si la aplicación JS ejecuta `if (window.config.analyticsUrl) { ... }`, un atacante que inyecta HTML `<a id="config" href="analyticsUrl:https://attacker.com"></a>` hace que `window.config` apunte al elemento HTML en lugar de `undefined`, alterando la lógica JS.
- **Remediación**: Sanitizar HTML inyectado con **DOMPurify** usando la opción `SANITIZE_DOM: true` o `FORBID_ATTR: ['id', 'name']`.
