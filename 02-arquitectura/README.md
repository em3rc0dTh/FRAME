# 02 — ARQUITECTURA
## Decisiones técnicas que la IA no debe tomar por vos

La arquitectura define **el esqueleto** del proyecto. Si la IA toma estas decisiones sin instrucciones, elige por convención, no por conveniencia de tu caso.

> **Regla:** Cada decisión arquitectónica sin especificar es una decisión que la IA toma sola. Eso genera iteraciones.

---

## A — Stack Tecnológico

Define cada capa de la aplicación. **Siempre incluí la razón** — esto evita que la IA sugiera alternativas.

### Formato a usar en el prompt:

```
Backend: [tecnología] — [razón en ≤5 palabras]
Base de datos: [tecnología] — [razón en ≤5 palabras]
Frontend: [tecnología] — [razón en ≤5 palabras]
Estilos: [tecnología] — [razón en ≤5 palabras]
Auth: [tecnología o "ninguna"] — [razón]
[Capa extra si aplica]: [tecnología] — [razón]
Idioma del código y UI: [idioma]
```

### Opciones comunes por capa:

| Capa | Opciones Comunes | Cuándo Usar Cada Una |
|---|---|---|
| Backend | Node.js/Express | APIs REST simples, webhooks, proyectos rápidos |
| Backend | Python/FastAPI | ML, procesamiento de datos, cientá |
| Backend | Next.js API routes | Cuando el frontend también es Next |
| DB | SQLite | Apps autocontenidas, sin infra externa |
| DB | PostgreSQL | Apps de producción con múltiples usuarios concurrentes |
| DB | MongoDB | Datos sin esquema fijo, documentos anidados |
| DB | Firebase | Auth + realtime + sin servidor propio |
| Frontend | React + Vite | SPAs, dashboards, apps complejas |
| Frontend | Next.js | SSR, SEO importante, apps fullstack |
| Frontend | HTML/CSS/JS vanilla | Landing pages, apps simples sin estado complejo |
| Estilos | Tailwind CSS | Velocidad de desarrollo, utilities |
| Estilos | shadcn/ui + Tailwind | Componentes pre-hechos de alta calidad |
| Estilos | CSS vanilla | Control total, sin dependencias |

### ⚠️ Decisiones que la IA puede tomar mal si no las especificás:
- Usar TypeScript en vez de JavaScript
- Usar Prisma en vez de consultas SQL directas
- Agregar Redis o servicios externos que no pediste
- Elegir una librería de UI diferente a la que querés

---

## B — Estructura de Archivos

Este es uno de los elementos más importantes del prompt.  
Define **dónde vive cada pieza de código** y cuál es su responsabilidad.

### Formato a usar en el prompt:

```
[nombre-proyecto]/
├── [carpeta-principal]/
│   ├── [archivo].js              ← [responsabilidad en una línea]
│   ├── [subcarpeta]/
│   │   ├── [archivo].js          ← [responsabilidad en una línea]
│   │   └── [archivo].js          ← [responsabilidad en una línea]
│   └── [subcarpeta]/
│       └── [archivo].js          ← [responsabilidad en una línea]
├── .env.example
├── package.json
└── README.md
```

### Convención de comentarios en el árbol:
- `←` seguido de la responsabilidad del archivo en UNA línea
- Si un archivo tiene más de una responsabilidad, está mal diseñado

### Arquitecturas de referencia:

**Fullstack Node + React (separados):**
```
proyecto/
├── server/
│   ├── index.js          ← Entry point + middleware global
│   ├── db.js             ← Init DB + schema + seed
│   ├── routes/           ← Un archivo por recurso REST
│   ├── services/         ← APIs externas + lógica compleja
│   └── utils/            ← Funciones puras reutilizables
├── client/
│   ├── src/
│   │   ├── App.jsx       ← Router principal
│   │   ├── pages/        ← Una carpeta/archivo por ruta
│   │   ├── components/   ← Componentes reutilizables
│   │   └── lib/
│   │       └── api.js    ← Todas las llamadas fetch centralizadas
│   └── index.html
├── .env.example
└── package.json
```

