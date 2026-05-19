# PROMPT TEMPLATE — FRAME
## Template Maestro para Generación de Apps con IA

> **Instrucciones de uso:**
> 1. Copiar este archivo completo a tu nuevo proyecto
> 2. Completar cada sección entre corchetes `[...]`
> 3. Eliminar las secciones que no apliquen (ej: 05-AGENTE si no hay IA)
> 4. Eliminar todas las instrucciones en cursiva y comentarios de ayuda antes de enviar
> 5. Verificar con `CHECKLIST_PREENVIO.md` antes de enviar a la IA

---

════════════════════════════════════════════════
[NOMBRE DEL PROYECTO] — [TIPO: App Web / API / Dashboard / Bot / etc.]
════════════════════════════════════════════════

## SECCIÓN 01 — CONTEXTO

Construí [descripción de la app en 2 oraciones máximo, en pasado como si ya existiera].
El sistema tiene [descripción de las partes principales: backend, frontend, integraciones].

**Usuarios:**
- [Actor primario]: [qué hace en la app]
- [Actor secundario]: [qué hace en la app]

**Flujo principal de valor:**
1. [Actor] hace [acción]
2. El sistema [procesa / valida / guarda]
3. [Actor] recibe [resultado concreto]

---

## SECCIÓN 02 — STACK TECNOLÓGICO

```
Backend:       [tecnología] — [razón en ≤5 palabras]
Base de datos: [tecnología] — [razón en ≤5 palabras]
Frontend:      [tecnología] — [razón en ≤5 palabras]
Estilos:       [tecnología] — [razón en ≤5 palabras]
[Auth]:        [tecnología o "ninguna"] — [razón]
[Extra]:       [tecnología] — [razón]
Idioma del código y UI: [español / inglés]
```

---

## SECCIÓN 03 — ESTRUCTURA DEL PROYECTO

```
[nombre-proyecto]/
├── [carpeta-backend]/
│   ├── [archivo-principal].js        ← [responsabilidad en una línea]
│   ├── [archivo-db].js               ← [responsabilidad en una línea]
│   ├── routes/
│   │   └── [recurso].js              ← [responsabilidad en una línea]
│   ├── services/
│   │   └── [servicio].js             ← [responsabilidad en una línea]
│   └── utils/
│       └── [helpers].js              ← [responsabilidad en una línea]
├── [carpeta-frontend]/
│   ├── src/
│   │   ├── App.jsx                   ← Router principal
│   │   ├── main.jsx                  ← Entry point React
│   │   ├── pages/
│   │   │   └── [Pagina].jsx          ← [responsabilidad]
│   │   ├── components/
│   │   │   └── [Componente].jsx      ← [responsabilidad]
│   │   └── lib/
│   │       └── api.js                ← Todas las llamadas fetch centralizadas
│   └── index.html
├── .env.example
├── package.json
└── README.md
```

---

## SECCIÓN 04 — BASE DE DATOS — ESQUEMA

Crear las siguientes [N] tablas/colecciones al iniciar la app en [[archivo-db].js]:

### Tabla/Colección: [nombre_tabla_1]

```sql
CREATE TABLE IF NOT EXISTS [nombre_tabla_1] (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  [campo_1] TEXT NOT NULL,
  [campo_2] TEXT DEFAULT '[valor]' CHECK([campo_2] IN ('[op1]', '[op2]', '[op3]')),
  [campo_3] TEXT,
  creado_en DATETIME DEFAULT (datetime('now', 'localtime')),
  actualizado_en DATETIME DEFAULT (datetime('now', 'localtime'))
);
```

[Crear trigger para actualizar `actualizado_en` automáticamente en cada UPDATE si aplica.]
[Insertar [N] registros de seed/ejemplo realistas al crear la tabla si aplica.]

### Tabla/Colección: [nombre_tabla_2]

```sql
CREATE TABLE IF NOT EXISTS [nombre_tabla_2] (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  [tabla_1]_id INTEGER NOT NULL REFERENCES [nombre_tabla_1](id),
  [campo] TEXT NOT NULL,
  creado_en DATETIME DEFAULT (datetime('now', 'localtime'))
);
```

[Repetir por cada tabla/colección adicional]

---

## SECCIÓN 05 — BACKEND — FLUJOS CRÍTICOS

[Por cada endpoint o flujo de negocio crítico, detallar lo siguiente:]

### [NOMBRE DEL FLUJO / ENDPOINT]

**Endpoint:** `[MÉTODO] /api/[ruta]`

**Flujo completo:**
1. [Validar / parsear el input — qué se espera recibir]
2. [Acción contra la DB o servicio externo]
3. [Qué se devuelve al cliente]

**Formato de respuesta exitosa:**
```json
{
  "[campo]": "[valor de ejemplo]"
}
```

**Manejo de errores:**
- Si [condición de error]: responder `{ "error": "[mensaje]" }` con status [código]
- Si falla [servicio externo]: responder con `{ "error": "[mensaje de fallback amigable]" }`

**Logging:** Loguear `[qué información]` al inicio / al error.

---

[Repetir bloque anterior por cada flujo crítico]

---

## SECCIÓN 06 — FRONTEND

### Página: [Nombre de la Página] ([/ruta])

**Descripción:** [Qué hace esta página y para quién]

**Secciones:**

