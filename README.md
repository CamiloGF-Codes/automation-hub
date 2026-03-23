# 🚀 AutomationHub

> Multi-tenant automation platform with embedded n8n

## 🎯 Objetivo

Plataforma SaaS para desplegar automatizaciones empresariales sin conocimientos técnicos.

## 🛠️ Stack Tecnológico

- **Backend:** FastAPI (Python 3.11)
- **Frontend:** Next.js 14 + TypeScript
- **Database:** PostgreSQL 15
- **Cache:** Redis 7
- **Automation Engine:** n8n
- **DevOps:** Docker, Kubernetes

## 🚀 Quick Start

```bash
# 1. Levantar servicios
docker-compose up -d

# 2. Verificar
docker-compose ps

# 3. Acceder a n8n
open http://localhost:5678
# User: admin / Pass: changeme123
