# Exploración de Agentes de IA Preconstruidos en Microsoft 365 Copilot y Comparación de Comportamientos

## 1. Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 30 minutos |
| **Complejidad** | Fácil |
| **Nivel Bloom** | Aplicar |
| **Módulo** | 1 – Fundamentos de Agentes de IA |
| **Lección asociada** | 1.1 – Diferencia entre Chatbot, Asistente y Agente Inteligente |

---

## 2. Descripción General

En este laboratorio explorarás cuatro agentes de IA preconstruidos —Copilot en Word, Copilot en Teams, Copilot en Outlook y ChatGPT GPT-4o— ejecutando un conjunto estandarizado de cinco tareas de prueba en cada uno. Observarás y documentarás las diferencias de comportamiento en cuanto a contexto, memoria, herramientas, razonamiento y autonomía. Los hallazgos se registrarán en una tabla comparativa dentro de tu workspace Notion, generando un artefacto de referencia que utilizarás en los laboratorios del Módulo 2.

---

## 3. Objetivos de Aprendizaje

Al completar este laboratorio serás capaz de:

- [ ] Identificar y diferenciar el comportamiento funcional de al menos tres agentes de IA preconstruidos (Copilot en Word, Copilot en Teams, Copilot en Outlook y ChatGPT GPT-4o).
- [ ] Clasificar cada agente explorado según la taxonomía de tipos de agentes (reactivo, asistente, autónomo) con base en evidencia observada.
- [ ] Documentar en la bitácora Notion las diferencias clave de comportamiento utilizando los cinco atributos: contexto, memoria, herramientas, razonamiento y autonomía.
- [ ] Relacionar los casos de uso observados con escenarios empresariales reales relevantes para tu contexto profesional.

---

## 4. Prerrequisitos

### Conocimientos previos

| Concepto | Descripción |
|----------|-------------|
| Diferencia Chatbot / Asistente / Agente | Lectura completada de la Lección 1.1 (temas 1.1 a 1.5 del Módulo 1) |
| Navegación básica en Microsoft 365 | Saber abrir Word, Teams y Outlook desde el portal office.com o aplicaciones de escritorio |
| Uso básico de Notion | Saber duplicar páginas y editar tablas en Notion |

### Accesos requeridos

| Recurso | Detalle |
|---------|---------|
| Cuenta Microsoft 365 | `usuario[N]@labagentes[N].onmicrosoft.com` con licencia E3 + Copilot activa |
| Microsoft Teams Desktop | Versión 24046.2813.2866.2461, sesión iniciada con cuenta del tenant |
| ChatGPT GPT-4o | Cuenta Plus o compartida provista por el instructor |
| Notion | Workspace `IA-Agentes-Lab-Workspace` duplicado en cuenta personal |
| Navegador | Google Chrome 124+ o Microsoft Edge 124+ |

---

## 5. Entorno de Laboratorio

### Hardware mínimo

| Componente | Requisito |
|------------|-----------|
| Procesador | 64 bits – Intel Core i5 8ª gen. o AMD Ryzen 5 3000+ |
| RAM | 8 GB mínimo (16 GB recomendado) |
| Almacenamiento libre | 2 GB |
| Conexión a internet | 10 Mbps mínimo estable |

### Software requerido

| Aplicación | Versión | Propósito |
|------------|---------|-----------|
| Microsoft Word (M365) | Canal actual 2404 | Explorar Copilot en Word |
| Microsoft Teams Desktop | 24046.2813.2866.2461 | Explorar Copilot en Teams |
| Microsoft Outlook (M365) | Canal actual 2404 | Explorar Copilot en Outlook |
| ChatGPT Web | GPT-4o (mayo 2024) | Agente de referencia externo |
| Notion Web App | 2.2.0 | Documentación de hallazgos |
| Google Chrome / Edge | 124.x | Acceso a servicios web |

### Preparación inicial del entorno

