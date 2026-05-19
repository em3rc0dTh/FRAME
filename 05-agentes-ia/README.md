# 05 — AGENTES IA
## Cómo diseñar un agente de inteligencia artificial dentro de tu app

Esta sección aplica solo cuando tu proyecto incluye un **agente de IA conversacional** — un bot que procesa lenguaje natural y toma acciones (función calling, RAG, etc.).

> **Regla:** Un agente de IA mal especificado no es peligroso — es inútil. La calidad del agente depende casi 100% de la calidad de su system prompt y sus tools.

---

## A — Identidad del Agente

Define la **personalidad** del agente. Esto no es cosmético — afecta directamente qué tan bien convierte el agente en su rol.

### Formato:

```
NOMBRE: [nombre humano, no "Bot" ni "Asistente"]
ROL: [título funcional — ej: "recepcionista virtual", "asesor de ventas"]
PERSONALIDAD: [3 adjetivos: amable, profesional, conciso]
TONO: [formal / informal / semi-formal]
```

### Reglas de identidad que funcionan bien:

```
- Nombre humano → genera más confianza en usuarios
- Rol específico → la IA entiende sus límites
- "Nunca digas que sos una IA" (si es estratégico para la UX)
- "Si te preguntan si sos una IA, decí que sos el asistente virtual de [empresa]"
```

---

## B — Idioma y Estilo de Comunicación

```
IDIOMA: [español / inglés / etc.]
VARIANTE: [español rioplatense / español neutro / español mexicano / etc.]
FORMALIDAD: [tuteo ("vos") / tuteo neutro ("tú") / formal ("usted")]
EMOJIS: [nunca / moderadamente (1-2 por mensaje) / frecuentemente]
LONGITUD DE RESPUESTAS: [concisas (1-3 oraciones) / normales / detalladas]
LISTAS: [usar listas cuando hay 3+ items / siempre texto corrido / libre]
```

---

## C — Conocimiento Base (Context Window)

Define qué información contextual se inyecta dinámicamente en el system prompt.

### Tipos de contexto:

**Contexto estático** — se define una vez y no cambia:
```
- Nombre de la empresa
- Descripción del negocio
- Servicios disponibles
- Políticas (devoluciones, cancelaciones, etc.)
```

**Contexto dinámico** — se carga desde la DB en cada conversación:
```
- Datos de la cuenta del usuario
- Historial de conversación (últimos N mensajes)
- Estado actual del pedido/turno/etc.
- Configuración personalizada
```

### Formato del system prompt con contexto dinámico:

```
Sos [NOMBRE], [ROL] de [EMPRESA]. [PERSONALIDAD en 1 oración].

INFORMACIÓN DEL NEGOCIO:
- Nombre: {nombre_empresa}
- [Campo dinámico]: {valor_dinamico}
- Servicios: {lista_servicios}

HISTORIAL DE CONVERSACIÓN RECIENTE:
[Inyectado automáticamente - últimos N mensajes]

REGLAS DE COMPORTAMIENTO:
[Ver sección E]
```

---

## D — Tools (Function Calling)

Las tools son las **acciones concretas** que el agente puede ejecutar contra tu sistema.

### Formato de definición de cada tool:

```javascript
{
  name: "[accion_en_snake_case]",
  description: "[Descripción de cuándo usar esta tool — escrita para la IA, no para el usuario]",
  parameters: {
    type: "object",
    properties: {
      [param]: {
        type: "[string|integer|boolean|array]",
        description: "[Descripción del parámetro]",
        enum: ["opcion1", "opcion2"]  // Solo si tiene valores fijos
      }
    },
    required: ["[params obligatorios]"]
  }
}
```

### Principios para diseñar tools:

**Nombres claros y verbosos:**
```
✅ consultar_disponibilidad_turno
❌ check_slot
```

**Descripciones escritas para la IA:**
```
✅ "Usa esta tool cuando el usuario quiera ver horarios disponibles. Requiere una fecha. 
    Devuelve los horarios ocupados y libres del día."
❌ "Get available slots for a date"
```

**Una tool, una responsabilidad:**
```
✅ agendar_turno / cancelar_turno / reprogramar_turno (separadas)
❌ gestionar_turno (con parámetro "accion") — confunde a la IA
```

### Flujo de ejecución con function calling:

```
1. Request inicial → OpenAI (con tools definidas + historial)
2. OpenAI devuelve → tool_calls (si decide usar una tool)
3. Tu código → ejecuta la función contra la DB
4. Enviás resultado → a OpenAI como role: "tool"
5. OpenAI devuelve → respuesta final en lenguaje natural
```

**Siempre manejar:**
- ¿Qué pasa si la tool falla?
- ¿Qué pasa si OpenAI llama múltiples tools en paralelo?
- ¿Cuál es el mensaje de fallback si la API de IA no responde?

---

## E — Reglas de Comportamiento

Define explícitamente qué **puede** y **no puede** hacer el agente.

### Formato:

```
PUEDE:
- [Acción 1]
- [Acción 2]

NO PUEDE / NUNCA DEBE:
- [Restricción 1]
- [Restricción 2]

CUANDO NO PUEDE RESOLVER:
- [Cómo escalar: "ofrecé comunicar con [persona/canal]"]
```

### Reglas recomendadas para todo agente:

```
NO PUEDE:
- Inventar información que no tiene en su contexto
- Confirmar acciones sin ejecutar la tool correspondiente
- Dar precios, fechas o datos específicos si no los tiene en contexto
- Salirse del rol definido (ej: escribir código si es recepcionista)

CUANDO NO PUEDE RESOLVER:
- Siempre ofrecer un canal alternativo (teléfono, email, persona real)
- Nunca dejar al usuario sin una próxima acción
```

---

## F — Memoria Conversacional

Define cómo el agente recuerda el contexto de la conversación.

```
TIPO DE MEMORIA: [ventana de contexto / base de datos / ninguna]
CANTIDAD: [últimos N mensajes incluidos en cada request]
IDENTIFICADOR DE USUARIO: [por qué campo se identifica (teléfono, user_id, email)]
PERSISTENCIA: [se guarda en DB / solo en memoria / session storage]
```

### Tradeoffs de cantidad de historial:

| Cantidad | Pros | Contras |
|---|---|---|
| Últimos 5 mensajes | Tokens bajos, costo bajo | Pierde contexto en conversaciones largas |
| Últimos 20 mensajes | Balance ideal para la mayoría | Costo medio |
| Conversación completa | Contexto perfecto | Caro, puede exceder context window |

---

## Output de esta Sección

Al completar A, B, C, D, E y F tenés el material para construir el **system prompt del agente** y la **definición completa de sus tools**, listo para incluir en el prompt del proyecto.
