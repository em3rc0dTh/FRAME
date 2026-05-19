# Ejemplo Real — Sistema de Turnos para Clínica Dental
## Prompt completo analizado y documentado como referencia

> **Este archivo es el prompt completo de referencia analizado en el framework FRAME.**  
> Usarlo como ejemplo de cómo se ve un prompt bien completado con el template maestro.  
> Ver `templates/PROMPT_TEMPLATE.md` para la versión genérica.

---

════════════════════════════════════════════════
Sistema de Turnos — Clínica Dental con WhatsApp + IA
════════════════════════════════════════════════

## SECCIÓN 01 — CONTEXTO

Construí un sistema completo de gestión de turnos para una clínica dental. El sistema tiene un agente de IA que actúa como recepcionista virtual por WhatsApp (vía Twilio) y un dashboard web para que el personal de la clínica administre todo.

**Usuarios:**
- Pacientes: agendan, cancelan y consultan turnos via WhatsApp
- Personal de la clínica: administra turnos y mensajes via Dashboard web

**Flujo principal de valor:**
1. Paciente envía mensaje por WhatsApp
2. El sistema procesa con IA (Sarah) y ejecuta la acción solicitada
3. Paciente recibe confirmación del turno agendado

---

## SECCIÓN 02 — STACK TECNOLÓGICO

```
Backend:       Node.js + Express — ecosistema maduro para APIs y webhooks
Base de datos: SQLite (better-sqlite3) — autocontenido, sin servicios externos
Frontend:      React + Vite — SPA, dashboard interactivo
Estilos:       Tailwind CSS + shadcn/ui — velocidad + componentes de calidad
WhatsApp:      Twilio (webhook) — integración estándar de la industria
IA:            OpenAI GPT-4o — mejor modelo disponible para conversación + function calling
Idioma del código y UI: Español
```

---

## SECCIÓN 03 — ESTRUCTURA DEL PROYECTO

```
clinica-dental/
├── server/
│   ├── index.js              ← Express server principal
│   ├── db.js                 ← Inicialización SQLite + esquema
│   ├── routes/
│   │   ├── webhook.js        ← Endpoint Twilio WhatsApp
│   │   ├── mensajes.js       ← API REST mensajes
│   │   ├── turnos.js         ← API REST turnos
│   │   └── configuracion.js  ← API REST config clínica
│   ├── services/
│   │   ├── openai.js         ← Lógica del agente IA
│   │   └── twilio.js         ← Formateo respuestas TwiML
│   └── utils/
│       └── fechas.js         ← Helpers de fecha en español
├── client/
│   ├── src/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   ├── pages/
│   │   │   ├── Landing.jsx
│   │   │   └── Dashboard.jsx
│   │   ├── components/
│   │   │   ├── TabMensajes.jsx
│   │   │   ├── TabTurnos.jsx
│   │   │   ├── TabConfiguracion.jsx
│   │   │   ├── CalendarioTurnos.jsx
│   │   │   └── ListaTurnos.jsx
│   │   └── lib/
│   │       └── api.js        ← Funciones fetch al backend
│   └── index.html
├── .env.example
├── package.json
└── README.md
```

---

## SECCIÓN 04 — BASE DE DATOS

[Ver prompt original completo para el SQL exacto de las 3 tablas y triggers]

### Qué hace de especial esta sección en el ejemplo:
- SQL exacto con `CREATE TABLE IF NOT EXISTS` — idempotente
- Constraints `CHECK` para estados válidos del turno
- Trigger para auto-actualizar `actualizado_en`
- Registro de seed con datos de ejemplo realistas
- Comentario en el prompt: "JSON array como string" para el campo `servicios`

---

## SECCIÓN 05 — BACKEND

### Lo que hace especial al webhook de Twilio en el ejemplo:

**Puntos de alta especificidad (correctamente detallados):**
- Formato de entrada: `application/x-www-form-urlencoded`
- Limpieza del prefijo `whatsapp:` del número
- Cantidad de historial para contexto conversacional: últimos 20 mensajes
- Formato exacto de respuesta TwiML (incluye XML completo)
- Mensaje de fallback exacto cuando falla OpenAI

