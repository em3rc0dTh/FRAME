# 01 — ORIENTACIÓN
## Clarificá la idea antes de escribir el prompt

Esta es la etapa más importante y la más ignorada.  
La mayoría de los proyectos fallan con IA porque se empieza a escribir el prompt sin tener claro qué se está construyendo.

> **Regla:** Si no podés responder todas las preguntas de esta sección, el proyecto no está listo para darle a una IA.

---

## Las 6 Preguntas de Orientación

Respondé estas preguntas en papel, en un doc, o directamente en el template de prompt. No las saltees.

---

### PREGUNTA 1 — ¿Qué hace la app?

Describila en **2 oraciones máximo**, como si se la explicaras a alguien que nunca programó.

```
La app es un sistema de [tipo] que permite a [usuario] hacer [acción principal].
Resuelve el problema de [problema concreto] eliminando la necesidad de [solución vieja].
```

**Ejemplo:**
> La app es un sistema de turnos que permite a los pacientes de una clínica dental agendar citas por WhatsApp.  
> Resuelve el problema de las llamadas telefónicas perdidas eliminando la necesidad de una recepcionista disponible 24/7.

**Señal de alarma:** Si necesitás más de 2 oraciones, el proyecto no está claro todavía.

---

### PREGUNTA 2 — ¿Quién la usa?

Definí los **actores** del sistema. Un actor es cualquier persona o sistema externo que interactúa con la app.

```
Actor primario: [quién usa la app todos los días]
Actor secundario: [quién usa la app ocasionalmente]
Actor sistema: [APIs, webhooks, servicios externos que interactúan]
```

**Ejemplo:**
- Actor primario: Pacientes de la clínica (via WhatsApp)
- Actor secundario: Personal de la clínica (via Dashboard web)
- Actor sistema: Twilio (webhook), OpenAI (API)

---

### PREGUNTA 3 — ¿Cuál es el flujo principal de valor?

El "happy path" — la secuencia de pasos que define el **valor central** de la app.

```
1. [Actor] hace [acción]
2. El sistema [reacciona]
3. [Actor] recibe [resultado]
```

**Ejemplo:**
1. Paciente envía mensaje por WhatsApp
2. El sistema procesa con IA y agenda el turno en la DB
3. Paciente recibe confirmación con fecha y hora

**Por qué importa:** Este flujo define qué features son críticos y cuáles son secundarios.

---

### PREGUNTA 4 — ¿Qué datos persisten?

Listá las **entidades principales** que la app necesita guardar. No el esquema completo todavía — solo las entidades y sus atributos clave.

```
Entidad: [nombre]
Atributos clave: [lista de campos más importantes]
Relaciones: [cómo se relaciona con otras entidades]
```

**Ejemplo:**
- Turno: fecha, paciente, tipo, estado
- Mensaje: texto, remitente, número de teléfono
- Configuración: datos de la clínica (única instancia)

---

### PREGUNTA 5 — ¿Qué integraciones externas hay?

Listá cada API o servicio externo que la app consume o del que recibe datos.

```
Integración: [nombre del servicio]
Dirección: [inbound (nos llaman) / outbound (nosotros llamamos) / bidireccional]
Propósito: [para qué se usa]
Credenciales: [qué keys/tokens se necesitan]
```

**Ejemplo:**
- Twilio: inbound webhook (recibimos mensajes de WhatsApp), API key
- OpenAI: outbound (llamamos para generar respuestas), API key

**Si no hay integraciones externas:** escribí "Ninguna — sistema autocontenido".

---

### PREGUNTA 6 — ¿Cuál es el criterio de éxito?

Define cuándo el proyecto está **terminado**. Esto va al final del prompt como checklist.

```
✅ [El flujo principal funciona end-to-end]
✅ [La DB se inicializa automáticamente al arrancar]
✅ [El frontend compila sin warnings]
✅ [Las integraciones externas conectan correctamente]
✅ [Toda la UI está en [idioma]]
✅ [README con instrucciones de instalación incluido]
```

---

## Output de esta Sección

Al terminar de responder las 6 preguntas, tenés el material para completar la **Sección 1 (CONTEXTO)** del template de prompt.

---

## Señales de que la Orientación está Completa

- [ ] Podés describir la app en 2 oraciones sin titubear
- [ ] Sabés exactamente cuántos tipos de usuarios hay
- [ ] El flujo principal tiene 3-5 pasos claros y concretos
- [ ] Sabés qué datos se guardan y cómo se relacionan
- [ ] Sabés qué APIs externas necesitás y qué credenciales
- [ ] Tenés al menos 5 criterios verificables de éxito
