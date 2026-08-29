# Herramientas para Auditoría en Windows Nativo

Todo lo necesario corre en Windows sin Kali ni WSL. Preferir binarios o `winget`; los binarios sueltos se descargan de GitHub Releases y se agregan al `PATH`.

## 1. HTTP — curl (integrado)

`curl.exe` viene en Windows 10/11.

```bash
# GET simple con headers visibles
curl -s -i http://localhost:3000/api/health

# Con headers personalizados
curl -s -i -H "Authorization: Bearer <token>" http://localhost:3000/api/users

# POST JSON
curl -s -i -X POST http://localhost:3000/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"alice","password":"test123"}'

# Seguir redirects y ver cookies
curl -s -i -L -c cookies.txt http://localhost:4200
curl -s -i -b cookies.txt http://localhost:3000/api/me

# Ver SOLO headers
curl -s -I http://localhost:4200
```

## 2. HTTP — PowerShell (alternativa)

```powershell
# Ver headers de seguridad
Invoke-WebRequest -Uri http://localhost:4200 -UseBasicParsing | Select-Object -ExpandProperty Headers

# Enviar POST JSON
$body = '{"username":"alice","password":"test123"}'
Invoke-RestMethod -Uri http://localhost:3000/api/login -Method Post -ContentType 'application/json' -Body $body
```

## 3. Búsqueda en código (sin instalar nada)

Usar grep/ripgrep del propio entorno de la skill para los patrones de Fase 2 (ej. `innerHTML`, `exec(`, `MD5`, `secret`, `password =`).

## 4. gitleaks — detección de secretos (recomendado)

```bash
winget install Gitleaks.Gitleaks
# escanear el historial git del proyecto
gitleaks detect -v --report-path gitleaks-report.json
```

Detecta API keys, tokens y contraseñas en el código y en commits históricos.

## 5. semgrep — SAST por patrones (recomendado)

Requiere Python (o Docker):

```bash
pip install semgrep
# escaneo básico con reglas OWASP
semgrep scan --config p/owasp-top-ten --config p/secrets .
```

Detecta patrones de código peligrosos por lenguaje. Si no está instalado, el análisis manual de patrones de Fase 2 es suficiente.

## 6. npm audit — vulnerabilidades de dependencias Node

```bash
# en la raíz del proyecto Node
npm audit
npm audit --omit=dev        # solo dependencias de producción
npm audit --json > audit.json
```

Requiere `package-lock.json` (debe estar versionado).

## 7. osv-scanner — SCA multi-lenguaje (recomendado)

```bash
# descargar binario de https://github.com/google/osv-scanner/releases (Windows)
osv-scanner -r .
```

Consulta la base de datos OSV (Google) para npm, pip, Go, Java, etc.

## 8. trivy — SCA + contenedores + config (opcional)

```bash
# descargar binario de https://github.com/aquasecurity/trivy/releases (Windows)
trivy fs --scanners vuln,secret,misconfig .
```

## 9. nuclei — escáner dinámico basado en plantillas (muy recomendado)

Nuclei permite escanear aplicaciones web activas usando plantillas actualizadas mantenidas por la comunidad (CVEs, cabeceras faltantes, misconfigurations, endpoints de administración expuestos).

```bash
# Instalación en Windows vía winget
winget install projectdiscovery.nuclei

# Escaneo básico de cabeceras y misconfigurations de un objetivo local
nuclei -u http://localhost:3000 -tags misconfig,exposure,headers

# Escaneo enfocado en tecnologías específicas (ej. Next.js, Express, Angular)
nuclei -u http://localhost:4200 -t http/technologies/

# Escaneo no destructivo de vulnerabilidades conocidas (CVEs)
nuclei -u http://localhost:3000 -severity critical,high,medium -es low,info
```

## 10. OWASP ZAP + MCP (opcional, DAST automático)

ZAP es el escáner web open-source de referencia y funciona en Windows.

1. Descargar instalador de https://www.zaproxy.org/download/
2. Abrir ZAP → **Marketplace → MCP Integration → Install** (add-on oficial, alpha).
3. **Options → MCP Integration**: anotar puerto y security key.
4. Con la app corriendo, conectar y orquestar spider + active scan.
   - Config MCP de ejemplo (añadir a `opencode.json`):
     ```json
     {
       "mcp": {
         "zap": {
           "type": "remote",
           "url": "https://localhost:8282",
           "enabled": true,
           "headers": { "Authorization": "Bearer {env:ZAP_MCP_KEY}" }
         }
       }
     }
     ```
5. Cruzar las alertas de ZAP con el análisis manual (Fase 5): ZAP da falsos positivos; siempre verificar en el código.

> El ZAP MCP Integration es alpha: usarlo como apoyo, no como veredicto final.

## Reglas de uso de herramientas

- Ninguna herramienta reemplaza la revisión manual del código.
- Los hallazgos de herramientas se verifican SIEMPRE en Fase 5.
- Si una herramienta no está instalada, continuar igual con el análisis manual y anotarlo en el reporte como "no ejecutado".
- Nunca correr escaneos agresivos contra hosts fuera de alcance.
