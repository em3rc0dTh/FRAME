# 🧠 FRAME — Framework de Prompting para Proyectos con IA

> **F**undación · **R**equisitos · **A**rquitectura · **M**odelo · **E**xpectativas  
> Un sistema para lanzar cualquier app en un solo tiro con IA, sin iterar infinitamente.

---

## ¿Qué es FRAME?

FRAME es una estructura de trabajo para **preparar proyectos antes de darlos a una IA**.  
No es código, no es un boilerplate — es el **proceso de pensar** que convierte una idea en un prompt que genera una app completa, funcional y bien organizada desde el primer intento.

El principio central es simple:  
> **La IA implementa. Vos diseñás. Este framework es el puente.**

---

## Estructura del Repositorio

```
FRAME/
├── README.md                          ← Estás aquí. Visión general del sistema
│
├── 01-orientacion/                    ← Cómo pensar el proyecto ANTES de escribir
│   └── README.md                      ← Preguntas clave para clarificar la idea
│
├── 02-arquitectura/                   ← Decisiones técnicas y estructura de código
│   └── README.md                      ← Stack, árbol de archivos, DB, API
│
├── 03-comportamiento/                 ← Lógica de negocio y flujos de usuario
│   └── README.md                      ← Features, flujos, reglas, errores
│
├── 04-calidad/                        ← NFRs, estilo visual, criterios de éxito
│   └── README.md                      ← No funcionales, diseño, checklist final
│
├── 05-agentes-ia/                     ← Para proyectos que incluyen un agente IA
│   └── README.md                      ← System prompt, tools, identidad, reglas
│
├── templates/
│   ├── PROMPT_TEMPLATE.md             ← ⭐ Template maestro de prompt (copiar y completar)
│   ├── PROMPT_AGENTE_IA.md            ← Template de system prompt para agentes
│   └── CHECKLIST_PREENVIO.md          ← Checklist de 16 puntos antes de enviar a la IA
│
└── ejemplos/
    └── clinica-dental/
        └── prompt-completo.md         ← Ejemplo real analizado (sistema de turnos)
```

---

## Cómo Usar FRAME en un Proyecto Nuevo

### Paso 1 — Orientarte (5 minutos)
Leer `01-orientacion/README.md` y responder las preguntas. Esto clarifica la idea antes de escribir una línea de prompt.

### Paso 2 — Definir Arquitectura (10-20 minutos)
Completar el árbol de archivos, el stack y el esquema de base de datos siguiendo `02-arquitectura/README.md`.

### Paso 3 — Mapear Comportamiento (10-15 minutos)
Documentar los flujos principales y reglas de negocio siguiendo `03-comportamiento/README.md`.

### Paso 4 — Definir Calidad (5 minutos)
Completar paleta, tipografía y requisitos no funcionales siguiendo `04-calidad/README.md`.

### Paso 5 — Armar el Prompt
Copiar `templates/PROMPT_TEMPLATE.md`, completar cada sección con lo definido en los pasos anteriores, verificar con `templates/CHECKLIST_PREENVIO.md`.

### Paso 6 — Enviar a la IA
Pegar el prompt completo en Claude, ChatGPT, Cursor, o la IA de tu elección.

---

## Las 10 Reglas de Oro

1. **Presentá el proyecto como ya existente** — La IA implementa, no diseña
2. **Stack completo con razón de ser** — Cada tecnología tiene su justificación
3. **Árbol de archivos con responsabilidades** — Define arquitectura sin ambigüedad
4. **Código exacto solo en puntos de alto riesgo** — SQL, formatos de API externa, TwiML
5. **Tabla de API obligatoria en fullstack** — Es el contrato entre frontend y backend
6. **Flujo paso a paso en cada feature crítico** — General → Componente → Dato → Error
7. **Paleta + tipografía + 2 adjetivos de estilo** — Reemplaza "hacelo bonito"
8. **Requisitos no funcionales explícitos** — La calidad del código no es opcional
9. **Criterios de éxito verificables** — Define cuándo está terminado
10. **Nivel de detalle proporcional al riesgo** — No sobre-especifiques lo que la IA ya sabe

---

## Principio de Densidad de Información

| Área | Nivel de Detalle | Razón |
|---|---|---|
| Esquema de DB | ☐☐☐☐☐ Máximo | Un campo mal tipado rompe todo |
| Flujo de webhook / API externa | ☐☐☐☐☐ Máximo | Formatos como TwiML son intolerantes a errores |
| Contrato de API interna | ☐☐☐☐☐ Alto | El 80% de bugs fullstack son rutas que no coinciden |
| Componentes UI con lógica | ☐☐☐☐☐ Alto | La IA puede inventar datos incorrectos |
| Componentes UI de presentación | ☐☐☐ Medio | La IA sabe hacer CSS bien |
| Boilerplate (Vite, ESLint, etc.) | ☐ Bajo | La IA conoce el boilerplate de memoria |
| Lógica interna de hooks | ☐ Bajo | Solo especificá el contrato (qué datos, cuándo) |

---

*FRAME es un sistema vivo. Actualizalo cuando descubras nuevos patrones en tus proyectos.*
