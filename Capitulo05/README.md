# 5 Laboratorio: Uso de métricas para medir desempeño en agentes

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 25 minutos |
| **Complejidad** | Fácil |
| **Nivel Bloom** | Aplicar |
| **Módulo** | 5 — Riesgos, métricas y mejora continua |
| **Entorno** | LabPractice-M2 |

## Descripción General

En este laboratorio accederás al panel de Analytics de Microsoft Copilot Studio para extraer métricas clave de desempeño del agente **AgenteEmpresarialLab** construido en el módulo 4. Ejecutarás un protocolo de pruebas de 10 preguntas para detectar alucinaciones y respuestas incorrectas, documentarás los hallazgos en un archivo Excel Online estructurado, e implementarás al menos una mejora concreta al agente basada en los datos analizados. El laboratorio concluye con un plan de mejora continua de 3 acciones priorizadas.

## Objetivos de Aprendizaje

- [ ] Identificar y extraer métricas clave (resolution rate, escalation rate, engagement rate, sesiones con errores) desde el panel de Analytics de Copilot Studio.
- [ ] Analizar indicadores de calidad para detectar temas con mayor tasa de abandono y áreas de mejora.
- [ ] Detectar y documentar al menos dos instancias de comportamiento incorrecto o alucinaciones mediante un protocolo de pruebas estructurado.
- [ ] Implementar al menos una mejora concreta al agente y verificar su impacto mediante re-ejecución de pruebas.
- [ ] Elaborar un plan de mejora continua con 3 acciones priorizadas documentado en formato estructurado.

## Prerrequisitos

### Conocimientos previos

| Requisito | Referencia |
|-----------|-----------|
| Conceptos de alucinaciones en IA generativa | Lección 5.1 — Riesgos Comunes en Agentes con IA Generativa |
| Categorías de riesgo: calidad, seguridad, operativos, éticos | Lección 5.1 — Tabla de categorías |
| Métricas de desempeño en agentes conversacionales | Lecciones 5.2, 5.3 y 5.4 |
| Edición de temas en Copilot Studio | Módulo 4 — Laboratorios 04-00-01 a 04-00-04 |

### Acceso y recursos necesarios

| Recurso | Detalle |
|---------|---------|
| Agente **AgenteEmpresarialLab** | Completado en Lab 04-00-04, con al menos 15 sesiones registradas |
| Plantilla Excel | `MetricasAgente_Plantilla.xlsx` disponible en SharePoint del curso (`/LabFiles/Lab05/`) |
| Protocolo de pruebas | `ProtocoloPruebas.docx` disponible en `/LabFiles/Lab05/` |
| Credenciales Microsoft 365 | `usuario[N]@labagentes[N].onmicrosoft.com` con licencia Copilot |
| Entorno Copilot Studio | `LabPractice-M2` (Sandbox, región United States) |

## Entorno del Laboratorio

### Hardware mínimo

| Componente | Especificación |
|------------|---------------|
| Procesador | Intel Core i5 8ª gen. / AMD Ryzen 5 3000+ (64 bits) |
| RAM | 8 GB mínimo (16 GB recomendado) |
| Almacenamiento libre | 2 GB |
| Conexión a internet | 10 Mbps mínimo |

### Software requerido

| Herramienta | Versión | Propósito |
|-------------|---------|-----------|
| Microsoft Edge / Google Chrome | 124.x+ | Navegador principal |
| Microsoft Copilot Studio | Web GA — Release Wave 1 2024 | Panel de Analytics y edición de temas |
| Microsoft Excel Online | Cloud 2023.12 | Registro de métricas y plan de mejora |
| OneDrive for Business | Cloud | Almacenamiento de entregables |
| SharePoint Online | Cloud | Acceso a plantillas del curso |

### Preparación inicial

Antes de comenzar, verifica que puedes acceder a los siguientes recursos:

1. Abre el navegador y navega a: `https://copilotstudio.microsoft.com/environments/LabPractice-M2`
2. Confirma que el agente **AgenteEmpresarialLab** aparece en la lista de agentes.
3. Abre una segunda pestaña con OneDrive: `https://onedrive.live.com` (o acceso corporativo).
4. Crea la carpeta `/LabFiles/Lab05/` en tu OneDrive si no existe.