**Next.js Fullstack:**
```
proyecto/
├── app/
│   ├── layout.jsx        ← Layout raíz
│   ├── page.jsx          ← Home
│   ├── [ruta]/
│   │   └── page.jsx      ← Página de ruta
│   └── api/
│       └── [endpoint]/
│           └── route.js  ← API route handler
├── components/           ← Componentes reutilizables
├── lib/
│   ├── db.js             ← Init DB
│   └── utils.js          ← Helpers
└── .env.local
```

**Vanilla (sin framework):**
```
proyecto/
├── index.html            ← Entry point
├── css/
│   └── styles.css        ← Estilos globales
├── js/
│   ├── main.js           ← Lógica principal
│   ├── api.js            ← Fetch calls
│   └── ui.js             ← Manipulación del DOM
└── assets/
    └── [imágenes, íconos]
```

---

## C — Esquema de Base de Datos

Incluí el SQL exacto (o schema JSON para NoSQL) de cada tabla/colección.  
Este es uno de los **puntos de alto riesgo** — un campo mal tipado rompe todo.

### Para SQL (SQLite, PostgreSQL):

```sql
-- Incluir siempre:
-- 1. CREATE TABLE IF NOT EXISTS
-- 2. Tipos de datos explícitos
-- 3. Constraints (NOT NULL, DEFAULT, CHECK)
-- 4. Timestamps de auditoría (creado_en, actualizado_en)
-- 5. Triggers para actualizar timestamps en UPDATE

CREATE TABLE IF NOT EXISTS [nombre_tabla] (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  [campo] TEXT NOT NULL,
  [campo] TEXT DEFAULT '[valor]' CHECK([campo] IN ('[opcion1]', '[opcion2]')),
  creado_en DATETIME DEFAULT (datetime('now', 'localtime')),
  actualizado_en DATETIME DEFAULT (datetime('now', 'localtime'))
);
```

### Para NoSQL (MongoDB, Firebase):

```javascript
// Schema de ejemplo (Mongoose o descripción estructurada)
{
  _id: ObjectId,
  [campo]: String, // requerido, único
  [campo]: {
    type: String,
    enum: ['opcion1', 'opcion2'],
    default: 'opcion1'
  },
  [campo_relacionado]: ObjectId, // ref: 'OtraColeccion'
  creadoEn: Date,          // auto
  actualizadoEn: Date      // auto con middleware
}
```

### Qué definir por cada entidad:
- [ ] Nombre de la tabla/colección
- [ ] Todos los campos con sus tipos
- [ ] Restricciones (NOT NULL, único, check)
- [ ] Valores por defecto
- [ ] Relaciones con otras tablas
- [ ] Datos de seed/ejemplo iniciales
- [ ] Triggers o middleware de actualización

---

## D — Contrato de API

Una tabla de endpoints es **obligatoria** en proyectos fullstack.  
Sin este contrato, hay un 80% de probabilidad de que frontend y backend no coincidan.

### Formato mínimo:

```
| Método | Ruta | Body/Params | Respuesta | Descripción |
|--------|------|-------------|-----------|-------------|
| GET | /api/[recurso] | — | [ {id, campo} ] | Lista todos |
| GET | /api/[recurso]/:id | — | {id, campo} | Obtiene uno |
| POST | /api/[recurso] | {campo, campo} | {id, campo} | Crea uno |
| PUT | /api/[recurso]/:id | {campo} | {campo actualizado} | Actualiza |
| DELETE | /api/[recurso]/:id | — | {ok: true} | Elimina |
```

### Para APIs externas (webhooks):
Especificar el formato exacto del payload que llega, no solo la ruta.  
Las APIs externas son puntos de alta rigidez — Twilio, Stripe, etc. tienen formatos exactos.

---

## Output de esta Sección

Al completar A, B, C y D tenés el material para las **Secciones 2, 3 y 7** del template de prompt (Stack, Estructura, API).
