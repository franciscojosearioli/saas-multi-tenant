# SaaS Multi-Tenant Multi-Vertical

Plataforma SaaS para PyMEs en múltiples rubros (restaurantes, retail, salud, servicios) con arquitectura multi-tenant, packs de verticales modulares, y soporte multicanal (WhatsApp, Web, etc.). Usa IA para conversaciones naturales y analítica para métricas de negocio.

## Arquitectura
- **Multi-tenant:** Aislamiento vía RLS en PostgreSQL; cada tenant tiene datos y configs propias.
- **Multi-vertical:** Packs modulares (restaurante, salud, etc.) con intents, flujos, y UI dinámica.
- **Multicanal:** WhatsApp (Meta API), Web widget, Messenger, Telegram.
- **Core SaaS:** Auth (JWT), billing (suscripciones), orquestador conversacional, analítica.
- **IA:** NLU (FastAPI + LLM), RAG (pgvector), reglas (JSON Logic).

## Stack
- **Backend:** Laravel (API, Sanctum, Horizon).
- **Workers IA:** Python (FastAPI, OpenAI, embeddings).
- **Frontend:** Next.js (panel multi-tenant).
- **DB:** PostgreSQL (JSONB, pgvector).
- **Infra:** Docker/Compose (dev), Kubernetes (prod), S3, ELK, Prometheus.

## Modelo de Datos
- **Core:** tenants, users, channels, verticals, tenant_verticals.
- **Catálogo:** items, item_variants, prices, availability_slots.
- **Operaciones:** orders, appointments, leads, tickets.
- **Conversaciones:** conversations, messages, intents, entities_extracted.
- **KB (RAG):** kb_documents, kb_chunks, kb_vectors.
- **Billing:** plans, subscriptions, usage_metrics.

## Vertical Packs
- **Restaurante:** Intents (pedido, reserva), entidades (plato, mesa), UI (menú).
- **Salud:** Intents (turnos, reprogramar), entidades (médico, horario).
- **Retail:** Intents (búsqueda, checkout), entidades (producto, stock).
- **Servicios:** Intents (agenda, paquete), entidades (profesional).

## Integraciones
- **WhatsApp:** Meta Cloud API (requiere aprobación plantillas).
- **Pagos:** Mercado Pago (checkout API).
- **Calendarios:** Google Calendar (sync turnos).
- **Futuro:** CRMs, ERPs, delivery.

## Seguridad
- **Row Level Security (RLS):** Aislamiento de datos por `tenant_id` en PostgreSQL usando políticas como `CREATE POLICY tenant_isolation ON tenants USING (id = current_setting('app.tenant_id')::uuid);`.
- **Autenticación:** JWT con Laravel Sanctum para APIs, incluyendo `tenant_id` en token payload.
- **Autorización:** Roles (admin, operador, usuario final) con permisos granulares definidos en Laravel policies.
- **Cifrado:** HTTPS obligatorio (SSL/TLS) para todas las comunicaciones. Certificados via Let’s Encrypt en producción.
- **Protección PII (IA):** Masking de datos personales (nombres, DNI, emails) antes de enviar a LLMs usando regex o Presidio.
- **Trazabilidad:** Logs de prompts y respuestas de IA en tabla `ia_logs` con `tenant_id`, timestamp, y hash.
- **Gestión de secretos:** Claves en `.env` para desarrollo; Docker secrets o vault en producción.
- **Pruebas:** Escaneos con OWASP ZAP, tests de RLS, y validación de inyección SQL.

## Compliance
- **GDPR (Europa):** Consentimiento explícito para datos personales, derecho al olvido, retención configurable (ej. borrar `messages` tras 6 meses). Ver [GDPR.eu](https://gdpr.eu).
- **LGPD (Argentina/Brasil):** Protección de datos personales (DNI, CPF) con masking y notificación de brechas. Ver [LGPD](https://www.gov.br/lgpd).
- **HIPAA (futuro, salud):** Cifrado AES-256 y auditorías para datos médicos (a evaluar en Fase 3).
- **Implementación:** 
  - PII masking en workers FastAPI antes de LLM.
  - Retención configurable en DB (ej. `DELETE FROM messages WHERE created_at < now() - interval '6 months'`).
  - Logs de auditoría para trazabilidad.

## User Stories
- Como dueño de restaurante, quiero activar un pack para gestionar pedidos vía WhatsApp.
- Como cliente, quiero reservar un turno médico desde Telegram.
- Como admin, quiero trackear uso de tokens IA para facturación.

## Roadmap
- **Fase 1 (MVP):** Tenancy, auth, billing, WhatsApp, RAG.
- **Fase 2:** Vertical packs (restaurante, salud, retail).
- **Fase 3:** Analítica, integraciones avanzadas, multi-sucursal.