---

## Paso a Paso

### Paso 1: Acceder al Panel de Analytics del Agente

**Objetivo:** Navegar al panel de métricas de desempeño del agente AgenteEmpresarialLab y familiarizarse con los indicadores disponibles.

**Instrucciones:**

1. En Microsoft Copilot Studio (`https://copilotstudio.microsoft.com/environments/LabPractice-M2`), haz clic en el agente **AgenteEmpresarialLab** para abrirlo.
2. En el menú lateral izquierdo, haz clic en **Analytics** (icono de gráfico de barras).
3. En la parte superior del panel, ajusta el rango de fechas para cubrir los últimos **7 días** (o el período que incluya las 15+ sesiones de los laboratorios anteriores).
4. Observa las pestañas disponibles: **Summary**, **Customer Satisfaction**, **Sessions**, y **Topics**.
5. Haz clic en la pestaña **Summary** para visualizar la vista general de métricas.

**Resultado esperado:**

El panel muestra un dashboard con las siguientes métricas visibles:
- **Total sessions** (número total de sesiones)
- **Engagement rate** (porcentaje de sesiones donde el usuario interactuó con un tema)
- **Resolution rate** (porcentaje de sesiones resueltas sin escalación)
- **Escalation rate** (porcentaje de sesiones escaladas a un agente humano)
- **Abandon rate** (porcentaje de sesiones abandonadas)

**Verificación:**

- [ ] El panel de Analytics carga correctamente y muestra datos (no está vacío).
- [ ] El número total de sesiones es ≥ 15.
- [ ] Las cuatro métricas principales (engagement, resolution, escalation, abandon) son visibles con valores numéricos.

---

### Paso 2: Extraer y Registrar Métricas Generales

**Objetivo:** Documentar las métricas clave en la hoja 'Métricas Generales' del archivo Excel Online.

**Instrucciones:**

1. Desde SharePoint del curso (`/LabFiles/Lab05/`), descarga o copia el archivo `MetricasAgente_Plantilla.xlsx` a tu carpeta personal de OneDrive en `/LabFiles/Lab05/`.
2. Renombra el archivo siguiendo la convención: `MetricasAgente_[TuNombre].xlsx` (ejemplo: `MetricasAgente_JuanGarcia.xlsx`).
3. Abre el archivo en **Excel Online**.
4. Navega a la hoja **Métricas Generales**.
5. Registra los siguientes datos extraídos del panel de Analytics:

| Celda | Métrica | Fuente en Copilot Studio |
|-------|---------|--------------------------|
| B2 | Total de sesiones | Summary → Total sessions |
| B3 | Engagement rate (%) | Summary → Engagement rate |
| B4 | Resolution rate (%) | Summary → Resolution rate |
| B5 | Escalation rate (%) | Summary → Escalation rate |
| B6 | Abandon rate (%) | Summary → Abandon rate |
| B7 | Sesiones con errores | Sessions → filtrar por "Unresolved" |
| B8 | Fecha de extracción | Fecha actual (formato DD/MM/AAAA) |

6. Para obtener las **sesiones con errores**, haz clic en la pestaña **Sessions** en el panel de Analytics. Filtra por el estado **Unresolved** o **Escalated** y registra el conteo.
7. Guarda el archivo (se guarda automáticamente en Excel Online).

**Resultado esperado:**

La hoja 'Métricas Generales' queda completada con datos reales del agente. Ejemplo de valores típicos:

```
Total de sesiones:       18
Engagement rate:         78%
Resolution rate:         61%
Escalation rate:         17%
Abandon rate:            22%
Sesiones con errores:    4
Fecha de extracción:     15/06/2024
```

**Verificación:**

- [ ] Todas las celdas B2 a B8 contienen valores (ninguna está vacía).
- [ ] Los porcentajes suman lógicamente (Resolution + Escalation + Abandon ≈ 100% del engagement).
- [ ] El archivo está guardado en `/LabFiles/Lab05/` de tu OneDrive.

---

### Paso 3: Analizar Temas y Detectar Áreas de Mejora

