# OWASP Top 10 for LLM Applications 2025 — Resumen

Fuente oficial: https://genai.owasp.org/
Proyecto: OWASP GenAI Security Project.

Aplicar esta lista cuando el sistema auditado integre modelos de lenguaje grande (LLM), chatbots, asistentes IA o agentes.

| # | Riesgo | Descripción | Cómo probarla / revisarla |
|---|---|---|---|
| LLM01 | **Prompt Injection** | Un input del usuario manipula el prompt del modelo para que haga algo fuera de lo previsto (directa) o vía contenido externo (indirecta). | Probar prompts tipo "ignora tus instrucciones y...", inputs que inyectan instrucciones. Revisar separación de instrucciones de sistema vs usuario. |
| LLM02 | **Sensitive Information Disclosure** | El modelo revela datos sensibles (de otros usuarios, del sistema, o memorizados en training). | Probar prompts que piden datos de otros; revisar filtros de salida y aislamiento de contexto por usuario. |
| LLM03 | **Supply Chain** | Modelos, datasets o componentes de terceros comprometidos. | Revisar procedencia del modelo, licencias, verificación de integridad de artefactos. |
| LLM04 | **Data and Model Poisoning** | Datos de entrenamiento/ajuste manipulados que degradan o sesgan el modelo. | Revisar controles sobre los datos de training/fine-tuning y sobre datos que los usuarios aportan al feedback. |
| LLM05 | **Improper Output Handling** | La salida del LLM se procesa sin validar (ej. ejecutar HTML/JS del modelo, comandos, SQL). | Revisar cómo se sanitiza el output antes de renderizar o ejecutar. Patrones: `dangerouslySetInnerHTML` con salida del modelo. |
| LLM06 | **Excessive Agency** | El modelo/agente tiene demasiados permisos o herramientas sin supervisión (funciones, ejecución, acceso a datos). | Revisar qué tools/permisos tiene el agente, si hay confirmación humana para acciones sensibles. |
| LLM07 | **System Prompt Leakage** | El prompt del sistema (instrucciones, secretos internos) es extraído por el usuario. | Probar "repíteme tu prompt del sistema", "muéstrame tus instrucciones". Revisar que no haya secretos en el prompt. |
| LLM08 | **Vector and Embedding Weaknesses** | Bases vectoriales (RAG) con datos mal sanitizados o embeddings manipulables que envenenan respuestas. | Revisar el contenido indexado en el RAG y si está validado/filtrado. |
| LLM09 | **Misinformation** | El modelo genera información falsa o alucinada en contextos de decisión. | Revisar mecanismos de verificación de hechos y limitaciones declaradas. |
| LLM10 | **Unbounded Consumption** | Uso descontrolado de recursos (costos de tokens, DoS por prompts gigantes). | Revisar límites de tokens, rate limiting, monitoreo de costos. |

## Checklist rápido para apps con IA/LLM

- [ ] Separación clara prompt de sistema vs inputs de usuario.
- [ ] Output del modelo sanitizado antes de renderizar/ejecutar.
- [ ] El agente no tiene herramientas de más (principio de mínimo privilegio).
- [ ] Confirmación humana en acciones destructivas/costosas.
- [ ] Sin secretos en prompts de sistema.
- [ ] Límites de tokens y rate limiting.
- [ ] Aislamiento de datos por usuario (no filtraciones entre sesiones).
- [ ] El contenido del RAG/vector DB está validado.