#### [Nombre de Sección / Componente]
- **Qué muestra:** [descripción del contenido]
- **Origen del dato:** `[GET /api/endpoint]` — polling cada [X] segundos / al cargar / manual
- **Comportamiento:** [qué pasa cuando el usuario interactúa]
- **Estado vacío:** [qué mostrar si no hay datos]
- **Estado de carga:** [skeleton / spinner / ninguno]

#### [Nombre de Sección / Componente]
- **Qué muestra:** [descripción]
- **Formulario:** Campos: [campo1 (tipo)], [campo2 (tipo)], [campo3 (tipo)]
- **Submit:** `[PUT/POST /api/endpoint]` — mostrar [toast de éxito / error] después
- **Validación:** [en tiempo real / al perder foco / solo al submit]

[Repetir por cada sección de la página]

---

### Página: [Nombre de la Segunda Página] ([/ruta])

[Repetir estructura anterior]

---

**Diseño visual — especificaciones globales:**

```
Paleta:
  Primario:    #[hex] — botones, CTAs, links
  Secundario:  #[hex] — backgrounds, cards
  Acento:      #[hex] — badges, highlights
  Texto:       #[hex]
  Background:  #[hex]
  Error:       #[hex]
  Éxito:       #[hex]

Tipografía: [Nombre exacto de Google Fonts]

Estética: [2-3 adjetivos: ej: minimalista, cálida, glassmorphism]
Bordes: [sharp / suaves (8px) / redondeados (16px)]
Sombras: [ninguna / suaves / dramáticas]
Animaciones: [ninguna / sutiles de entrada / elaboradas]
Responsive: [mobile-first / desktop-first]
```

---

## SECCIÓN 07 — CONTRATO API

Todos los endpoints bajo `/api/`:

| Método | Ruta | Body / Params | Respuesta | Descripción |
|--------|------|---------------|-----------|-------------|
| GET | /api/[recurso] | — | `[{id, campo}]` | [descripción] |
| GET | /api/[recurso]/:id | `id` en URL | `{id, campo}` | [descripción] |
| POST | /api/[recurso] | `{campo, campo}` | `{id, campo}` | [descripción] |
| PUT | /api/[recurso]/:id | `{campo}` | `{campo actualizado}` | [descripción] |
| DELETE | /api/[recurso]/:id | — | `{ok: true}` | [descripción] |

[Agregar o quitar filas según los endpoints reales del proyecto]

---

## SECCIÓN 08 — VARIABLES DE ENTORNO

Crear archivo `.env.example` con:

```
[VARIABLE_1]=descripcion_de_donde_obtenerla
[VARIABLE_2]=descripcion_de_donde_obtenerla
PORT=[numero_de_puerto_default]
```

El server debe leer de `.env` usando [dotenv / process.env / config].  
Si [VARIABLE_CRÍTICA] no está configurada, mostrar error claro al iniciar y detener el proceso.

---

## SECCIÓN 09 — SCRIPTS DE PACKAGE.JSON

```json
{
  "scripts": {
    "dev": "[comando para correr en desarrollo — frontend + backend simultáneo]",
    "build": "[comando para compilar el frontend]",
    "start": "[comando para correr en producción]",
    "setup": "[comando de instalación inicial de todas las dependencias]"
  }
}
```

[En producción, [backend] sirve los archivos estáticos del build de [frontend].]

---

## SECCIÓN 10 — REQUISITOS NO FUNCIONALES

**Manejo de errores:**
- Try/catch en todos los endpoints
- Formato consistente de error: `{ "error": "mensaje", "detalle": "..." }`
- [Regla específica de error adicional]

**Logging:**
- Loguear con timestamp: [evento 1], [evento 2], todos los errores
- Formato: `[CONTEXTO] descripción — dato relevante`

**Validación:**
- Validar inputs en todos los endpoints antes de tocar la DB
- [Regla de validación específica]

**Seguridad:**
- CORS: configurado para [localhost:XXXX] en desarrollo
- Rate limiting: [X] requests por minuto por [IP / número de teléfono]
- [Regla de seguridad adicional]

**Código:**
- [JavaScript puro / TypeScript] — [sin TypeScript / con strict mode]
- Comentarios en [idioma], explicando el "por qué", no el "qué"
- [Convención de nombres de variables y archivos]

**UI:**
- Toda la interfaz en [idioma]: labels, placeholders, mensajes de error, tooltips
- Fechas en [locale]: formato `[ejemplo: "22 de marzo de 2026, 14:30"]`

---

## SECCIÓN 11 — INSTRUCCIONES FINALES

Creá TODO el proyecto desde cero con la estructura definida en Sección 03.

Asegurate de que:
- ✅ El server arranca sin errores y la DB se inicializa automáticamente
- ✅ El frontend compila sin warnings
- ✅ El flujo principal ([descripción del happy path]) funciona end-to-end
- ✅ [Criterio de éxito específico del proyecto]
- ✅ [Criterio de éxito específico del proyecto]

Incluí un `README.md` en [idioma] con:
- Descripción del proyecto
- Pre-requisitos
- Instalación de dependencias
- Configuración de variables de entorno
- [Configuración de integración externa si aplica]
- Cómo levantar en desarrollo
- Cómo hacer build para producción
- Estructura del proyecto explicada

[No uses TypeScript — JavaScript puro para simplicidad] [ELIMINAR SI USÁS TS]
[Todo el texto visible al usuario debe estar en [idioma]]