**Objetivo:** Identificar los temas más activados y aquellos con mayor tasa de abandono para priorizar mejoras.

**Instrucciones:**

1. En el panel de Analytics de Copilot Studio, haz clic en la pestaña **Topics**.
2. Observa la tabla de temas que muestra: nombre del tema, número de sesiones, resolution rate por tema, escalation rate por tema y abandon rate por tema.
3. Ordena la tabla por **Abandon rate** de mayor a menor (haz clic en el encabezado de la columna).
4. Identifica los **3 temas con mayor tasa de abandono**.
5. Identifica los **3 temas más activados** (mayor número de sesiones).
6. En tu archivo Excel Online, navega a la hoja **Análisis de Temas**.
7. Registra la siguiente información para cada tema relevante:

| Columna A | Columna B | Columna C | Columna D | Columna E | Columna F |
|-----------|-----------|-----------|-----------|-----------|-----------|
| Nombre del tema | Sesiones | Resolution rate | Escalation rate | Abandon rate | Observaciones |

8. Completa al menos **6 filas** (los 3 más activados + los 3 con mayor abandono; pueden solaparse).
9. En la columna **Observaciones**, anota hipótesis sobre por qué cada tema tiene alto abandono. Por ejemplo:
   - "Falta mensaje de clarificación cuando el usuario no proporciona suficiente información"
   - "Las frases disparadoras son demasiado específicas y no capturan variantes"
   - "El tema no tiene ruta de fallback definida"

**Resultado esperado:**

La hoja 'Análisis de Temas' contiene al menos 6 filas con datos reales. Se identifica claramente cuál es el tema con mayor tasa de abandono (candidato principal para mejora).

**Verificación:**

- [ ] Se registraron al menos 6 temas con sus métricas completas.
- [ ] Se identificó el tema con mayor tasa de abandono (marcado o resaltado en la hoja).
- [ ] Cada tema tiene al menos una observación/hipótesis en la columna F.

---

### Paso 4: Ejecutar Protocolo de Pruebas para Detectar Alucinaciones

**Objetivo:** Ejecutar 10 preguntas predefinidas contra el agente para detectar respuestas incorrectas, alucinaciones o comportamientos fuera de alcance.

**Instrucciones:**

1. Abre el archivo `ProtocoloPruebas.docx` desde SharePoint (`/LabFiles/Lab05/`). Este documento contiene 10 preguntas de prueba con sus **respuestas esperadas**.
2. En Microsoft Copilot Studio, con el agente **AgenteEmpresarialLab** abierto, haz clic en **Test your copilot** (panel de pruebas lateral derecho, icono de chat).
3. Si el panel no está visible, haz clic en el botón **Test** en la esquina inferior izquierda.
4. Ejecuta cada una de las 10 preguntas del protocolo, una por una:
   - Escribe la pregunta exactamente como aparece en el protocolo.
   - Espera la respuesta completa del agente.
   - Compara la respuesta obtenida con la respuesta esperada del protocolo.
   - Clasifica el resultado: **Correcta**, **Parcialmente correcta**, o **Incorrecta**.
5. Para cada respuesta **incorrecta** o **parcialmente correcta**, determina el tipo de error:
   - **Alucinación**: el agente inventó información que no existe en sus fuentes de conocimiento.
   - **Respuesta fuera de scope**: el agente respondió sobre un tema que no debería manejar.
   - **Error de conector**: el agente no pudo acceder a la fuente de datos correctamente.
   - **Información desactualizada**: la respuesta se basa en datos obsoletos.
   - **Respuesta incompleta**: falta información crítica en la respuesta.
6. Navega a la hoja **Registro de Alucinaciones** en tu archivo Excel Online.
7. Para cada respuesta incorrecta o parcialmente correcta, registra:

| Col. A | Col. B | Col. C | Col. D | Col. E | Col. F |
|--------|--------|--------|--------|--------|--------|
| # Pregunta | Pregunta textual | Respuesta obtenida | Respuesta esperada | Tipo de error | Severidad (Alta/Media/Baja) |

