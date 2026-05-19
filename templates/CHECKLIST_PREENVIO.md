# CHECKLIST PRE-ENVÍO
## 16 puntos de verificación antes de enviar el prompt a la IA

> Verificá cada punto. Si fallás en alguno, la IA va a inventar lo que falta.  
> Un prompt incompleto = iteraciones infinitas.

---

## 🟦 BLOQUE 1 — ORIENTACIÓN (4 puntos)

- [ ] **1.1** ¿Describí la app en 2 oraciones máximo, en pasado ("construí..."), sin ambigüedad?

- [ ] **1.2** ¿Definí todos los actores del sistema (usuarios + sistemas externos)?

- [ ] **1.3** ¿El flujo principal de valor tiene 3-5 pasos concretos y verificables?

- [ ] **1.4** ¿Tengo al menos 5 criterios verificables de éxito en la Sección 11?

---

## 🟩 BLOQUE 2 — ARQUITECTURA (4 puntos)

- [ ] **2.1** ¿Especifiqué el stack tecnológico completo con una razón por cada tecnología?

- [ ] **2.2** ¿Incluí el árbol de archivos con comentarios de responsabilidad en cada archivo?

- [ ] **2.3** ¿Incluí el esquema completo de la base de datos (SQL o schema JSON) con tipos, constraints y datos de seed?

- [ ] **2.4** ¿Incluí la tabla completa de endpoints API con método, ruta y descripción?

---

## 🟨 BLOQUE 3 — COMPORTAMIENTO (4 puntos)

- [ ] **3.1** ¿Especifiqué el flujo completo (happy path + errores) de cada feature crítico?

- [ ] **3.2** ¿Definí el manejo de errores con formato de respuesta consistente?

- [ ] **3.3** ¿Especifiqué el comportamiento de la UI (polling, estados de carga, estados vacíos, toasts)?

- [ ] **3.4** ¿Hay código exacto embebido en los puntos de alto riesgo (formatos de API externa, SQL, TwiML, etc.)?

---

## 🟥 BLOQUE 4 — CALIDAD (4 puntos)

- [ ] **4.1** ¿Definí paleta de colores con hexadecimales y nombre exacto de la tipografía?

- [ ] **4.2** ¿Especifiqué el idioma de la UI y el idioma/estilo de los comentarios del código?

- [ ] **4.3** ¿Incluí requisitos no funcionales (CORS, rate limiting, logging, validación)?

- [ ] **4.4** ¿Los criterios de éxito de la Sección 11 son verificables sin ambigüedad?

---

## Puntuación

| Resultado | Acción |
|---|---|
| **16/16** ✅ | Enviá el prompt. Está listo. |
| **12-15** ⚠️ | Completá los puntos que faltan antes de enviar. |
| **< 12** ❌ | No enviés todavía. La IA va a inventar demasiadas cosas. |

---

## Lista Rápida de Antipatrones — Lo que NUNCA debe estar en el prompt

❌ "Hacelo bonito" → Reemplazar con paleta + tipografía + 2 adjetivos de estética  
❌ "Usá la tecnología que prefieras" → Especificar stack completo con razones  
❌ "Organizá el código de forma limpia" → Incluir árbol de directorios con responsabilidades  
❌ Stack sin versión cuando importa → Especificar versión major si la API cambia entre versiones  
❌ Endpoints incompletos → La tabla de API debe tener TODOS los endpoints  
❌ "Manejá los errores" → Especificar formato exacto de respuesta de error  
❌ Features nice-to-have en el prompt → Solo features core en la primera versión  
❌ Descripción en futuro ("voy a hacer...") → Usar pasado ("construí...") para que la IA implemente  
