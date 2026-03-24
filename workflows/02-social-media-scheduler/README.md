# Social Media Scheduler Workflow

> Workflow #2 de AutomationHub - Publicación automatizada en redes sociales

## 📋 Descripción

Workflow de n8n que ejecuta diariamente a las 9am (zona horaria Costa Rica) para:
1. Leer posts programados desde Google Sheets
2. Filtrar posts pendientes para hoy
3. Publicar en Buffer API
4. Actualizar status en Google Sheets

## 🔄 Flujo del Workflow

```
┌─────────────────────┐
│  Daily Trigger 9AM  │
│  (Cron: 0 9 * * *)  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Read Scheduled Posts│
│   (Google Sheets)   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Filter Today's Posts │
│  date=today, status  │
│      =pending        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Prepare Buffer       │
│      Payload         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Publish to Buffer   │
│      (HTTP POST)     │
└──────────┬──────────┘
           │
     ┌─────┴─────┐
     │           │
     ▼           ▼
┌─────────┐  ┌─────────┐
│ Success │  │  Error  │
│         │  │         │
▼         ▼  ▼         ▼
┌─────────────┐ ┌─────────────┐
│Mark Published│ │ Mark Failed │
│(Sheets)      │ │ (Sheets)    │
└─────────────┘ └─────────────┘
```

## 📊 Estructura de Google Sheets

### Hoja: "Posts"

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `id` | string | ID único del post | `post_001` |
| `platform` | string | Plataforma destino | `twitter`, `linkedin`, `instagram`, `facebook` |
| `content` | string | Texto del post | `¡Nuevo artículo en el blog! 🚀` |
| `media_url` | string | URL de imagen/video (opcional) | `https://example.com/image.jpg` |
| `scheduled_date` | date | Fecha programada | `2024-01-15` |
| `status` | string | Estado del post | `pending`, `published`, `failed` |
| `published_at` | datetime | Fecha de publicación real | `2024-01-15T09:00:00Z` |
| `error_message` | string | Mensaje de error si falló | `API rate limit exceeded` |

### Ejemplo de datos

```csv
id,platform,content,media_url,scheduled_date,status,published_at,error_message
post_001,twitter,"¡Nuevo artículo en el blog! 🚀 https://automation.hub/blog",,2024-01-15,pending,,
post_002,linkedin,"Nuestra guía completa de automatización ya disponible",https://example.com/cover.jpg,2024-01-15,pending,,
post_003,instagram,"Descubre cómo ahorrar 10 horas semanales 📸",https://example.com/ig.jpg,2024-01-16,pending,,
```

## 🔐 Variables de Entorno Requeridas

Configurar en n8n:

```env
# Google Sheets
GOOGLE_SHEETS_ID=1BxiMVs0XRA5nFMdKvBdBZjgmUUqptbps74NYv

# Buffer API - IDs de perfil por plataforma
BUFFER_TWITTER_ID=5d7b3a1e2f4c8b9a3d2e1f0a
BUFFER_LINKEDIN_ID=5d7b3a1e2f4c8b9a3d2e1f0b
BUFFER_INSTAGRAM_ID=5d7b3a1e2f4c8b9a3d2e1f0c
BUFFER_FACEBOOK_ID=5d7b3a1e2f4c8b9a3d2e1f0d
```

## 🔑 Credenciales Necesarias

### 1. Google Sheets OAuth2

1. Crear proyecto en [Google Cloud Console](https://console.cloud.google.com)
2. Habilitar Google Sheets API
3. Crear credenciales OAuth2
4. Configurar en n8n:
   - **Credential Type:** Google Sheets OAuth2 API
   - **Credential Name:** `google-sheets-credentials`

### 2. Buffer API Token

1. Obtener token desde [Buffer Developer Portal](https://buffer.com/developers/api)
2. Configurar en n8n:
   - **Credential Type:** HTTP Header Auth
   - **Header Name:** `Authorization`
   - **Header Value:** `Bearer YOUR_BUFFER_TOKEN`
   - **Credential Name:** `buffer-api-token`

## 🚀 Instalación

### Paso 1: Importar Workflow

```bash
# En n8n UI:
# 1. Ir a Workflows → Import from File
# 2. Seleccionar: workflow.json
```

### Paso 2: Configurar Credenciales

1. Ir a **Settings → Credentials**
2. Agregar credenciales de Google Sheets OAuth2
3. Agregar credenciales de Buffer API

### Paso 3: Configurar Variables de Entorno

En `docker-compose.yml` o `.env` de n8n:

```yaml
n8n:
  environment:
    - GOOGLE_SHEETS_ID=tu_sheet_id
    - BUFFER_TWITTER_ID=tu_twitter_profile_id
    - BUFFER_LINKEDIN_ID=tu_linkedin_profile_id
    - BUFFER_INSTAGRAM_ID=tu_instagram_profile_id
    - BUFFER_FACEBOOK_ID=tu_facebook_profile_id
```

### Paso 4: Crear Google Sheet

1. Crear nuevo Google Sheet
2. Crear hoja llamada "Posts"
3. Agregar columnas según estructura definida
4. Copiar ID del Sheet (de la URL: `/d/SHEET_ID/edit`)

### Paso 5: Activar Workflow

```bash
# En n8n UI:
# 1. Abrir workflow importado
# 2. Click en "Active" toggle (esquina superior derecha)
# 3. El workflow ejecutará diariamente a las 9am
```

## 📝 Uso Manual (Testing)

Para probar sin esperar al cron:

1. Abrir workflow en n8n
2. Click en "Execute Workflow"
3. Verificar nodos individuales
4. Revisar logs en "Executions"

## ⚠️ Troubleshooting

### Error: "Google Sheets credentials not found"

```
Solución: Configurar credenciales OAuth2 en n8n
```

### Error: "Buffer API rate limit exceeded"

```
Solución: El workflow marca automáticamente como "failed"
         Reintentar manualmente después del límite
```

### Error: "No posts found for today"

```
Solución: Verificar que scheduled_date tenga formato YYYY-MM-DD
         Verificar que status sea "pending"
```

## 🔧 Personalización

### Cambiar Hora de Ejecución

En nodo "Daily Trigger 9AM", modificar cron:

```javascript
// Actual: 9:00 AM
"0 9 * * *"

// Cambiar a: 8:30 AM
"30 8 * * *"

// Cambiar a: cada 6 horas
"0 */6 * * *"
```

### Agregar Nueva Plataforma

1. Agregar variable de entorno: `BUFFER_TIKTOK_ID`
2. En nodo "Prepare Buffer Payload", agregar mapping:

```javascript
const platformMap = {
  // ... existentes
  'tiktok': 'tiktok'
};
```

### Modificar Zona Horaria

En `docker-compose.yml`:

```yaml
n8n:
  environment:
    - GENERIC_TIMEZONE=America/New_York  # Cambiar según necesidad
```

## 📈 Métricas y Monitoreo

El workflow genera logs con:

```json
{
  "timestamp": "2024-01-15T09:00:00Z",
  "post_id": "post_001",
  "platform": "twitter",
  "status": "published",
  "buffer_response": { ... }
}
```

Para monitoreo avanzado, considerar integrar con:
- **n8n External Logging:** Elasticsearch, Splunk
- **Webhook Notifications:** Slack, Discord

## 🔗 Workflow Relacionado

- **Workflow #1:** Lead Generation Pipeline
- **Workflow #3:** (Próximamente) Customer Onboarding

## 📄 Licencia

Parte de AutomationHub - Plataforma multi-tenant de automatizaciones.