8. Criterios de severidad:
   - **Alta**: la respuesta podría causar daño financiero, legal o reputacional si se actuara sobre ella.
   - **Media**: la respuesta es incorrecta pero el usuario probablemente detectaría el error.
   - **Baja**: la respuesta es incompleta o imprecisa pero no causa daño directo.

**Resultado esperado:**

Se identifican al menos **2 instancias** de comportamiento incorrecto documentadas en la hoja 'Registro de Alucinaciones'. Ejemplo:

```
Pregunta #3: "¿Cuál es la política de devoluciones para productos electrónicos?"
Respuesta obtenida: "La política permite devoluciones hasta 90 días después de la compra."
Respuesta esperada: "La política permite devoluciones hasta 30 días después de la compra."
Tipo de error: Alucinación
Severidad: Alta
```

```
Pregunta #7: "¿Quién es el director de finanzas?"
Respuesta obtenida: "El director de finanzas es Carlos Mendoza, contacto: cmendoza@empresa.com"
Respuesta esperada: "No tengo información sobre el directorio de la empresa."
Tipo de error: Respuesta fuera de scope
Severidad: Media
```

**Verificación:**

- [ ] Se ejecutaron las 10 preguntas del protocolo completo.
- [ ] Se documentaron al menos 2 respuestas incorrectas con todos los campos completos.
- [ ] Cada registro incluye tipo de error y severidad asignada.
- [ ] Las clasificaciones de severidad son coherentes con los criterios definidos.

---

### Paso 5: Implementar una Mejora Concreta al Agente

**Objetivo:** Aplicar una mejora al tema con mayor tasa de abandono basándose en los datos de métricas y las observaciones del análisis.

**Instrucciones:**

1. Revisa la hoja **Análisis de Temas** e identifica el tema con la mayor tasa de abandono.
2. Revisa tus observaciones/hipótesis para ese tema (columna F).
3. En Copilot Studio, navega a **Topics** en el menú lateral izquierdo.
4. Haz clic en el tema identificado para abrirlo en el editor visual.
5. Implementa **al menos una** de las siguientes mejoras según la hipótesis identificada:

**Opción A — Añadir mensaje de clarificación:**

6a. Localiza el nodo donde el usuario suele abandonar (observa el flujo del tema).
7a. Haz clic en el ícono **+** (agregar nodo) antes del punto de abandono.
8a. Selecciona **Ask a question** (Hacer una pregunta).
9a. Escribe un mensaje de clarificación. Ejemplo:

```
"Para poder ayudarte mejor, ¿podrías especificar [detalle necesario]? 
Puedes elegir entre las siguientes opciones:"
```

10a. Agrega opciones de respuesta múltiple que guíen al usuario.

**Opción B — Ampliar frases disparadoras (trigger phrases):**

6b. En la parte superior del editor del tema, haz clic en **Trigger phrases**.
7b. Revisa las frases existentes.
8b. Agrega al menos **5 variantes adicionales** que los usuarios podrían usar. Ejemplo:

```
Frases originales:
- "política de devoluciones"
- "quiero devolver un producto"

Frases nuevas a agregar:
- "cómo devuelvo algo"
- "devolución"
- "regresar un artículo"
- "no quiero el producto"
- "cambio o devolución"
```

**Opción C — Añadir nodo de fallback:**

6c. Al final del flujo del tema, agrega un nodo **Message** con un mensaje de fallback:

```
"Si no pude resolver tu consulta, puedo transferirte a un agente humano 
o puedes reformular tu pregunta de otra manera. ¿Qué prefieres?"
```

7c. Agrega un nodo **Question** con opciones: "Transferir a agente" / "Reformular pregunta".
8c. Conecta "Transferir a agente" a un nodo **Transfer to agent** (si está disponible) o a un mensaje con instrucciones de contacto.

9. Haz clic en **Save** (Guardar) en la esquina superior derecha del editor de temas.

**Resultado esperado:**

El tema modificado ahora tiene un flujo mejorado que aborda la causa raíz del abandono. El cambio es visible en el editor visual del tema.

**Verificación:**

