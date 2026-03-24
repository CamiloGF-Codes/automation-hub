# CONTEXTO PARA CLAUDE CODE

## Información del Proyecto
**Nombre:** AutomationHub  
**Tipo:** Plataforma SaaS multi-tenant de automatizaciones con n8n  
**Ubicación:** /Users/camilogonzalez/Proyects/automation-hub/  
**GitHub:** https://github.com/CamiloGF-Codes/automation-hub  
**Autor:** Camilo González  
**Email:** camilogonzalezfuentes1303@gmail.com  

## Stack Tecnológico
- **Backend:** FastAPI 0.109 (Python 3.11), SQLAlchemy 2.0, PostgreSQL 15, Redis 7
- **Frontend:** Next.js 14, TypeScript, Tailwind CSS, shadcn/ui
- **Automation:** n8n (Docker, multi-tenant)
- **DevOps:** Docker Compose, Kubernetes, Terraform, GitHub Actions

## Objetivo del Proyecto
1. **Corto plazo:** Conseguir trabajo en IBM/Amazon/Microsoft Costa Rica
2. **Mediano plazo:** Lanzar agencia de automatizaciones
3. **Largo plazo:** $3-5k/mes ingresos recurrentes en 6 meses

## Arquitectura
Multi-tenant: cada organización tiene su propia instancia Docker de n8n.  
Backend FastAPI gestiona provisioning, autenticación JWT, routing.  
Frontend muestra marketplace de 10 workflows + analytics en tiempo real.

## Estado Actual

### ✅ Completado
- [x] Setup del entorno (Homebrew, Python 3.11, Node, Git, Docker)
- [x] Docker Compose funcionando (PostgreSQL + Redis + n8n)
- [x] Workflow #1: Lead Generation Pipeline (webhook + email simulado)
- [x] Estructura de carpetas completa
- [x] Backend skeleton (main.py con FastAPI básico)
- [x] Repo público en GitHub: https://github.com/CamiloGF-Codes/automation-hub
- [x] Claude Code configurado con Ollama local
- [x] CLAUDE_CONTEXT.md creado para memoria persistente

### 🔄 En Progreso
- [x] Workflow #2: Social Media Scheduler
  - [x] Flujo de n8n diseñado (Schedule + Sheets + Buffer + Update)
  - [x] workflow.json exportado a /workflows/02-social-media-scheduler/
  - [x] README.md documentado con setup e instrucciones
  - [ ] Google Sheet de prueba (crear manualmente - instrucciones en README)
  - [ ] Credenciales OAuth configuradas en n8n
  - [ ] Testing end-to-end completo

### ⏳ Próximas Tareas (Roadmap 10 semanas)
**Prioridad 1 (Completar Workflow #2):**
- [ ] Crear Google Sheet de prueba con datos
- [ ] Configurar Google Sheets OAuth en n8n
- [ ] Configurar Buffer API (o webhook.site para testing)
- [ ] Testing completo y validación
- [ ] Marcar Workflow #2 como completado ✅

**Prioridad 2 (Semana 2):**
- [ ] Backend: Modelos SQLAlchemy (User, Organization, Workflow, Execution)
- [ ] Backend: Alembic setup + primera migración
- [ ] Backend: Autenticación JWT (endpoints: register, login, me)
- [ ] Workflow #3: Invoice Automation (Stripe → PDF → Email → Sheets)

**Prioridad 3 (Semana 3-4):**
- [ ] Sistema de provisioning de n8n (Docker SDK)
- [ ] Workflows 4-7
- [ ] Frontend: Setup Next.js

## Notas de la Sesión Actual
**Fecha:** 2025-03-23  
**Duración:** ~4 horas  
**Completado hoy:**
- Configuración completa del proyecto
- Workflow #1 funcional
- Workflow #2 diseñado (80% completo)
- Sistema de contexto con CLAUDE_CONTEXT.md

**Bloqueadores:**
- Sin tokens en modelo local (Ollama) - continuar mañana
- Workflow #2 necesita Google Sheet + credenciales (manual)

**Próxima sesión:**
- Terminar Workflow #2 (20-30 min)
- Empezar Backend con modelos SQLAlchemy (1-2 horas)

## Última Actualización
**Fecha:** 2025-03-23 (fin de sesión)  
**Última tarea completada:** Workflow #2 - flujo diseñado (pendiente credenciales)  
**Próxima tarea:** Completar Workflow #2 + empezar Backend  
**Estado:** Listo para commit y push - pausa hasta mañana  