# PROMPT TEMPLATE — AGENTE IA
## System Prompt + Function Calling para Agentes Conversacionales

> **Cuándo usar este template:**  
> Cuando tu proyecto incluye un agente de IA conversacional que toma acciones (agenda turnos, busca productos, responde preguntas con datos reales, etc.)  
> Este template define el **system prompt** y las **tools** del agente — se incluye dentro de `server/services/[ia].js`.

---

## PARTE A — SYSTEM PROMPT DEL AGENTE

```
Sos [NOMBRE], [ROL] de [EMPRESA/NEGOCIO].
Sos [adjetivo 1], [adjetivo 2] y [adjetivo 3].

TU ROL ES:
- [Tarea principal 1]
- [Tarea principal 2]
- [Tarea principal 3]

ESTILO DE COMUNICACIÓN:
- Idioma: [español rioplatense / español neutro / inglés]
- Formalidad: [tuteo con "vos" / tuteo con "tú" / formal con "usted"]
- Longitud: [respuestas concisas de 1-3 oraciones / respuestas detalladas]
- Emojis: [usar moderadamente / no usar / usar frecuentemente]

INFORMACIÓN SOBRE [EMPRESA/NEGOCIO]:
- Nombre: {nombre_empresa}         ← Variable dinámica desde DB
- [Campo]: {valor}                  ← Variable dinámica desde DB
- [Campo]: {valor}                  ← Variable dinámica desde DB
- Servicios disponibles: {servicios}

REGLAS IMPORTANTES — LO QUE SIEMPRE DEBÉS HACER:
- [Regla de comportamiento 1]
- [Regla de comportamiento 2]
- Antes de [acción], siempre pedí: [dato requerido 1], [dato requerido 2]

REGLAS IMPORTANTES — LO QUE NUNCA DEBÉS HACER:
- Nunca inventés información que no tengas en tu contexto
- Nunca confirmés [acción] sin ejecutar la tool correspondiente
- [Restricción específica del negocio]

CUANDO NO PODÉS RESOLVER ALGO:
- Ofrecé comunicar al usuario con [persona / canal alternativo]
- Siempre dejá al usuario con una próxima acción clara
```

---

## PARTE B — TOOLS (FUNCTION CALLING)

### Estructura base de cada tool:

```javascript
{
  type: "function",
  function: {
    name: "[accion_en_snake_case]",
    description: `[Descripción detallada para la IA de cuándo usar esta tool,
                  qué hace exactamente y qué devuelve. Mínimo 2 oraciones.]`,
    parameters: {
      type: "object",
      properties: {
        [param_1]: {
          type: "[string|integer|boolean|number]",
          description: "[Descripción del parámetro]"
        },
        [param_2]: {
          type: "string",
          enum: ["[opcion1]", "[opcion2]", "[opcion3]"],
          description: "[Descripción del parámetro con opciones fijas]"
        },
        [param_opcional]: {
          type: "string",
          description: "[Descripción — este parámetro es opcional]"
        }
      },
      required: ["[param_1]", "[param_2]"]
    }
  }
}
```

---

### Tools de referencia por categoría:

**Categoría: CONSULTA DE DATOS**
```javascript
// Buscar registros
{
  name: "buscar_[entidad]",
  description: `Usa esta tool cuando el usuario quiera consultar [entidad].
                Devuelve la lista de [entidades] que coinciden con los criterios.`,
  parameters: {
    properties: {
      [criterio_busqueda]: { type: "string", description: "..." }
    },
    required: ["[criterio_busqueda]"]
  }
}

// Obtener un registro específico
{
  name: "obtener_[entidad]",
  description: `Usa esta tool cuando el usuario quiera ver el detalle de [entidad] específico.`,
  parameters: {
    properties: {
      id: { type: "integer", description: "ID del registro" }
    },
    required: ["id"]
  }
}
```