- [ ] El tema se guardó sin errores (no hay indicadores rojos en el editor).
- [ ] La mejora implementada es coherente con la hipótesis registrada en el Paso 3.
- [ ] El flujo del tema muestra el nuevo nodo o las nuevas frases disparadoras.

---

### Paso 6: Verificar el Impacto de la Mejora

**Objetivo:** Re-ejecutar las pruebas relevantes para confirmar que la mejora implementada resuelve o reduce el problema identificado.

**Instrucciones:**

1. En el panel **Test your copilot**, haz clic en el ícono de **Reset** (reiniciar conversación) para limpiar el contexto.
2. Del protocolo de pruebas, identifica las preguntas que están relacionadas con el tema mejorado (generalmente 2-3 preguntas).
3. Ejecuta esas preguntas nuevamente en el panel de pruebas.
4. Evalúa si las respuestas mejoraron:
   - ¿El agente ahora solicita clarificación cuando antes abandonaba?
   - ¿El agente reconoce las nuevas variantes de frases?
   - ¿El usuario tiene una ruta de salida clara en lugar de un punto muerto?
5. Documenta los resultados de la re-prueba en la hoja **Registro de Alucinaciones**, añadiendo una nueva sección al final:

| Col. A | Col. B | Col. C |
|--------|--------|--------|
| # Pregunta (re-test) | Resultado antes de mejora | Resultado después de mejora |

6. Si la mejora no resolvió el problema, documenta por qué y propón una acción adicional.

**Resultado esperado:**

Al menos una de las preguntas re-ejecutadas muestra una mejora observable en la respuesta del agente (mejor claridad, nueva pregunta de clarificación, o reconocimiento de frase que antes no activaba el tema).

**Verificación:**

- [ ] Se re-ejecutaron al menos 2 preguntas relevantes al tema mejorado.
- [ ] Se documentó la comparación antes/después en el archivo Excel.
- [ ] Al menos una pregunta muestra mejora observable en la respuesta.

---

### Paso 7: Elaborar el Plan de Mejora Continua

**Objetivo:** Documentar un plan de 3 acciones priorizadas basado en todo el análisis realizado.

**Instrucciones:**

1. En tu archivo Excel Online, navega a la hoja **Plan de Mejora**.
2. Completa la tabla con exactamente **3 acciones de mejora** priorizadas, usando la siguiente estructura:

| Columna | Contenido |
|---------|-----------|
| A — Prioridad | 1, 2 o 3 (1 = más urgente) |
| B — Área | Calidad / Seguridad / Operativo / Ético (según categorías de riesgo de lección 5.1) |
| C — Hallazgo | Descripción breve del problema detectado |
| D — Acción propuesta | Mejora concreta a implementar |
| E — Métrica de éxito | Cómo se medirá que la mejora funcionó |
| F — Plazo estimado | Tiempo estimado para implementar |
| G — Estado | "Implementada" (si ya se hizo en Paso 5) o "Pendiente" |

3. Criterios de priorización:
   - **Prioridad 1**: Problemas de severidad Alta que afectan la confianza del usuario o generan riesgo legal/financiero.
   - **Prioridad 2**: Problemas de severidad Media que degradan la experiencia pero no causan daño directo.
   - **Prioridad 3**: Mejoras de optimización que incrementan la eficiencia o satisfacción.

4. Ejemplo de plan completado:

```
Prioridad 1 | Calidad | Alucinación en política de devoluciones (pregunta #3) |
  Acción: Vincular respuesta a documento fuente específico vía knowledge source |
  Métrica: Resolution rate del tema "Devoluciones" sube de 45% a 75% |
  Plazo: 2 días | Estado: Pendiente

Prioridad 2 | Operativo | Tema "Consulta de estado" tiene 40% de abandono |
  Acción: Agregar pregunta de clarificación solicitando número de pedido |
  Métrica: Abandon rate del tema baja de 40% a 20% |
  Plazo: 1 día | Estado: Implementada

Prioridad 3 | Ético | Respuestas con diferente nivel de detalle según tipo de consulta |
  Acción: Estandarizar plantillas de respuesta para todos los temas principales |
  Métrica: Varianza en longitud de respuesta < 20% entre temas similares |
  Plazo: 5 días | Estado: Pendiente
```