**Puntos de baja especificidad (correctamente omitidos):**
- Cómo configurar el body-parser de Express (la IA lo sabe)
- Estructura de la request de Express (la IA lo sabe)

---

## SECCIÓN 06 — FRONTEND

### Lo que hace especial el frontend en el ejemplo:

**Landing Page:**
- 3 secciones definidas: Hero, Características (3 cards), Cómo Funciona
- Paleta específica: blanco, #E0F2FE, #1E40AF, acentos verdes
- Fuente específica: "Plus Jakarta Sans" o "DM Sans" (con ejemplos)
- Animaciones: "sutiles de entrada (fade in al hacer scroll)"
- No especifica qué librería de animación usar → la IA decide (bajo riesgo)

**Dashboard:**
- Estructura de 3 pestañas con shadcn/ui Tabs (librería específica)
- Polling exacto: 5 segundos, usando React Query
- Comportamiento de mensajes: colores, íconos, alineación definidos
- Badges con colores específicos por estado de turno
- Formulario de configuración con PUT a endpoint específico

---

## SECCIÓN 07 — CONTRATO API

| Método | Ruta | Descripción |
|--------|------|-------------|
| POST | /api/webhook/whatsapp | Webhook de Twilio |
| GET | /api/mensajes | Últimos 50 mensajes |
| GET | /api/turnos | Todos los turnos |
| GET | /api/turnos/:fecha | Turnos de una fecha específica |
| GET | /api/configuracion | Config de la clínica |
| PUT | /api/configuracion | Actualizar config |

---

## SECCIÓN 08 — VARIABLES DE ENTORNO

```
OPENAI_API_KEY=tu_api_key_aqui
PORT=3001
```

---

## SECCIÓN 09 — SCRIPTS

```json
{
  "dev": "concurrently \"node server/index.js\" \"cd client && npm run dev\"",
  "build": "cd client && npm run build",
  "start": "node server/index.js",
  "setup": "npm install && cd client && npm install"
}
```

---

## SECCIÓN 10 — REQUISITOS NO FUNCIONALES

El prompt especifica explícitamente:
- Try/catch en todos los endpoints
- Formato de error: `{ "error": "mensaje", "detalle": "..." }`
- Logging: mensaje recibido + llamada a OpenAI + errores con timestamp
- Validación de inputs antes de DB
- CORS para localhost en desarrollo
- Rate limiting: 30 req/min por número de teléfono
- UI en español: labels, placeholders, mensajes de error, tooltips
- Comentarios en español
- Fechas con locale español

---

## SECCIÓN 11 — CRITERIOS DE ÉXITO

✅ Proyecto creado desde cero con la estructura definida  
✅ Server arranca sin errores, DB se inicializa automáticamente  
✅ Frontend compila sin warnings  
✅ README en español con: instalación, OpenAI, Twilio, desarrollo, producción  
✅ Sin TypeScript — JavaScript puro  
✅ Todo el texto visible al usuario en español  

---

## 📊 Análisis de por qué este prompt funcionó

| Elemento | ¿Está presente? | Impacto |
|---|---|---|
| Contexto en pasado | ✅ "Construí..." | La IA implementa sin inventar |
| Stack con razones | ✅ Cada tecnología justificada | Cero sugerencias no pedidas |
| Árbol de archivos | ✅ Con responsabilidades | Arquitectura sin ambigüedad |
| SQL exacto | ✅ 3 tablas completas + triggers | DB sin errores de esquema |
| Código embebido crítico | ✅ TwiML exacto | Formato de webhook correcto |
| Tabla de API | ✅ 6 endpoints definidos | Frontend + backend alineados |
| Paleta con hex | ✅ Colores específicos | UI no genérica |
| Tipografía específica | ✅ Fuente de Google Fonts | Tipografía profesional |
| Requisitos no funcionales | ✅ 8 áreas cubiertas | Calidad de código profesional |
| Criterios de éxito | ✅ 6 criterios verificables | La IA sabe cuándo terminar |
