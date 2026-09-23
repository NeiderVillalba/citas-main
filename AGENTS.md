# AGENTS.md — Orquestación del workspace de citas

## Alcance

Este workspace coordina dos repositorios Git independientes:

- `citas-api`: backend Java 21, Spring Boot, Maven, arquitectura hexagonal, MySQL/Flyway.
- `citas-web`: frontend TypeScript React o Angular, que consume `citas-api` directamente por REST.

La carpeta raíz no es un tercer repositorio. No inicializar Git ni crear historial Git en ella.

## Inicio de cada tarea

1. Leer `README.md`, `PRD.md`, `RESTRICCIONES_TECNICAS.md` y `database/REQUISITOS_NORMALIZACION_3FN.md`.
2. Leer los `README.md` y, cuando existan, los `AGENTS.md` del repositorio afectado.
3. Para consultas de contexto, leer primero `citas-api/docs/wiki/llm-wiki/wiki/index.md`.
4. Clasificar el alcance: backend, frontend o cross-repo.
5. Si habrá cambios cross-repo o de contrato REST, presentar antes un plan con repositorios, archivos, compatibilidad y evidencia requerida.

## Límites de responsabilidad

- Lógica de dominio, seguridad, persistencia, migraciones, REST y automatizaciones: `citas-api`.
- Interfaz, experiencia visual, estado cliente e integración REST: `citas-web`.
- No crear Express, BFF ni otra capa intermedia.
- No inventar requisitos fuera del PRD, restricciones técnicas o historias aprobadas.
- Una historia de usuario aprobada es la unidad primaria de alcance y Definition of Done.

## Git

- `main` es estable; todo trabajo se realiza en `develop`.
- Mantener los repositorios separados: no mover código ni crear enlaces de Git entre ellos.
- No reescribir historial para ocultar progreso.
- Todo cambio debe ser trazable y verificado de forma proporcional a su riesgo.

## Contratos REST

Todo cambio REST requiere:

1. documentación del contrato en la LLM Wiki;
2. implementación y pruebas relevantes en `citas-api`;
3. adaptación, typecheck/build y prueba aplicable en `citas-web`;
4. evidencia de compatibilidad o una migración explícita.

## Datos y seguridad

- Usar exclusivamente datos sintéticos, salvo información pública incluida explícitamente en los requisitos.
- Nunca leer, mostrar, copiar ni versionar secretos de `.env`.
- Mantener contraseñas, tokens, credenciales y PII fuera de documentación, logs y ejemplos.
- Cada repositorio conserva solamente `.env.example` sin valores reales.

## LLM Wiki global

La única Wiki vive en `citas-api/docs/wiki/llm-wiki/`.

- `raw/`: fuentes curadas e inmutables; no editar una fuente ya ingresada.
- `schema/`: convenciones, plantillas y procedimiento de ingestión/lint.
- `wiki/`: conocimiento sintetizado y enlazado.
- Leer `wiki/index.md` antes de consultar o actualizar páginas.
- Actualizar `wiki/index.md` ante cambios estructurales.
- Añadir en `wiki/log.md` un registro append-only de cada INGEST, QUERY, LEARN o LINT.
- Persistir solo conocimiento durable, clasificado como HECHO, DECISIÓN, PREFERENCIA o PREGUNTA ABIERTA.
- Separar evidencia comprobada de inferencias.

## Skills y automatizaciones

- `scrum-spec-orchestrator` solo escribe en `citas-api/docs/wiki/scrum/` y no implementa código.
- `stitch-design-to-frontend` gobierna Stitch → aprobación → Google AI Studio → reconciliación visual; no define ni inventa backend.
- Los workflows n8n se versionan exclusivamente como JSON en `citas-api/automations/n8n/`.

## Verificación

- Backend: pruebas de dominio, aplicación e integración relevantes.
- Frontend: build, typecheck y pruebas aplicables al framework elegido.
- Cross-repo: validar el contrato REST en los flujos afectados.
- Si falta una decisión que altere datos, seguridad, contrato o UX, registrarla como pregunta abierta y solicitar definición antes de implementarla.