5. Guarda el archivo final.

**Resultado esperado:**

La hoja 'Plan de Mejora' contiene 3 acciones completas con todos los campos rellenados. Al menos una acción tiene estado "Implementada" (la del Paso 5).

**Verificación:**

- [ ] Exactamente 3 acciones documentadas con prioridad 1, 2 y 3.
- [ ] Cada acción tiene todos los campos completos (A-G).
- [ ] Al menos una acción tiene estado "Implementada".
- [ ] Las áreas de riesgo corresponden a las categorías de la lección 5.1 (Calidad, Seguridad, Operativo, Ético).
- [ ] Las métricas de éxito son medibles y específicas.

---

## Validación y Pruebas Finales

Antes de dar por completado el laboratorio, verifica los siguientes criterios de aceptación:

| # | Criterio | Cumple (✓/✗) |
|---|----------|:---:|
| 1 | Archivo `MetricasAgente_[TuNombre].xlsx` existe en OneDrive `/LabFiles/Lab05/` | |
| 2 | Hoja 'Métricas Generales' tiene todas las celdas B2-B8 completadas | |
| 3 | Hoja 'Análisis de Temas' tiene al menos 6 filas con datos reales | |
| 4 | Hoja 'Registro de Alucinaciones' documenta al menos 2 instancias de error | |
| 5 | Cada registro de error incluye: pregunta, respuesta obtenida, respuesta esperada, tipo de error y severidad | |
| 6 | Se implementó al menos 1 mejora en el editor de temas de Copilot Studio | |
| 7 | Se documentó la comparación antes/después de la mejora | |
| 8 | Hoja 'Plan de Mejora' contiene 3 acciones priorizadas con todos los campos | |
| 9 | Al menos 1 acción del plan tiene estado "Implementada" | |
| 10 | El agente AgenteEmpresarialLab está guardado con los cambios aplicados | |

**Puntuación mínima para aprobar:** 8 de 10 criterios cumplidos.

---

## Solución de Problemas

### Problema 1: El panel de Analytics no muestra datos o aparece vacío

**Síntomas:**
- Al acceder a Analytics, las métricas muestran "0" o "No data available".
- Los gráficos aparecen sin información.
- El mensaje "There isn't enough data to show analytics" se muestra en pantalla.

**Causa:**
El panel de Analytics de Copilot Studio requiere un mínimo de sesiones de conversación y un período de procesamiento de 1-2 horas para que los datos se reflejen. Si las sesiones de prueba del módulo 4 se realizaron hace menos de 2 horas, es posible que los datos aún no estén disponibles. También puede ocurrir si las sesiones se ejecutaron solo en el panel de pruebas (Test) sin publicar el agente.

**Solución:**
1. Verifica que el agente fue **publicado** al menos una vez (botón **Publish** en la esquina superior derecha).
2. Confirma que las conversaciones se realizaron a través de un canal publicado (Teams, sitio web demo) y no solo en el panel de Test (las sesiones del panel Test sí se registran en Analytics, pero con retraso).
3. Si los datos siguen sin aparecer, genera sesiones adicionales ahora: abre el panel de pruebas y ejecuta al menos 5 conversaciones completas (activando diferentes temas y llevándolas hasta un cierre).
4. Espera 30 minutos y refresca el panel de Analytics.
5. Si persiste el problema, ajusta el rango de fechas para incluir **los últimos 30 días** en lugar de 7.
6. Como alternativa temporal: usa la pestaña **Sessions** (que se actualiza más rápido) para contar sesiones manualmente y registra los datos disponibles en Excel con una nota indicando "datos parciales por latencia del panel".

---

### Problema 2: No se puede editar el archivo Excel Online (error de permisos o archivo bloqueado)

**Síntomas:**
- Al abrir `MetricasAgente_Plantilla.xlsx` aparece el mensaje "Read-only" o "You don't have permission to edit".
- El archivo se abre pero no permite escribir en las celdas.
- Aparece un mensaje indicando que otro usuario tiene el archivo abierto.

**Causa:**
El archivo plantilla en SharePoint puede estar configurado como solo lectura, o no se copió correctamente al OneDrive personal del estudiante. También puede ocurrir si otro participante abrió el mismo archivo de plantilla compartido en modo edición.

