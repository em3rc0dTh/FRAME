# 04 — CALIDAD
## Lo que convierte código funcional en código profesional

La calidad define los **requisitos no funcionales** — todo lo que hace que la app se vea bien, funcione rápido, sea segura y sea fácil de mantener.

> **Regla:** Sin esta sección, la IA genera código que funciona pero que tiene errores silenciosos, UI genérica y cero manejo de edge cases. Esta sección es lo que separa un MVP de un producto.

---

## A — Diseño Visual

Define la identidad visual antes de escribir el prompt. La IA puede diseñar, pero diseña genérico si no le das dirección.

### 4.A.1 — Paleta de Colores

Elegí siempre con hexadecimales. Nunca digas "azul" o "verde" — son demasiado amplios.

```
Color primario: #[hex] — [uso: botones, CTA, links]
Color secundario: #[hex] — [uso: backgrounds, cards]
Color de acento: #[hex] — [uso: badges, highlights, hover]
Color de texto: #[hex] — [uso: texto principal]
Color de texto suave: #[hex] — [uso: subtítulos, labels]
Background: #[hex] — [color de fondo general]
Color de error: #[hex] — [mensajes de error]
Color de éxito: #[hex] — [mensajes de confirmación]
```

### Paletas de referencia pre-definidas:

**Modo oscuro premium:**
```
Primario: #6366F1 (índigo eléctrico)
Secundario: #1E1E2E (fondo oscuro)
Acento: #A78BFA (violeta suave)
Texto: #E2E8F0
Background: #0F0F1A
```

**Modo claro corporativo:**
```
Primario: #1E40AF (azul profundo)
Secundario: #EFF6FF (azul muy suave)
Acento: #10B981 (verde salud)
Texto: #1E293B
Background: #FFFFFF
```

**Modo claro cálido:**
```
Primario: #D97706 (ámbar)
Secundario: #FFFBEB (crema)
Acento: #059669 (verde)
Texto: #292524
Background: #FAFAF9
```

---

### 4.A.2 — Tipografía

Siempre especificar fuente de Google Fonts. La IA usa por defecto Arial/sans-serif si no se le indica.

```
Fuente principal: [nombre exacto de Google Fonts]
Fuente de código (si aplica): [nombre, ej: JetBrains Mono]
```

### Fuentes recomendadas por estética:

| Estética | Fuente Recomendada | Por qué |
|---|---|---|
| Moderna y técnica | `Plus Jakarta Sans` | Geométrica, muy legible |
| Elegante y profesional | `DM Sans` | Humanista, cálida |
| Minimalista | `Inter` | Neutral, altamente legible |
| Editorial / Premium | `Fraunces` | Serif con personalidad |
| Startups / Tech | `Sora` | Moderna, amigable |
| Médico / Salud | `Nunito` | Redondeada, confiable |

---

### 4.A.3 — Estética General

Elegí 2-3 adjetivos del siguiente listado que definan la personalidad visual:

```
[ ] Glassmorphism (fondos con blur + transparencia)
[ ] Neumorphism (sombras suaves dentro del mismo fondo)
[ ] Minimalista (mucho espacio blanco, pocos elementos)
[ ] Bold / Maximalist (colores fuertes, tipografía grande)
[ ] Corporativo (formal, sin adornos)
[ ] Cálido (bordes redondeados, colores suaves)
[ ] Oscuro premium (dark mode, luces de neón suaves)
[ ] Editorial (mucho texto, tipografía como protagonista)
```

### 4.A.4 — Componentes Visuales

```
Bordes: [sharp (0px) / suaves (8px) / redondeados (16px) / pill (9999px)]
Sombras: [ninguna / suaves / dramáticas]
Hover effects: [ninguno / color / elevación / subrayado]
Animaciones: [ninguna / sutiles / elaboradas]
Responsive: [mobile-first / desktop-first]
```

---

## B — Requisitos No Funcionales

### 4.B.1 — Manejo de Errores

```
Formato de error: { "error": "[mensaje]" } o { "ok": false, "mensaje": "[mensaje]" }
Try/catch en: [todos los endpoints / solo endpoints críticos]
Logging: [qué información incluir en cada log]
```

### 4.B.2 — Logging

Define qué eventos se loguean y en qué formato:

```
Loguear con timestamp:
- [Evento 1]: "[formato del log]"
- [Evento 2]: "[formato del log]"
- Todos los errores con: timestamp + ruta + mensaje de error
```

**Ejemplo:**
```
[WEBHOOK] Mensaje recibido de +598XXXXXXXX: "Quiero un turno"
[OPENAI] Llamada iniciada para +598XXXXXXXX — tokens estimados: ~500
[ERROR] Ruta POST /api/turnos — Invalid date format: "2026-13-45"
```

### 4.B.3 — Validación de Inputs

```
Validar en todos los endpoints:
- [campo]: [regla de validación]
- [campo]: [regla de validación]
Formato de error de validación: { "error": "El campo [campo] es requerido" }
```

### 4.B.4 — Seguridad

```
CORS: permitir [http://localhost:XXXX] en desarrollo, [dominio.com] en producción
Rate limiting: [X] requests por [minuto/hora] por [IP / usuario / número de teléfono]
Variables de entorno: [lista de variables que NUNCA deben hardcodearse]
Auth: [ninguna / JWT / sesiones / OAuth]
```

### 4.B.5 — Performance

```
Polling: [cada X segundos en qué componentes]
Paginación: [X items por página en qué endpoints]
Caché: [qué respuestas cachear y por cuánto tiempo]
Lazy loading: [qué componentes cargar under demand]
```

---

## C — Convenciones de Código

Define el estilo de código antes de que la IA empiece a escribir.

```
Lenguaje: [JavaScript / TypeScript / Python]
Módulos: [CommonJS (require) / ESModules (import)]
Async: [Promises (.then) / async/await]
Comentarios: [en qué idioma / cuándo comentar]
Nombres de variables: [camelCase / snake_case / PascalCase]
Nombres de archivos: [camelCase / kebab-case / PascalCase]
```

**Ejemplo:**
```
Lenguaje: JavaScript puro (sin TypeScript)
Módulos: CommonJS (require/module.exports)
Async: async/await en todos los handlers
Comentarios: en español, explicando el "por qué" no el "qué"
Variables: camelCase
Archivos backend: camelCase | Archivos frontend: PascalCase para componentes
```

---

## D — README del Proyecto

Especificá que el README debe incluir estas secciones (la IA lo genera completo si se lo pedís):

```
README debe incluir:
- Descripción del proyecto (2 párrafos)
- Stack tecnológico
- Pre-requisitos (Node version, etc.)
- Instalación paso a paso
- Configuración de variables de entorno
- [Configuración de integración externa si aplica]
- Cómo correr en desarrollo
- Cómo hacer build para producción
- Estructura del proyecto explicada
```

---

## Output de esta Sección

Al completar A, B, C y D tenés el material para las **Secciones 8, 9 y 10** del template de prompt (Variables de Entorno, Scripts, Requisitos No Funcionales).