1. Verifica que puedes acceder al portal Microsoft 365 en [https://www.office.com](https://www.office.com) con tus credenciales del tenant de práctica.
2. Abre Microsoft Teams Desktop y confirma que el ícono de Copilot aparece en la barra lateral izquierda.
3. Abre Notion en el navegador y navega a tu copia personal del workspace `IA-Agentes-Lab-Workspace`. Localiza la página **Bitácora-Lab-01**.

---

## 6. Instrucciones Paso a Paso

### Paso 1: Preparar la bitácora de documentación en Notion

**Objetivo:** Configurar la página de registro donde documentarás todas las observaciones del laboratorio.

**Instrucciones:**

1. Abre tu navegador y accede a [https://www.notion.so](https://www.notion.so).
2. Navega al workspace duplicado `IA-Agentes-Lab-Workspace`.
3. Abre la página **Bitácora-Lab-01**.
4. Verifica que la página contiene las siguientes secciones pre-creadas:
   - **Tabla de Tareas de Prueba** (5 filas con las tareas estandarizadas)
   - **Tabla Comparativa de Agentes** (4 columnas: Word, Teams, Outlook, ChatGPT)
   - **Notas de Observación** (área de texto libre)
5. En la sección superior de la página, completa los campos de encabezado:
   - **Nombre del participante:** [Tu nombre completo]
   - **Fecha:** [Fecha actual]
   - **Hora de inicio:** [Hora actual]
6. Revisa las **cinco tareas de prueba estandarizadas** que ejecutarás en cada agente:

| # | Tarea | Categoría |
|---|-------|-----------|
| T1 | "Redacta un correo profesional para solicitar una reunión con el equipo de ventas para discutir los resultados del Q1" | Generación de contenido |
| T2 | "¿Cuáles fueron los temas principales de mi última reunión de equipo?" | Recuperación de información |
| T3 | "Dado que nuestro presupuesto se redujo un 15% y tenemos 3 proyectos pendientes, ¿cuál debería priorizar?" | Razonamiento sobre contexto |
| T4 | "Necesito ayuda con lo del proyecto" (solicitud deliberadamente ambigua) | Manejo de ambigüedad |
| T5 | "Agenda una reunión para mañana a las 10am con María García del departamento de finanzas" | Ejecución con herramientas |

**Resultado esperado:** La página Bitácora-Lab-01 está abierta, personalizada con tus datos y lista para recibir las observaciones de cada interacción.

**Verificación:** Confirma que puedes editar la tabla comparativa agregando texto en al menos una celda. Si la página está en modo solo lectura, estás trabajando sobre el workspace original del instructor — regresa y utiliza tu copia personal.

---

### Paso 2: Explorar Copilot en Microsoft Word

**Objetivo:** Ejecutar las cinco tareas de prueba en Copilot dentro de Word y registrar el comportamiento observado.

**Instrucciones:**

1. Abre Microsoft Word desde el portal [office.com](https://www.office.com) o desde la aplicación de escritorio.
2. Crea un nuevo documento en blanco.
3. Activa Copilot haciendo clic en el ícono de **Copilot** en la cinta de opciones (pestaña *Inicio*). Se abrirá el panel lateral de Copilot.
4. Ejecuta la **Tarea T1** — escribe en el panel de Copilot:
   ```
   Redacta un correo profesional para solicitar una reunión con el equipo de ventas para discutir los resultados del Q1
   ```
5. Observa y registra en Notion:
   - ¿Generó contenido directamente en el documento o solo en el panel?
   - ¿Qué tan elaborada fue la respuesta?
   - ¿Ofreció opciones o variantes?
6. Ejecuta la **Tarea T2** — escribe:
   ```
   ¿Cuáles fueron los temas principales de mi última reunión de equipo?
   ```
7. Observa: ¿Puede acceder a información fuera del documento actual? Registra la respuesta (probablemente indicará que no tiene acceso a esa información desde Word).
8. Ejecuta la **Tarea T3** — escribe:
   ```
   Dado que nuestro presupuesto se redujo un 15% y tenemos 3 proyectos pendientes, ¿cuál debería priorizar?
   ```
9. Observa: ¿Pide más contexto? ¿Razona con la información limitada? ¿Genera un análisis estructurado?
10. Ejecuta la **Tarea T4** — escribe:
    ```
    Necesito ayuda con lo del proyecto
    ```
11. Observa: ¿Cómo maneja la ambigüedad? ¿Pide clarificación o asume un contexto?
12. Ejecuta la **Tarea T5** — escribe:
    ```
    Agenda una reunión para mañana a las 10am con María García del departamento de finanzas
    ```
13. Observa: ¿Puede ejecutar esta acción o indica que no tiene esa capacidad?
14. En tu bitácora Notion, completa la columna **"Copilot en Word"** de la tabla comparativa con tus observaciones para cada tarea.

**Resultado esperado:** Has identificado que Copilot en Word se especializa en generación y edición de contenido dentro del documento, tiene contexto limitado al documento actual, no accede a información de otras aplicaciones de M365 y no puede ejecutar acciones externas como agendar reuniones.

**Verificación:** Tu columna "Copilot en Word" en Notion tiene al menos una observación por cada una de las 5 tareas. Toma una captura de pantalla del panel de Copilot mostrando al menos una respuesta y guárdala como evidencia.

---

### Paso 3: Explorar Copilot en Microsoft Teams

**Objetivo:** Ejecutar las cinco tareas de prueba en Copilot dentro de Teams y registrar las diferencias de comportamiento respecto a Word.

**Instrucciones:**

1. Abre Microsoft Teams Desktop (versión 24046.2813.2866.2461).
2. En la barra lateral izquierda, haz clic en el ícono de **Copilot** (ícono con forma de chispa/estrella).
3. Se abrirá la ventana de chat con Copilot en Teams.
4. Ejecuta la **Tarea T1** — escribe:
   ```
   Redacta un correo profesional para solicitar una reunión con el equipo de ventas para discutir los resultados del Q1
   ```
5. Observa y registra:
   - ¿Genera el contenido de manera similar a Word?
   - ¿Ofrece enviarlo directamente o solo genera texto?
   - ¿Menciona contactos reales de tu organización?
6. Ejecuta la **Tarea T2** — escribe:
   ```
   ¿Cuáles fueron los temas principales de mi última reunión de equipo?
   ```
7. Observa: ¿Accede a transcripciones de reuniones anteriores? ¿Muestra resúmenes de chats recientes? Este es un punto clave de diferenciación con Word.
8. Ejecuta la **Tarea T3** — escribe:
   ```
   Dado que nuestro presupuesto se redujo un 15% y tenemos 3 proyectos pendientes, ¿cuál debería priorizar?
   ```
9. Observa: ¿Intenta buscar información en documentos compartidos o chats? ¿Su razonamiento es diferente al de Word?
10. Ejecuta la **Tarea T4** — escribe:
    ```
    Necesito ayuda con lo del proyecto
    ```
11. Observa: ¿Hace referencia a proyectos mencionados en chats recientes? ¿Pide clarificación de manera diferente?
12. Ejecuta la **Tarea T5** — escribe:
    ```
    Agenda una reunión para mañana a las 10am con María García del departamento de finanzas
    ```
13. Observa: ¿Puede acceder al calendario? ¿Ofrece crear la reunión o solo sugiere cómo hacerlo?
14. En tu bitácora Notion, completa la columna **"Copilot en Teams"** de la tabla comparativa.

**Resultado esperado:** Has identificado que Copilot en Teams tiene un contexto más amplio (acceso a chats, reuniones, archivos compartidos), puede recuperar información de conversaciones previas, y potencialmente interactuar con el calendario, mostrando mayor integración con herramientas organizacionales que Copilot en Word.

**Verificación:** Compara mentalmente las respuestas de Teams vs. Word para la Tarea T2. Deberías notar una diferencia significativa en la capacidad de recuperar información contextual. Registra esta diferencia explícitamente en tus notas.

---

### Paso 4: Explorar Copilot en Microsoft Outlook

**Objetivo:** Ejecutar las cinco tareas de prueba en Copilot dentro de Outlook y observar su especialización en gestión de correo electrónico.

**Instrucciones:**

1. Abre Microsoft Outlook desde el portal [outlook.office.com](https://outlook.office.com) o la aplicación de escritorio.
2. Localiza el ícono de **Copilot** en la interfaz de Outlook. Puede aparecer:
   - En la barra superior al redactar un nuevo correo ("Borrador con Copilot")
   - En el panel lateral al leer un correo largo ("Resumir")
   - En la barra de herramientas principal
3. Inicia una nueva conversación con Copilot (haz clic en el ícono de Copilot en la barra principal de Outlook).
4. Ejecuta la **Tarea T1** — escribe:
   ```
   Redacta un correo profesional para solicitar una reunión con el equipo de ventas para discutir los resultados del Q1
   ```
5. Observa y registra:
   - ¿Genera el correo listo para enviar?
   - ¿Sugiere destinatarios basándose en contactos existentes?
   - ¿Ofrece insertar el borrador directamente en un nuevo mensaje?
6. Ejecuta la **Tarea T2** — escribe:
   ```
   ¿Cuáles fueron los temas principales de mi última reunión de equipo?
   ```
7. Observa: ¿Busca en correos relacionados con reuniones? ¿Tiene acceso a información de Teams desde Outlook?
8. Ejecuta la **Tarea T3** — escribe:
   ```
   Dado que nuestro presupuesto se redujo un 15% y tenemos 3 proyectos pendientes, ¿cuál debería priorizar?
   ```
9. Observa: ¿Intenta buscar correos relevantes sobre presupuesto o proyectos?
10. Ejecuta la **Tarea T4** — escribe:
    ```
    Necesito ayuda con lo del proyecto
    ```
11. Observa: ¿Hace referencia a correos recientes sobre proyectos? ¿Su manejo de ambigüedad difiere de Word y Teams?
12. Ejecuta la **Tarea T5** — escribe:
    ```
    Agenda una reunión para mañana a las 10am con María García del departamento de finanzas
    ```
13. Observa: ¿Puede interactuar con el calendario desde Outlook? ¿Ofrece crear una invitación de calendario?
14. En tu bitácora Notion, completa la columna **"Copilot en Outlook"** de la tabla comparativa.

**Resultado esperado:** Has identificado que Copilot en Outlook está especializado en el contexto de correo electrónico, puede generar borradores listos para enviar, tiene acceso al historial de correos como fuente de contexto, y potencialmente puede interactuar con el calendario integrado.

**Verificación:** Para la Tarea T1, Copilot en Outlook debería ofrecer una experiencia más fluida hacia el envío real del correo comparado con Word (que solo genera texto en un documento). Anota esta diferencia.

---

### Paso 5: Explorar ChatGPT GPT-4o

**Objetivo:** Ejecutar las mismas cinco tareas en ChatGPT GPT-4o como referencia externa y comparar su comportamiento con los agentes integrados de Microsoft 365.

**Instrucciones:**

1. Abre tu navegador y accede a [https://chat.openai.com](https://chat.openai.com).
2. Inicia sesión con la cuenta ChatGPT Plus provista.
3. Verifica que el modelo seleccionado sea **GPT-4o** (visible en la parte superior de la interfaz).
4. Inicia una **nueva conversación** (clic en "New chat").
5. Ejecuta la **Tarea T1** — escribe:
   ```
   Redacta un correo profesional para solicitar una reunión con el equipo de ventas para discutir los resultados del Q1
   ```
6. Observa y registra:
   - Calidad y extensión de la respuesta
   - ¿Ofrece variantes de tono?
   - ¿Puede enviarlo? (No — no tiene integración con tu correo)
7. Ejecuta la **Tarea T2** — escribe:
   ```
   ¿Cuáles fueron los temas principales de mi última reunión de equipo?
   ```
8. Observa: ChatGPT **no tiene acceso** a tus datos organizacionales. Registra cómo maneja esta limitación. ¿Pide contexto? ¿Inventa información?
9. Ejecuta la **Tarea T3** — escribe:
   ```
   Dado que nuestro presupuesto se redujo un 15% y tenemos 3 proyectos pendientes, ¿cuál debería priorizar?
   ```
10. Observa: ¿Cómo razona sin contexto específico? ¿Ofrece un marco de decisión genérico? ¿Pide más detalles?
11. Ejecuta la **Tarea T4** — escribe:
    ```
    Necesito ayuda con lo del proyecto
    ```
12. Observa: ¿Cómo maneja la ambigüedad comparado con los Copilots de M365? ¿Hace preguntas de clarificación?
13. Ejecuta la **Tarea T5** — escribe:
    ```
    Agenda una reunión para mañana a las 10am con María García del departamento de finanzas
    ```
14. Observa: ChatGPT no puede ejecutar esta acción. ¿Cómo comunica esta limitación? ¿Ofrece alternativas?
15. En tu bitácora Notion, completa la columna **"ChatGPT GPT-4o"** de la tabla comparativa.

**Resultado esperado:** Has identificado que ChatGPT GPT-4o tiene excelente capacidad de generación de contenido y razonamiento, pero carece de acceso a datos organizacionales y no puede ejecutar acciones en sistemas externos. Su manejo de ambigüedad tiende a ser más sofisticado (preguntas de clarificación elaboradas) pero opera sin contexto empresarial.

**Verificación:** La diferencia más notable debería ser en T2 (recuperación de información) donde ChatGPT no puede acceder a datos reales mientras que Copilot en Teams sí puede. Confirma que esta observación está documentada.

---

### Paso 6: Completar la tabla comparativa y clasificación

**Objetivo:** Sintetizar las observaciones en una tabla comparativa estructurada y clasificar cada agente según la taxonomía de la Lección 1.1.

**Instrucciones:**

1. Regresa a tu página **Bitácora-Lab-01** en Notion.
2. Navega a la sección **Tabla Comparativa de Agentes**.
3. Completa la tabla con el siguiente formato para cada agente:

| Atributo | Copilot Word | Copilot Teams | Copilot Outlook | ChatGPT GPT-4o |
|----------|--------------|---------------|-----------------|-----------------|
| **Contexto** | [Alcance del contexto disponible] | | | |
| **Memoria** | [¿Recuerda entre interacciones?] | | | |
| **Herramientas** | [¿Qué herramientas puede usar?] | | | |
| **Razonamiento** | [Calidad del razonamiento observado] | | | |
| **Autonomía** | [Nivel: Nula/Baja/Media/Alta] | | | |

4. Debajo de la tabla, agrega una sección titulada **"Clasificación según Taxonomía"** y clasifica cada agente:
   - **Reactivo:** Opera solo cuando se le solicita, sin memoria ni planificación.
   - **Asistente:** Comprende lenguaje natural, mantiene contexto conversacional, genera contenido pero no ejecuta acciones autónomas.
   - **Autónomo:** Planifica, usa herramientas y ejecuta acciones con mínima supervisión.

5. Para cada agente, escribe una justificación de 2-3 oraciones explicando por qué lo clasificaste de esa manera. Ejemplo:

   > **Copilot en Word — Clasificación: Asistente**
   > Comprende lenguaje natural y genera contenido de alta calidad. Su contexto está limitado al documento actual. No ejecuta acciones fuera de Word ni accede a herramientas externas, por lo que su autonomía es baja.

6. En la sección **"Notas de Observación"**, redacta un párrafo de 3-5 oraciones identificando al menos un escenario empresarial real de tu contexto profesional donde cada tipo de agente sería la opción más adecuada.

7. Registra la **hora de finalización** en el encabezado de la bitácora.

**Resultado esperado:** Tu bitácora contiene:
- Tabla comparativa completa con los 5 atributos × 4 agentes (20 celdas completadas).
- Clasificación taxonómica justificada para cada agente.
- Notas de aplicación empresarial.

**Verificación:** Revisa que ninguna celda de la tabla esté vacía. Cada clasificación debe tener una justificación basada en evidencia observada (no en suposiciones teóricas).

---

## 7. Validación y Pruebas

Para considerar este laboratorio completado exitosamente, verifica los siguientes criterios:

| # | Criterio de validación | Cumple (✓/✗) |
|---|------------------------|:---:|
| 1 | Se ejecutaron las 5 tareas de prueba en Copilot Word y se registraron observaciones | |
| 2 | Se ejecutaron las 5 tareas de prueba en Copilot Teams y se registraron observaciones | |
| 3 | Se ejecutaron las 5 tareas de prueba en Copilot Outlook y se registraron observaciones | |
| 4 | Se ejecutaron las 5 tareas de prueba en ChatGPT GPT-4o y se registraron observaciones | |
| 5 | La tabla comparativa tiene las 20 celdas (5 atributos × 4 agentes) completadas | |
| 6 | Cada agente tiene una clasificación taxonómica con justificación de 2-3 oraciones | |
| 7 | Se identificó al menos un escenario empresarial real para cada tipo de agente | |
| 8 | Se tomó al menos una captura de pantalla como evidencia de interacción | |

**Resultado esperado de clasificación típica:**

| Agente | Clasificación esperada |
|--------|----------------------|
| Copilot en Word | Asistente (especializado en redacción) |
| Copilot en Teams | Asistente con rasgos de agente (acceso a herramientas organizacionales) |
| Copilot en Outlook | Asistente con rasgos de agente (integración con calendario/correo) |
| ChatGPT GPT-4o | Asistente (potente en razonamiento pero sin acceso a herramientas externas en modo básico) |

> **Nota:** Las clasificaciones pueden variar según la versión exacta de Copilot y las funcionalidades habilitadas en tu tenant. Lo importante es que la justificación sea coherente con el comportamiento observado.

---

## 8. Solución de Problemas

### Problema 1: El ícono de Copilot no aparece en Word, Teams u Outlook

**Síntomas:** Al abrir la aplicación de Microsoft 365, no se visualiza el ícono de Copilot en la cinta de opciones (Word), la barra lateral (Teams) o la barra de herramientas (Outlook).

**Causa:** La licencia de Microsoft 365 Copilot no se ha propagado correctamente a la cuenta del usuario, o la aplicación está usando una versión anterior al canal actual 2404 que no incluye la integración de Copilot.

**Solución:**
1. Verifica tu licencia accediendo a [https://portal.office.com/account](https://portal.office.com/account) → *Suscripciones*. Debe aparecer "Microsoft 365 Copilot" en la lista.
2. Si la licencia aparece pero el ícono no se muestra, cierra completamente la aplicación y vuelve a abrirla.
3. En aplicaciones de escritorio, ve a *Archivo > Cuenta* y verifica que la versión sea 2404 o superior. Si es anterior, haz clic en *Opciones de actualización > Actualizar ahora*.
4. Si el problema persiste, usa la versión web (word.cloud.microsoft, outlook.office.com) donde Copilot se activa automáticamente con la licencia correcta.
5. Contacta al instructor si después de estos pasos Copilot sigue sin aparecer — puede ser necesario que el administrador del tenant verifique la asignación de licencias.

---

### Problema 2: ChatGPT no permite seleccionar el modelo GPT-4o

**Síntomas:** Al acceder a chat.openai.com, el selector de modelo solo muestra GPT-3.5 o no permite cambiar a GPT-4o. Las respuestas son notablemente menos elaboradas de lo esperado.

**Causa:** La cuenta de ChatGPT no tiene suscripción Plus activa, o se agotó el límite de uso de GPT-4o en el período actual (OpenAI aplica límites de mensajes por hora para usuarios Plus).

**Solución:**
1. Verifica tu plan accediendo a *Settings > Subscription* en la interfaz de ChatGPT. Debe indicar "Plus" o "Team".
2. Si la cuenta es la compartida por el instructor, verifica que estás usando las credenciales correctas (no tu cuenta personal gratuita).
3. Si ves el mensaje "You've reached the GPT-4o limit", espera 1 hora para que se restablezca el límite, o solicita al instructor credenciales alternativas.
4. Como alternativa temporal para completar el laboratorio, puedes usar GPT-4o mini (si está disponible) documentando en tus notas que usaste un modelo diferente y cualquier diferencia de comportamiento observada.

---

## 9. Limpieza

Este laboratorio no genera recursos que requieran eliminación significativa. Realiza las siguientes acciones de orden:

1. **Documento de Word:** El documento en blanco creado en el Paso 2 puede eliminarse o renombrarse como `Lab-01-Exploración-Copilot-Word` si deseas conservarlo como referencia.
2. **Conversación de ChatGPT:** Puedes renombrar la conversación como "Lab 01 - Exploración de Agentes" para fácil referencia futura, o eliminarla si no la necesitas.
3. **Capturas de pantalla:** Guarda cualquier captura tomada durante el laboratorio en una carpeta local organizada (por ejemplo, `~/LabAgentes/capturas/lab-01/` o `C:/LabAgentes/capturas/lab-01/`).
4. **Notion:** No elimines la Bitácora-Lab-01 — será referencia para los laboratorios del Módulo 2.

---

## 10. Resumen

### Lo que aprendiste

En este laboratorio aplicaste los conceptos teóricos de la Lección 1.1 interactuando directamente con cuatro agentes de IA preconstruidos. Comprobaste de primera mano que:

- **El contexto disponible** varía drásticamente según la plataforma: Copilot en Teams accede a conversaciones y reuniones, Copilot en Word solo al documento actual, y ChatGPT no tiene acceso a datos organizacionales.
- **La capacidad de acción** (uso de herramientas) es lo que más diferencia a un asistente de un agente: generar texto no es lo mismo que ejecutar una tarea.
- **El manejo de ambigüedad** revela la sofisticación del sistema: los mejores piden clarificación contextualizada en lugar de asumir o fallar.
- **La autonomía** en los agentes preconstruidos actuales es todavía limitada, pero los Copilots de M365 muestran rasgos de agente al integrar múltiples fuentes de información y ofrecer acciones.

### Conexión con los próximos laboratorios

Los hallazgos documentados en tu tabla comparativa servirán como referencia cuando, en los laboratorios del Módulo 2 (02-00-01 a 02-00-04), diseñes y construyas tu propio agente en Microsoft Copilot Studio. Podrás definir conscientemente qué nivel de contexto, memoria, herramientas y autonomía quieres otorgarle a tu agente, basándote en lo que observaste que funciona (y lo que no) en los agentes preconstruidos.

### Recursos adicionales

- [Microsoft — Documentación oficial de Copilot en Microsoft 365](https://learn.microsoft.com/es-es/copilot/microsoft-365/)
- [OpenAI — Guía de uso de ChatGPT](https://help.openai.com/en/collections/3742473-chatgpt)
- [Russell & Norvig — Capítulo de Agentes Inteligentes (referencia académica)](https://aima.cs.berkeley.edu/)
- [Microsoft — Comparativa de capacidades de Copilot por aplicación](https://adoption.microsoft.com/es-es/copilot/)

---