**Solución:**
1. **No edites el archivo original en SharePoint.** Primero cópialo a tu OneDrive personal:
   - En SharePoint, haz clic derecho sobre `MetricasAgente_Plantilla.xlsx`.
   - Selecciona **Copy to** → **My files** (OneDrive) → navega a `/LabFiles/Lab05/`.
   - Haz clic en **Copy here**.
2. Navega a tu OneDrive, abre la carpeta `/LabFiles/Lab05/` y renombra el archivo copiado a `MetricasAgente_[TuNombre].xlsx`.
3. Abre el archivo renombrado — ahora tendrás permisos completos de edición.
4. Si el problema persiste en tu propia copia:
   - Cierra todas las pestañas del navegador que tengan el archivo abierto.
   - Espera 30 segundos y vuelve a abrir.
   - Si sigue en modo lectura, haz clic en **Edit Workbook** → **Edit in Browser** en la barra superior.
5. Como último recurso, crea un nuevo archivo Excel Online en blanco en tu OneDrive y recrea las 4 hojas manualmente: 'Métricas Generales', 'Análisis de Temas', 'Registro de Alucinaciones' y 'Plan de Mejora'.

---

## Limpieza

Al finalizar el laboratorio:

1. **Guardar el archivo Excel final**: Confirma que `MetricasAgente_[TuNombre].xlsx` está guardado en OneDrive `/LabFiles/Lab05/` con todas las hojas completadas.
2. **No eliminar cambios del agente**: Las mejoras implementadas en el Paso 5 deben permanecer en el agente para referencia futura y evaluación del instructor.
3. **Cerrar panel de pruebas**: Haz clic en la **X** del panel "Test your copilot" para cerrarlo.
4. **Captura de pantalla final**: Toma una captura del panel de Analytics mostrando las métricas y guárdala en `C:/LabAgentes/capturas/` (Windows) o `~/LabAgentes/capturas/` (macOS) con el nombre `lab05-analytics-final.png`.
5. **No cerrar sesión** en Copilot Studio si tienes laboratorios adicionales pendientes.

---

## Resumen

En este laboratorio aplicaste el ciclo completo de medición y mejora de un agente de IA:

| Fase | Actividad realizada |
|------|-------------------|
| **Medición** | Extracción de métricas clave (resolution rate, escalation rate, abandon rate) del panel de Analytics |
| **Análisis** | Identificación de temas problemáticos y formulación de hipótesis sobre causas de abandono |
| **Detección** | Ejecución de protocolo de pruebas estructurado para identificar alucinaciones y errores |
| **Mejora** | Implementación de una corrección concreta en el editor de temas |
| **Verificación** | Re-ejecución de pruebas para medir el impacto de la mejora |
| **Planificación** | Elaboración de plan de mejora continua con 3 acciones priorizadas |

### Conceptos clave reforzados

- Las **alucinaciones** (lección 5.1) se detectan sistemáticamente comparando respuestas del agente contra fuentes de verdad documentadas.
- La **matriz de riesgos** por categoría (Calidad, Seguridad, Operativo, Ético) se aplica directamente en la clasificación de errores y la priorización de mejoras.
- El **principio de mínimo privilegio** y la **trazabilidad** se reflejan en la documentación rigurosa de cada hallazgo y acción.
- El ciclo **Medir → Analizar → Mejorar → Verificar** es la base de la gestión responsable de agentes con IA generativa.

### Recursos adicionales

| Recurso | Enlace |
|---------|--------|
| Documentación oficial de Analytics en Copilot Studio | https://learn.microsoft.com/en-us/microsoft-copilot-studio/analytics-overview |
| OWASP Top 10 para LLMs (referencia de riesgos) | https://owasp.org/www-project-top-10-for-large-language-model-applications/ |
| Microsoft Responsible AI Dashboard | https://www.microsoft.com/en-us/ai/responsible-ai |
| Guía de mejora continua para chatbots empresariales | https://learn.microsoft.com/en-us/microsoft-copilot-studio/analytics-topic-details |

---