**Categoría: ESCRITURA / ACCIONES**
```javascript
// Crear un registro
{
  name: "crear_[entidad]",
  description: `Usa esta tool para crear un nuevo [entidad]. 
                Verificá disponibilidad antes si aplica.
                Devuelve el [entidad] creado con su ID.`,
  parameters: {
    properties: {
      [campo_requerido]: { type: "string", description: "..." },
      [campo_requerido]: { type: "string", description: "..." },
      [campo_opcional]: { type: "string", description: "..." }
    },
    required: ["[campo1]", "[campo2]"]
  }
}

// Actualizar estado
{
  name: "actualizar_estado_[entidad]",
  description: `Usa esta tool para cambiar el estado de un [entidad].
                Estados posibles: '[estado1]', '[estado2]', '[estado3]'.`,
  parameters: {
    properties: {
      id: { type: "integer", description: "ID del registro a actualizar" },
      nuevo_estado: {
        type: "string",
        enum: ["[estado1]", "[estado2]", "[estado3]"],
        description: "El nuevo estado"
      }
    },
    required: ["id", "nuevo_estado"]
  }
}
```

**Categoría: VERIFICACIÓN / DISPONIBILIDAD**
```javascript
{
  name: "verificar_disponibilidad",
  description: `Usa esta tool ANTES de crear [entidad] para verificar disponibilidad.
                Devuelve los slots disponibles y ocupados.`,
  parameters: {
    properties: {
      [parametro]: { type: "string", description: "..." }
    },
    required: ["[parametro]"]
  }
}
```

---

## PARTE C — LÓGICA DE EJECUCIÓN DE TOOLS

Esta es la lógica que va en `server/services/[ia].js`:

```javascript
// 1. Preparar el contexto
const systemPrompt = generarSystemPrompt(configuracion); // Inyecta variables dinámicas
const historial = obtenerHistorial(numero_usuario, 20); // Últimos 20 mensajes

// 2. Primera llamada a la IA
const respuesta = await openai.chat.completions.create({
  model: "gpt-4o",
  messages: [
    { role: "system", content: systemPrompt },
    ...historial,
    { role: "user", content: mensaje_usuario }
  ],
  tools: tools,
  tool_choice: "auto"
});

// 3. Si la IA quiere usar tools
if (respuesta.choices[0].finish_reason === "tool_calls") {
  const tool_calls = respuesta.choices[0].message.tool_calls;
  const resultados = [];

  for (const tool_call of tool_calls) {
    const nombre_tool = tool_call.function.name;
    const args = JSON.parse(tool_call.function.arguments);

    // Ejecutar la tool
    const resultado = await ejecutarTool(nombre_tool, args);

    resultados.push({
      tool_call_id: tool_call.id,
      role: "tool",
      content: JSON.stringify(resultado)
    });
  }

  // 4. Segunda llamada con resultados de tools
  const respuesta_final = await openai.chat.completions.create({
    model: "gpt-4o",
    messages: [
      { role: "system", content: systemPrompt },
      ...historial,
      { role: "user", content: mensaje_usuario },
      respuesta.choices[0].message,  // Mensaje original del asistente con tool_calls
      ...resultados                   // Resultados de cada tool
    ]
  });

  return respuesta_final.choices[0].message.content;
}

// 5. Si no usa tools, devolver respuesta directa
return respuesta.choices[0].message.content;
```

---

## PARTE D — FUNCIÓN DE DESPACHO DE TOOLS

```javascript
async function ejecutarTool(nombre, args) {
  switch (nombre) {
    case "[nombre_tool_1]":
      return await [funcionDeTool1](args);

    case "[nombre_tool_2]":
      return await [funcionDeTool2](args);

    // Para cada tool definida en PARTE B
    
    default:
      return { error: `Tool desconocida: ${nombre}` };
  }
}
```

---

## PARTE E — FALLBACKS Y MANEJO DE ERRORES

```
Si OpenAI no responde / timeout:
  → Responder: "[Mensaje amigable explicando el problema] Podés [alternativa: llamar / escribir a X]."

Si una tool falla:
  → La IA recibe: { error: "No se pudo completar la acción. Motivo: [descripción]" }
  → La IA debe informar al usuario que no pudo completar la acción y ofrecer alternativa

Si el mensaje del usuario es incomprensible:
  → La IA pide clarificación sin asumir intención
```
