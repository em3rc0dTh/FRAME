# 03 — COMPORTAMIENTO
## La lógica de negocio que la IA no puede inferir

El comportamiento define **qué hace la app cuando algo pasa**. Es la diferencia entre una app que existe y una app que funciona correctamente.

> **Regla:** Todo flujo sin especificar es un flujo que la IA implementa según su criterio. Su criterio no conoce tu negocio.

---

## A — Features Principales

Listá todas las funcionalidades de la app. Organizalas en:
- **Core features** — sin estas la app no existe
- **Features de soporte** — mejoran la experiencia
- **Nice to have** — para versiones futuras (NO incluir en el prompt inicial)

### Formato:

```
CORE FEATURES:
[ ] [Feature 1] — [descripción en una oración]
[ ] [Feature 2] — [descripción en una oración]

FEATURES DE SOPORTE:
[ ] [Feature] — [descripción]

FUERA DE ALCANCE (no incluir en el prompt):
- [Feature que sería nice to have pero no es crítica]
```

### ⚠️ Trampa común: el "scope creep" en el prompt
Incluir features de nice-to-have en el prompt inicial **multiplica la complejidad** y aumenta la chance de que la IA cometa errores en lo crítico. Lanzá primero lo core.

---

## B — Flujos de Usuario (User Flows)

Para cada feature core, definí el flujo completo:
- El **happy path** (lo que pasa cuando todo sale bien)
- Los **casos de error** (lo que pasa cuando algo falla)

### Formato de flujo:

```
FLUJO: [Nombre del flujo]
Actor: [quién lo inicia]

Happy Path:
1. El usuario [hace acción]
2. El sistema [valida / procesa]
3. El sistema [guarda / envía]
4. El usuario [recibe / ve]

Casos de Error:
- Si [condición de error]: responder con [mensaje exacto o comportamiento]
- Si [condición de error]: responder con [mensaje exacto o comportamiento]

Validaciones requeridas:
- [campo] es obligatorio
- [campo] debe ser [formato/tipo]
- [regla de negocio]
```

### Ejemplo real:

```
FLUJO: Agendar turno por WhatsApp
Actor: Paciente (via WhatsApp)

Happy Path:
1. El paciente envía mensaje pidiendo turno
2. La IA solicita: nombre completo, fecha preferida, tipo de consulta
3. El sistema verifica disponibilidad en la DB
4. El sistema crea el turno con estado 'confirmado'
5. La IA responde con la confirmación y los detalles del turno

Casos de Error:
- Si el horario está ocupado: sugerir los próximos 3 horarios disponibles
- Si OpenAI falla: responder "Disculpá, estoy teniendo problemas técnicos. Llamá al {telefono}"
- Si faltan datos: volver a pedir los datos faltantes antes de agendar

Validaciones:
- Nombre es obligatorio para crear el turno
- La fecha debe ser futura (no pasada)
- No agendar fuera del horario de atención (9:00 - 18:00)
```

---

## C — Reglas de Negocio

Las reglas de negocio son restricciones que el sistema debe respetar siempre. Son difíciles de inferir para la IA porque dependen del contexto de tu negocio.

### Formato:

```
REGLAS DE NEGOCIO:
- [Regla 1]: [descripción concreta y verificable]
- [Regla 2]: [descripción concreta y verificable]

REGLAS DE DATOS:
- [Entidad] puede estar en estados: [estado1], [estado2], [estado3]
- Transiciones permitidas: [estado1] → [estado2], [estado2] → [estado3]
- Transiciones NO permitidas: [estado2] → [estado1]
```

### Tipos de reglas comunes:

**Reglas de estado:**
```
Un turno solo puede pasar de 'pendiente' a 'confirmado' o 'cancelado'.
Un turno 'completado' no puede volver a 'confirmado'.
```

**Reglas de acceso:**
```
Solo el admin puede eliminar registros.
Un usuario solo puede ver sus propios datos.
```

**Reglas de tiempo:**
```
No se pueden agendar turnos con menos de 2 horas de anticipación.
Los turnos cancelados con más de 24hs de anticipación no tienen cargo.
```

**Reglas de cantidad:**
```
Un paciente no puede tener más de 3 turnos activos simultáneos.
Máximo 4 turnos por hora en el mismo consultorio.
```

---

## D — Comportamiento de la UI

Específica cómo reacciona cada componente visual a las acciones del usuario.

### Categorías de comportamiento UI:

**Estados de carga:**
```
- Mostrar [skeleton / spinner / texto "Cargando..."] mientras se espera la respuesta
- Deshabilitar el botón de submit mientras hay una request en curso
```

**Feedback al usuario:**
```
- Toast de éxito: "[mensaje]" — duración [X] segundos
- Toast de error: "[mensaje de error]" — con botón de retry
- Confirmación antes de [acción destructiva]: "¿Estás seguro de que querés [acción]?"
```

**Actualización en tiempo real:**
```
- Polling cada [X] segundos en [componente]
- WebSocket para [evento en tiempo real]
```

**Estados vacíos:**
```
- Si no hay [datos]: mostrar "[mensaje amigable]" en lugar de lista vacía
```

**Formularios:**
```
- Validación: [en tiempo real mientras escribe / al perder foco / solo al submit]
- Reset: [limpiar form después de submit exitoso / no limpiar]
```

---

## E — Manejo de Errores Global

Define el patrón de respuesta de error que la IA debe usar en todos los endpoints.

### Formato estándar de error (elegí uno y usalo siempre):

```javascript
// Opción A: Simple
{ "error": "Mensaje de error legible" }

// Opción B: Con detalle
{
  "error": "Mensaje de error legible",
  "detalle": "Información técnica adicional",
  "codigo": "ERROR_CODE"
}

// Opción C: Con contexto
{
  "ok": false,
  "mensaje": "Mensaje de error legible",
  "campo": "nombre_campo",  // si aplica
  "codigo": 400
}
```

### HTTP Status codes — cuándo usar cada uno:

| Código | Cuándo Usarlo |
|---|---|
| 200 | Operación exitosa |
| 201 | Recurso creado exitosamente |
| 400 | Input inválido del cliente |
| 401 | No autenticado |
| 403 | No autorizado (autenticado pero sin permiso) |
| 404 | Recurso no encontrado |
| 409 | Conflicto (ej: email ya registrado) |
| 422 | Datos válidos pero no procesables (ej: turno en fecha pasada) |
| 500 | Error interno del servidor |

---

## Output de esta Sección

Al completar A, B, C, D y E tenés el material para las **Secciones 5, 6 y 10** del template de prompt (Backend, Frontend, Requisitos No Funcionales).
