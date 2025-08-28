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
- RLS en DB por tenant_id.
- JWT auth con Sanctum.
- PII masking para IA (regex).
- Logs de prompts para auditoría.

## User Stories
- Como dueño de restaurante, quiero activar un pack para gestionar pedidos vía WhatsApp.
- Como cliente, quiero reservar un turno médico desde Telegram.
- Como admin, quiero trackear uso de tokens IA para facturación.

## Roadmap
- **Fase 1 (MVP):** Tenancy, auth, billing, WhatsApp, RAG.
- **Fase 2:** Vertical packs (restaurante, salud, retail).
- **Fase 3:** Analítica, integraciones avanzadas, multi-sucursal.
