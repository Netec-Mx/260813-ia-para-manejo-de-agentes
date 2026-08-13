# Creación de un Agente y Elección de Modelo de IA Generativa

## Metadatos del Laboratorio

| Campo | Valor |
|---|---|
| **Duración** | 10 minutos |
| **Complejidad** | Fácil |
| **Nivel Bloom** | Crear |
| **Plataforma principal** | Microsoft Copilot Studio (Release Wave 1 2024) |
| **Entorno** | LabPractice-M2 |

## Descripción General

En este laboratorio crearás tu primer agente de IA desde cero en Microsoft Copilot Studio. El agente se llamará **Nova Assistant** y servirá como asistente de soporte interno para empleados de la empresa ficticia **TechNova Solutions**. Configurarás su nombre, descripción, ícono representativo y seleccionarás GPT-4o como modelo de IA generativa base, justificando esta elección frente a GPT-3.5 Turbo. Este agente será el punto de partida obligatorio para todos los laboratorios subsiguientes del módulo.

## Objetivos de Aprendizaje

Al completar este laboratorio serás capaz de:

- [ ] Crear un agente funcional desde cero en Microsoft Copilot Studio, definiendo nombre, descripción e ícono representativo
- [ ] Evaluar y seleccionar el modelo de IA generativa más adecuado (GPT-4o vs GPT-3.5 Turbo) comparando capacidades de razonamiento, costo y velocidad
- [ ] Identificar las diferencias clave entre plataformas de creación de agentes para justificar la elección de Microsoft Copilot Studio como plataforma para el caso de uso empresarial
- [ ] Navegar la interfaz principal de Copilot Studio y localizar las secciones de configuración del agente

## Prerrequisitos

### Conocimientos Previos

| Requisito | Descripción |
|---|---|
| Lectura completada | Lecciones 3.1 (Panorama de plataformas) y 3.2 (Elección de modelo de IA Gen base) |
| Comprensión básica | Diferencias entre modelos GPT-4o y GPT-3.5 Turbo |
| Familiaridad | Navegación web y gestión de cuentas Microsoft 365 |

### Acceso y Cuentas

| Recurso | Detalle |
|---|---|
| Cuenta Microsoft 365 | `usuario[N]@labagentes[N].onmicrosoft.com` con licencia E3 + Copilot |
| Contraseña inicial | `LabCopilot2024!` (cambiar en primer acceso) |
| URL Copilot Studio | https://copilotstudio.microsoft.com |
| Entorno de práctica | `LabPractice-M2` (tipo Sandbox, región United States) |

## Entorno del Laboratorio

### Software Requerido

| Software | Versión | Propósito |
|---|---|---|
| Microsoft Edge | 124.0.2478.67 | Navegador principal |
| Google Chrome (alternativa) | 124.0.6367.119 | Navegador alternativo |
| Microsoft Copilot Studio | Release Wave 1 2024 (Web GA) | Plataforma de creación del agente |
| Visual Studio Code | 1.89.1 | Documentación de justificación |

### Estructura de Carpetas Local

Verifica que la siguiente estructura exista antes de comenzar:

```
C:/LabAgentes/          (Windows)
~/LabAgentes/           (macOS/Linux)
├── prompts/
├── capturas/
└── docs/
```

Si no existe, créala ejecutando en terminal:

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "C:\LabAgentes\prompts", "C:\LabAgentes\capturas", "C:\LabAgentes\docs"
```

**macOS/Linux (Terminal):**
```bash
mkdir -p ~/LabAgentes/{prompts,capturas,docs}
```

## Procedimiento Paso a Paso

### Paso 1: Acceder a Microsoft Copilot Studio y Seleccionar el Entorno

**Objetivo:** Iniciar sesión en Copilot Studio y verificar que estás trabajando en el entorno correcto `LabPractice-M2`.

**Instrucciones:**

1. Abre tu navegador (Microsoft Edge o Google Chrome).
2. Navega a la URL: `https://copilotstudio.microsoft.com`
3. Inicia sesión con tus credenciales del tenant de práctica:
   - **Usuario:** `usuario[N]@labagentes[N].onmicrosoft.com`
   - **Contraseña:** Tu contraseña actualizada (o `LabCopilot2024!` si es tu primer acceso; en ese caso, sigue las instrucciones para cambiarla).
4. Una vez en el dashboard principal, localiza el **selector de entorno** en la esquina superior derecha de la barra de navegación.
5. Haz clic en el selector de entorno y selecciona **LabPractice-M2** de la lista desplegable.
6. Confirma que el nombre del entorno aparece correctamente en la barra superior.

**Resultado Esperado:**

La interfaz principal de Copilot Studio se muestra con el entorno `LabPractice-M2` activo. Deberías ver el dashboard con opciones como "Crear" (o "Create"), el listado de agentes existentes (posiblemente vacío si es tu primera vez) y la barra de navegación lateral.

**Verificación:**

- ✅ El nombre `LabPractice-M2` aparece visible en el selector de entorno
- ✅ No hay mensajes de error de licencia o permisos
- ✅ El botón para crear un nuevo agente está disponible y habilitado

---

### Paso 2: Crear un Nuevo Agente desde el Dashboard

**Objetivo:** Iniciar el asistente de creación de un nuevo agente (Copilot) y configurar los datos básicos de identidad.

**Instrucciones:**

1. En el dashboard principal de Copilot Studio, haz clic en el botón **"+ Crear"** (o **"+ Create"** si tu interfaz está en inglés) ubicado en el panel lateral izquierdo o en el área central del dashboard.
2. Selecciona la opción **"Nuevo agente"** (o **"New agent"**).
3. El asistente de creación se abrirá mostrando un campo de conversación donde puedes describir qué quieres que haga tu agente. En el campo de descripción conversacional, escribe:

```
Necesito un agente de soporte interno para empleados de TechNova Solutions que responda preguntas sobre políticas de la empresa, procedimientos de TI y recursos humanos.
```

4. Haz clic en **"Omitir para configurar"** (o **"Skip to configure"**) para acceder directamente al formulario de configuración manual en lugar de usar la creación guiada por IA.
5. En el formulario de configuración, completa los siguientes campos:

| Campo | Valor |
|---|---|
| **Nombre** | `Nova Assistant` |
| **Descripción** | `Agente de soporte interno para empleados de TechNova Solutions. Responde consultas sobre políticas empresariales, procedimientos de TI, recursos humanos y procesos internos.` |
| **Instrucciones** | Deja este campo con el contenido predeterminado por ahora (se configurará en laboratorios posteriores) |

6. Para el **ícono del agente**, haz clic en el área del ícono y selecciona una de las siguientes opciones:
   - Selecciona un ícono predeterminado que represente soporte técnico o asistencia (por ejemplo, un ícono de auriculares, un robot o una estrella).
   - Alternativamente, si la plataforma permite subir una imagen personalizada, utiliza cualquier imagen de 48x48 píxeles o superior que represente un asistente virtual.

7. Haz clic en **"Crear"** (o **"Create"**) para finalizar la creación inicial del agente.

**Resultado Esperado:**

El agente **Nova Assistant** se crea exitosamente y se abre la interfaz de edición del agente. Deberías ver:
- El nombre "Nova Assistant" en la parte superior del editor
- El panel de navegación lateral con secciones como "Temas" (Topics), "Acciones" (Actions), "Conocimiento" (Knowledge) y "Configuración" (Settings)
- Un panel de prueba (Test) en el lado derecho o inferior de la pantalla

**Verificación:**

- ✅ El agente aparece con el nombre "Nova Assistant" en la interfaz
- ✅ La descripción configurada es visible en la sección de información del agente
- ✅ El ícono seleccionado se muestra correctamente
- ✅ No aparecen errores de creación

---

### Paso 3: Explorar las Opciones de Modelo de IA Generativa

**Objetivo:** Localizar la configuración del modelo de IA generativa y comparar las opciones disponibles (GPT-4o vs GPT-3.5 Turbo) para tomar una decisión informada.

**Instrucciones:**

1. Dentro del editor del agente "Nova Assistant", navega a la sección **"Configuración"** (o **"Settings"**) haciendo clic en el ícono de engranaje en la barra superior o en el menú lateral.
2. Busca la sección **"IA generativa"** (o **"Generative AI"** / **"AI features"**). Esta sección puede estar dentro de:
   - `Configuración > IA generativa`
   - `Configuración > Modelo de lenguaje`
   - O directamente en las opciones de "Respuestas generativas" del agente
3. Localiza las opciones de modelo disponibles. En Copilot Studio Release Wave 1 2024, las opciones típicas incluyen:
   - **GPT-4o** (gpt-4o-2024-05-13)
   - **GPT-3.5 Turbo**
4. Antes de seleccionar un modelo, revisa la siguiente tabla comparativa que resume las diferencias clave:

| Criterio | GPT-4o | GPT-3.5 Turbo |
|---|---|---|
| **Capacidad de razonamiento** | Superior — maneja instrucciones complejas, múltiples restricciones y ambigüedades | Básica — funciona bien con instrucciones simples y directas |
| **Velocidad de respuesta** | Rápida (optimizado respecto a GPT-4 original) | Muy rápida |
| **Costo por token** | Mayor (aproximadamente 5-10x más que GPT-3.5) | Menor |
| **Ventana de contexto** | 128K tokens | 16K tokens |
| **Seguimiento de instrucciones** | Excelente — alta adherencia al system prompt | Bueno — puede desviarse en escenarios complejos |
| **Capacidad multimodal** | Sí (texto, imagen, audio) | Solo texto |
| **Caso de uso recomendado** | Agentes empresariales con instrucciones detalladas y múltiples fuentes de conocimiento | Chatbots simples de FAQ con respuestas predefinidas |

5. Documenta mentalmente (o en una nota rápida) la justificación para elegir GPT-4o:
   - El agente Nova Assistant manejará políticas complejas de RRHH y TI
   - Necesita seguir instrucciones detalladas del system prompt (que se configurarán en labs posteriores)
   - La ventana de contexto amplia permite integrar documentos extensos como fuente de conocimiento
   - La capacidad de razonamiento superior es necesaria para interpretar preguntas ambiguas de los empleados

**Resultado Esperado:**

Puedes visualizar las opciones de modelo disponibles en la interfaz de configuración y comprendes las diferencias entre GPT-4o y GPT-3.5 Turbo en el contexto del caso de uso.

**Verificación:**

- ✅ Has localizado la sección de configuración de IA generativa
- ✅ Puedes identificar los modelos disponibles en la plataforma
- ✅ Comprendes por qué GPT-4o es la elección adecuada para este caso de uso

---

### Paso 4: Seleccionar GPT-4o como Modelo Base

**Objetivo:** Configurar GPT-4o como el modelo de IA generativa del agente y guardar la configuración.

**Instrucciones:**

1. En la sección de configuración de IA generativa, selecciona **GPT-4o** como modelo base.
   - Si la interfaz presenta un selector desplegable, elige "GPT-4o" de la lista.
   - Si la interfaz presenta opciones de radio button o tarjetas, selecciona la tarjeta correspondiente a GPT-4o.
2. Verifica que la opción de **respuestas generativas** (Generative Answers) esté **habilitada**. Esta configuración permite que el agente utilice el modelo seleccionado para generar respuestas basadas en las fuentes de conocimiento que se integrarán en laboratorios posteriores.
3. Si existe una opción de **"Nivel de creatividad"** o **"Temperature"**, déjala en el valor predeterminado (generalmente medio o "balanced").
4. Haz clic en **"Guardar"** (o **"Save"**) para aplicar los cambios de configuración.
5. Regresa al editor principal del agente haciendo clic en la flecha de retroceso o en el nombre del agente en la barra de navegación.

**Resultado Esperado:**

El modelo GPT-4o queda configurado como motor de IA generativa del agente Nova Assistant. La configuración se guarda sin errores y el agente está listo para recibir configuraciones adicionales en laboratorios posteriores.

**Verificación:**

- ✅ GPT-4o aparece como modelo seleccionado en la configuración
- ✅ Las respuestas generativas están habilitadas
- ✅ El guardado se completó sin mensajes de error
- ✅ Al regresar a la configuración, la selección de GPT-4o persiste

---

### Paso 5: Verificar la Creación del Agente y Documentar

**Objetivo:** Confirmar que el agente está correctamente creado con todos los parámetros configurados y documentar la evidencia.

**Instrucciones:**

1. En el editor del agente, verifica la información general:
   - Haz clic en **"Configuración"** > **"Detalles del agente"** (o la sección equivalente)
   - Confirma que el nombre es `Nova Assistant`
   - Confirma que la descripción está completa
   - Confirma que el ícono está asignado
2. Navega nuevamente a la configuración de IA generativa y confirma que GPT-4o está seleccionado.
3. Toma una captura de pantalla de la pantalla de configuración del agente mostrando:
   - El nombre del agente
   - El modelo de IA seleccionado
4. Guarda la captura de pantalla en tu carpeta local:
   - **Windows:** `C:\LabAgentes\capturas\lab03-01-config-agente.png`
   - **macOS/Linux:** `~/LabAgentes/capturas/lab03-01-config-agente.png`
5. Abre Visual Studio Code y crea un archivo de documentación con la justificación de la elección del modelo:

**Ruta del archivo:** `C:\LabAgentes\docs\justificacion-modelo.md` (Windows) o `~/LabAgentes/docs/justificacion-modelo.md` (macOS/Linux)

**Contenido del archivo:**

```markdown
# Justificación de Elección de Modelo - Nova Assistant

## Agente
- **Nombre:** Nova Assistant
- **Plataforma:** Microsoft Copilot Studio
- **Entorno:** LabPractice-M2
- **Fecha de creación:** [Fecha actual]

## Modelo Seleccionado
**GPT-4o** (gpt-4o-2024-05-13)

## Justificación

### ¿Por qué GPT-4o y no GPT-3.5 Turbo?

1. **Razonamiento complejo:** Nova Assistant debe interpretar preguntas ambiguas 
   de empleados sobre políticas de RRHH y TI que pueden tener múltiples 
   interpretaciones. GPT-4o maneja mejor la desambiguación.

2. **Adherencia a instrucciones:** En laboratorios posteriores se configurará un 
   system prompt detallado con restricciones, tono y límites. GPT-4o tiene 
   superior capacidad de seguir instrucciones complejas sin desviarse.

3. **Ventana de contexto amplia (128K tokens):** Permite integrar documentos 
   extensos de políticas empresariales como fuente de conocimiento sin truncar 
   información relevante.

4. **Caso de uso empresarial:** Para un agente de soporte interno que maneja 
   información sensible y debe ser preciso, la calidad de respuesta justifica 
   el costo adicional respecto a GPT-3.5 Turbo.

### ¿Por qué Microsoft Copilot Studio y no otra plataforma?

- TechNova Solutions utiliza Microsoft 365 como suite de productividad
- Integración nativa con SharePoint (fuente de documentos de políticas)
- Publicación directa en Microsoft Teams (canal de comunicación de empleados)
- Cumplimiento de seguridad corporativa ya gestionado por el tenant M365
- Control administrativo centralizado mediante Power Platform Admin Center
```

6. Guarda el archivo en Visual Studio Code.

**Resultado Esperado:**

Tienes documentación completa de la creación del agente y la justificación de la elección del modelo, almacenada en la estructura de carpetas del laboratorio.

**Verificación:**

- ✅ Captura de pantalla guardada en `capturas/lab03-01-config-agente.png`
- ✅ Archivo `justificacion-modelo.md` creado y guardado en `docs/`
- ✅ El agente Nova Assistant aparece en el listado de agentes del entorno LabPractice-M2

---

## Validación y Pruebas

Para confirmar que el laboratorio se completó exitosamente, verifica los siguientes criterios:

### Lista de Verificación Final

| # | Criterio | Estado |
|---|---|---|
| 1 | El agente "Nova Assistant" existe en el entorno LabPractice-M2 | ☐ |
| 2 | El nombre del agente es exactamente "Nova Assistant" | ☐ |
| 3 | La descripción del agente menciona "TechNova Solutions" y "soporte interno" | ☐ |
| 4 | El agente tiene un ícono asignado (no el predeterminado genérico) | ☐ |
| 5 | GPT-4o está seleccionado como modelo de IA generativa | ☐ |
| 6 | Las respuestas generativas están habilitadas | ☐ |
| 7 | La captura de pantalla existe en la carpeta `capturas/` | ☐ |
| 8 | El archivo `justificacion-modelo.md` existe en la carpeta `docs/` | ☐ |

### Prueba Rápida del Agente

Para verificar que el agente responde (aunque aún no tiene conocimiento configurado):

1. En el editor del agente, localiza el panel de **"Probar"** (Test) en la parte derecha o inferior.
2. Si el panel no está visible, haz clic en el botón **"Probar agente"** (o "Test your agent").
3. Escribe un mensaje simple: `Hola, ¿qué puedes hacer?`
4. El agente debería responder con un mensaje genérico indicando que puede ayudar (la respuesta será básica porque aún no se han configurado temas ni conocimiento).
5. Confirma que la respuesta se genera sin errores — esto valida que el modelo GPT-4o está correctamente conectado.

---

## Solución de Problemas

### Problema 1: El entorno LabPractice-M2 no aparece en el selector

**Síntomas:**
- Al hacer clic en el selector de entorno, solo aparece el entorno predeterminado (Default) u otros entornos, pero no `LabPractice-M2`.
- Puede aparecer un mensaje indicando que no tienes acceso a entornos adicionales.

**Causa:**
El administrador del tenant no ha creado el entorno `LabPractice-M2` o no ha asignado tu usuario como miembro del entorno con rol de "Maker" o superior en Power Platform Admin Center.

**Solución:**
1. Verifica que estás usando la cuenta correcta del tenant de práctica (`usuario[N]@labagentes[N].onmicrosoft.com`) y no una cuenta personal.
2. Intenta acceder directamente mediante la URL con el entorno especificado: `https://copilotstudio.microsoft.com/environments/LabPractice-M2`
3. Si el problema persiste, contacta al instructor para que verifique en Power Platform Admin Center (`https://admin.powerplatform.microsoft.com`) que:
   - El entorno `LabPractice-M2` está creado con tipo "Sandbox" y región "United States"
   - Tu usuario tiene asignado el rol "Environment Maker" o "System Administrator" en ese entorno
4. Cierra sesión completamente, limpia la caché del navegador y vuelve a iniciar sesión.

---

### Problema 2: No se muestra la opción de selección de modelo GPT-4o

**Síntomas:**
- En la sección de configuración de IA generativa, no aparece un selector de modelo.
- Solo se muestra una opción de habilitar/deshabilitar respuestas generativas sin posibilidad de elegir entre GPT-4o y GPT-3.5 Turbo.
- O bien, la sección de "IA generativa" no existe en el menú de configuración.

**Causa:**
Esto puede ocurrir por dos razones: (1) La licencia del tenant no incluye el complemento de Copilot / IA generativa necesario, o (2) La versión de Copilot Studio disponible en tu región tiene la selección de modelo gestionada automáticamente por la plataforma (en algunas configuraciones, GPT-4o se asigna por defecto sin opción manual visible).

**Solución:**
1. Si la sección de IA generativa existe pero no muestra selector de modelo: verifica que las respuestas generativas estén habilitadas. En muchas configuraciones de Release Wave 1 2024, GPT-4o es el modelo predeterminado cuando se habilitan las respuestas generativas — documenta esto en tu archivo de justificación.
2. Si la sección no existe en absoluto:
   - Navega a `Configuración` > `Características de IA` (AI Features) — el nombre puede variar según la localización de la interfaz.
   - Busca en `Configuración` > `Respuestas generativas` > `Configuración del modelo`.
3. Si ninguna opción está disponible, confirma con el instructor que la licencia del tenant incluye "Microsoft Copilot Studio" con capacidades de IA generativa habilitadas (no solo la versión clásica de Power Virtual Agents).
4. Como alternativa de documentación: si el modelo se asigna automáticamente, toma una captura de pantalla de la configuración disponible y anota en tu archivo de justificación que la plataforma asigna GPT-4o por defecto en el entorno configurado.

---

## Limpieza

Este laboratorio **NO requiere limpieza**. El agente "Nova Assistant" creado en este laboratorio es un artefacto necesario para los laboratorios subsiguientes (Lab 03-00-02, 03-00-03 y 03-00-04).

**⚠️ Importante:** No elimines el agente "Nova Assistant" al finalizar este laboratorio. Los siguientes laboratorios del módulo construyen sobre este agente añadiendo:
- System prompt personalizado (Lab 03-00-02)
- Fuentes de conocimiento (Lab 03-00-03)
- Pruebas de validación (Lab 03-00-04)

---

## Resumen

En este laboratorio has completado las siguientes acciones fundamentales:

| Acción | Resultado |
|---|---|
| Acceso a Copilot Studio | Sesión iniciada en el entorno LabPractice-M2 |
| Creación del agente | "Nova Assistant" creado con nombre, descripción e ícono |
| Evaluación de modelos | Comparación GPT-4o vs GPT-3.5 Turbo documentada |
| Selección de modelo | GPT-4o configurado como modelo base |
| Documentación | Justificación técnica guardada en archivo Markdown |

### Conceptos Clave Reforzados

- **Microsoft Copilot Studio** es la plataforma adecuada cuando la organización opera en el ecosistema Microsoft 365, por su integración nativa con Teams, SharePoint y Power Platform.
- **GPT-4o** es preferible a GPT-3.5 Turbo para agentes empresariales que requieren razonamiento complejo, adherencia estricta a instrucciones y procesamiento de documentos extensos.
- La elección de plataforma y modelo debe documentarse con justificación técnica vinculada al caso de uso específico.

### Próximo Laboratorio

En el **Lab 03-00-02** configurarás las instrucciones personalizadas (system prompt) del agente Nova Assistant, definiendo su comportamiento, tono, restricciones y límites operativos utilizando técnicas avanzadas de ingeniería de prompts.

### Recursos Adicionales

- [Documentación oficial de Microsoft Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/fundamentals-what-is-copilot-studio)
- [Modelos de IA en Azure OpenAI Service](https://learn.microsoft.com/es-es/azure/ai-services/openai/concepts/models)
- [Comparativa GPT-4o vs GPT-3.5 Turbo — OpenAI](https://platform.openai.com/docs/models)
- [Power Platform Admin Center — Gestión de entornos](https://learn.microsoft.com/es-es/power-platform/admin/environments-overview)

---

# 4 Laboratorio: Integración de instrucciones personalizadas

## Metadata

| Campo | Detalle |
|-------|---------|
| **Duración** | 10 minutos |
| **Complejidad** | Fácil |
| **Nivel Bloom** | Aplicar |
| **Plataforma principal** | Microsoft Copilot Studio (Release Wave 1 2024) |
| **Modelo de IA** | GPT-4o (gpt-4o-2024-05-13) |

## Descripción General

En este laboratorio integrarás instrucciones personalizadas (system prompt) en el agente **Nova Assistant** creado en el Lab 03-00-01. Redactarás un prompt de sistema estructurado que define la identidad, tono, restricciones y formato de respuesta del agente, lo pegarás en la sección de configuración de Copilot Studio y verificarás su correcto funcionamiento mediante tres pruebas en el panel de chat integrado.

## Objetivos de Aprendizaje

Al completar este laboratorio serás capaz de:

- [ ] Redactar un system prompt estructurado y completo que defina rol, tono, restricciones y formato de respuestas para el agente Nova Assistant.
- [ ] Integrar las instrucciones personalizadas en la sección de configuración del agente en Copilot Studio y confirmar que el agente las reconoce.
- [ ] Aplicar mejores prácticas de prompt engineering para instrucciones de sistema, incluyendo definición de persona, contexto empresarial, reglas de comportamiento y ejemplos de respuesta.

## Prerrequisitos

### Conocimientos previos

| Requisito | Descripción |
|-----------|-------------|
| Lab 03-00-01 completado | Agente **Nova Assistant** con GPT-4o creado y guardado en Copilot Studio dentro del entorno `LabPractice-M2` |
| Prompt engineering básico | Comprensión de los conceptos de rol, contexto, restricciones y formato (material teórico del módulo 3.2) |
| Plantilla de instrucciones | Documento `.docx` provisto por el instructor con la plantilla base del system prompt |

### Acceso y software

| Herramienta | Versión / Detalle |
|-------------|-------------------|
| Microsoft Copilot Studio | Web GA — Release Wave 1 2024 |
| Navegador | Microsoft Edge 120+ o Google Chrome 120+ |
| Editor de texto | Notepad++ 8.6.4 o Visual Studio Code 1.89.1 |
| Credenciales | `usuario[N]@labagentes[N].onmicrosoft.com` con licencia M365 E3 + Copilot |

## Entorno del Laboratorio

### Estructura de carpetas requerida

Antes de iniciar, confirma que la siguiente estructura existe en tu equipo local:

```
Windows:  C:\AgenteLabs\
macOS:    ~/AgenteLabs/
```

Si no existe, créala ahora:

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "C:\AgenteLabs"
```

**macOS/Linux (Terminal):**
```bash
mkdir -p ~/AgenteLabs
```

### Acceso a Copilot Studio

URL directa al entorno de práctica:

```
https://copilotstudio.microsoft.com/environments/LabPractice-M2
```

---

## Paso a Paso

### Paso 1: Abrir el agente Nova Assistant en Copilot Studio

**Objetivo:** Acceder al agente creado en el laboratorio anterior y navegar a la sección de instrucciones.

**Instrucciones:**

1. Abre tu navegador y navega a `https://copilotstudio.microsoft.com`.
2. Inicia sesión con tus credenciales del tenant de práctica (`usuario[N]@labagentes[N].onmicrosoft.com`).
3. En la esquina superior derecha, verifica que el entorno activo sea **LabPractice-M2**. Si no lo es, haz clic en el selector de entornos y selecciónalo.
4. En el panel lateral izquierdo, haz clic en **Copilots** (o **Agentes**).
5. Localiza el agente **Nova Assistant** en la lista y haz clic sobre él para abrirlo.
6. En la vista del agente, localiza la sección **Instructions** (Instrucciones) en el panel de configuración principal. En Copilot Studio Release Wave 1 2024, esta sección aparece directamente en la pestaña **Overview** o en la sección superior etiquetada como *"Describe what your copilot should do and how it should behave"*.

**Resultado esperado:** Estás dentro del editor del agente Nova Assistant con el campo de instrucciones visible y editable.

**Verificación:** El campo de instrucciones muestra un área de texto vacía o con contenido genérico del Lab anterior. El nombre del agente en la barra superior muestra "Nova Assistant".

---

### Paso 2: Redactar el system prompt estructurado

**Objetivo:** Crear un prompt de sistema completo siguiendo las mejores prácticas de prompt engineering, que defina identidad, tono, restricciones y formato.

**Instrucciones:**

1. Abre **Visual Studio Code** o **Notepad++** en tu equipo.
2. Crea un nuevo archivo y guárdalo inmediatamente con el nombre:
   - Windows: `C:\AgenteLabs\nova_instructions_v1.txt`
   - macOS: `~/AgenteLabs/nova_instructions_v1.txt`
3. Escribe el siguiente system prompt en el archivo. Copia el contenido completo tal como aparece a continuación:

```text
## Identidad
Eres Nova Assistant, el asistente virtual de soporte interno de TechNova Solutions. Tu función principal es ayudar a los empleados de la empresa a resolver dudas sobre procesos internos, políticas corporativas y procedimientos operativos.

## Tono y estilo de comunicación
- Profesional pero cercano.
- Conciso: responde de forma directa sin rodeos innecesarios.
- Empático: reconoce la situación del usuario antes de dar la respuesta.
- Siempre responde en español, independientemente del idioma en que te escriban.

## Restricciones de comportamiento
1. NO respondas preguntas que estén fuera del ámbito empresarial de TechNova Solutions.
2. NO inventes políticas, procedimientos ni datos que no estén en tu base de conocimiento.
3. Si no tienes la información solicitada, indícalo con honestidad y redirige al equipo correspondiente.
4. NO proporciones opiniones personales, asesoría legal ni recomendaciones médicas.
5. NO compartas información confidencial de un departamento con empleados de otro sin autorización explícita.

## Formato de respuestas
- Cuando la respuesta incluya más de 3 ítems, utiliza listas con viñetas o numeradas.
- Usa negritas para resaltar términos clave o nombres de procesos.
- Mantén las respuestas en un máximo de 150 palabras salvo que el usuario pida más detalle.
- Incluye al final de cada respuesta la pregunta: "¿Hay algo más en lo que pueda ayudarte?"

## Manejo de preguntas fuera de alcance
- Para temas de nómina, vacaciones, beneficios o políticas de personal → redirige al equipo de **Recursos Humanos** (rrhh@technova.com).
- Para problemas técnicos con equipos, software o accesos → redirige al equipo de **Soporte TI** (soporte.ti@technova.com).
- Para cualquier otro tema no cubierto → indica que no dispones de esa información y sugiere contactar al supervisor directo.

## Ejemplo de respuesta esperada
Usuario: "¿Cuántos días de vacaciones me corresponden?"
Nova Assistant: "Entiendo que necesitas información sobre tus días de vacaciones. Lamentablemente, no tengo acceso a los datos específicos de tu expediente. Te recomiendo contactar al equipo de **Recursos Humanos** en rrhh@technova.com, quienes podrán darte la información exacta según tu antigüedad. ¿Hay algo más en lo que pueda ayudarte?"
```

4. Guarda el archivo (`Ctrl+S` en Windows / `Cmd+S` en macOS).

**Resultado esperado:** El archivo `nova_instructions_v1.txt` está guardado en la carpeta `AgenteLabs` con el system prompt completo y estructurado.

**Verificación:** Abre el archivo desde el explorador de archivos y confirma que el contenido se muestra correctamente sin caracteres corruptos. El archivo debe tener aproximadamente 1.5 KB de tamaño.

---

### Paso 3: Integrar las instrucciones en Copilot Studio

**Objetivo:** Pegar el system prompt redactado en la sección de instrucciones del agente dentro de Copilot Studio y guardar la configuración.

**Instrucciones:**

1. Regresa a la ventana del navegador con Copilot Studio abierto en el agente **Nova Assistant**.
2. Haz clic dentro del campo de texto de la sección **Instructions**.
3. Si existe contenido previo en el campo, selecciónalo todo (`Ctrl+A`) y elimínalo.
4. Regresa a VS Code / Notepad++, selecciona **todo** el contenido del archivo `nova_instructions_v1.txt` (`Ctrl+A`) y cópialo (`Ctrl+C`).
5. Vuelve al navegador y pega el contenido en el campo de instrucciones (`Ctrl+V`).
6. Revisa visualmente que el texto se haya pegado correctamente y que no haya saltos de línea rotos ni caracteres extraños.
7. Haz clic en el botón **Save** (Guardar) en la parte superior del editor del agente.
8. Espera a que aparezca la confirmación de guardado (notificación verde o mensaje "Saved successfully").

**Resultado esperado:** Las instrucciones personalizadas están integradas en el agente Nova Assistant. El campo de instrucciones muestra el prompt completo con su estructura de secciones.

**Verificación:** Después de guardar, navega fuera del agente (por ejemplo, al dashboard de Copilots) y vuelve a abrir Nova Assistant. Confirma que las instrucciones persisten en el campo de texto.

---

### Paso 4: Verificar el comportamiento mediante pruebas en el panel de chat

**Objetivo:** Ejecutar tres pruebas básicas que confirmen que el agente respeta la identidad, el tono, las restricciones y el formato definidos en el system prompt.

**Instrucciones:**

1. En el editor del agente Nova Assistant, localiza el panel **Test your copilot** (Probar tu copiloto) en el lado derecho de la pantalla. Si no está visible, haz clic en el botón **Test** en la barra superior.
2. Haz clic en el icono de **reiniciar conversación** (flecha circular) para iniciar una sesión limpia.

**Prueba 1 — Verificación de identidad y tono:**

3. Escribe en el chat de prueba:
```
Hola, ¿quién eres y cómo puedes ayudarme?
```
4. Observa la respuesta. Verifica que:
   - Se identifica como **Nova Assistant** de **TechNova Solutions**.
   - El tono es profesional y cercano.
   - Responde en español.
   - Finaliza con "¿Hay algo más en lo que pueda ayudarte?" o frase equivalente.

**Prueba 2 — Verificación de restricciones y redirección:**

5. Escribe en el chat de prueba:
```
¿Cuántos días de vacaciones me corresponden este año?
```
6. Observa la respuesta. Verifica que:
   - **NO inventa** un número de días.
   - Reconoce que no tiene esa información.
   - Redirige al equipo de **Recursos Humanos** con el correo `rrhh@technova.com`.
   - Mantiene tono empático.

**Prueba 3 — Verificación de formato (listas):**

7. Escribe en el chat de prueba:
```
¿Qué canales de comunicación interna tiene TechNova Solutions?
```
8. Observa la respuesta. Verifica que:
   - Si lista más de 3 elementos, utiliza formato de lista con viñetas.
   - Si no tiene información específica, indica honestamente que no dispone de esos datos y sugiere contactar al equipo correspondiente.
   - Responde en español y cierra con la pregunta de seguimiento.

**Resultado esperado:** Las tres pruebas confirman que el agente respeta las instrucciones configuradas: se identifica correctamente, no inventa información, redirige cuando corresponde y utiliza el formato indicado.

**Verificación:** Toma una captura de pantalla del panel de prueba mostrando al menos dos de las tres interacciones y guárdala como:
- Windows: `C:\AgenteLabs\prueba_instrucciones_v1.png`
- macOS: `~/AgenteLabs/prueba_instrucciones_v1.png`

---

## Validación y Pruebas

Para considerar este laboratorio como completado exitosamente, verifica los siguientes criterios:

| # | Criterio de validación | Estado |
|---|------------------------|--------|
| 1 | El archivo `nova_instructions_v1.txt` existe en `C:\AgenteLabs\` (o `~/AgenteLabs/`) con el system prompt completo | ☐ |
| 2 | El campo Instructions del agente Nova Assistant en Copilot Studio contiene el prompt íntegro | ☐ |
| 3 | El agente se guarda sin errores después de pegar las instrucciones | ☐ |
| 4 | Prueba 1: El agente se identifica como Nova Assistant de TechNova Solutions con tono profesional en español | ☐ |
| 5 | Prueba 2: El agente no inventa datos y redirige correctamente a RRHH | ☐ |
| 6 | Prueba 3: El agente respeta el formato de listas o reconoce honestamente la falta de información | ☐ |
| 7 | Captura de pantalla guardada como evidencia | ☐ |

---

## Solución de Problemas

### Problema 1: El agente no responde en español a pesar de la instrucción

**Síntomas:** Al escribir en el panel de prueba, el agente responde parcial o totalmente en inglés, ignorando la instrucción de responder siempre en español.

**Causa:** Copilot Studio puede tener configurado un idioma primario en inglés a nivel del agente, lo cual puede interferir con las instrucciones del system prompt. Además, si el mensaje del usuario está en inglés, el modelo puede priorizar el idioma de entrada sobre las instrucciones.

**Solución:**
1. Ve a **Settings** (Configuración) del agente → **Language** (Idioma).
2. Verifica que el idioma principal esté configurado como **Español (es)** o agrega español como idioma soportado.
3. Si no puedes cambiar el idioma del agente, refuerza la instrucción en el prompt agregando al inicio: `REGLA ABSOLUTA: Todas tus respuestas deben estar en español sin excepción.`
4. Guarda y vuelve a probar reiniciando la conversación.

---

### Problema 2: Las instrucciones no se guardan o el campo aparece vacío al reabrir el agente

**Síntomas:** Después de pegar el system prompt y hacer clic en Save, al navegar fuera y volver al agente, el campo de instrucciones aparece vacío o con el contenido anterior.

**Causa:** Puede deberse a un problema de caché del navegador, a que el guardado no se completó correctamente (por ejemplo, por un timeout de red), o a que el contenido excede un límite de caracteres no documentado.

**Solución:**
1. Limpia la caché del navegador o abre Copilot Studio en una ventana de incógnito.
2. Verifica tu conexión a internet (prueba abriendo otra página).
3. Intenta guardar nuevamente. Después de hacer clic en **Save**, espera al menos 5 segundos hasta ver la confirmación visual.
4. Si el problema persiste, reduce ligeramente el contenido del prompt (elimina el ejemplo de respuesta esperada al final) y vuelve a intentar.
5. Como alternativa, intenta con un navegador diferente (Edge si usabas Chrome, o viceversa).

---

## Limpieza

Este laboratorio **no requiere limpieza** de recursos. Los artefactos generados son necesarios para el siguiente laboratorio (Lab 03-00-03):

- **Conservar:** El agente Nova Assistant con las instrucciones integradas en Copilot Studio.
- **Conservar:** El archivo `nova_instructions_v1.txt` en la carpeta `AgenteLabs`.
- **Conservar:** La captura de pantalla de evidencia.

> ⚠️ **No elimines ni modifiques** las instrucciones del agente hasta completar el Lab 03-00-03, ya que este laboratorio es prerrequisito directo.

---

## Resumen

En este laboratorio aplicaste las mejores prácticas de prompt engineering para crear un system prompt estructurado que transforma un agente genérico en un asistente especializado. Los conceptos clave practicados fueron:

- **Definición de identidad (persona):** Establecer quién es el agente y cuál es su función.
- **Tono y estilo:** Configurar cómo se comunica el agente (profesional, conciso, empático).
- **Restricciones explícitas:** Definir qué NO debe hacer el agente para evitar respuestas incorrectas o fuera de alcance.
- **Formato de respuesta:** Estandarizar la presentación de la información (listas, negritas, longitud máxima).
- **Manejo de excepciones:** Definir rutas de escalamiento cuando el agente no puede resolver una consulta.

### Conexión con el siguiente laboratorio

En el **Lab 03-00-03** iterarás sobre estas instrucciones, refinando el prompt basándote en los resultados de las pruebas y agregando escenarios más complejos de interacción.

### Recursos adicionales

- [Documentación oficial: Instrucciones en Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-create-edit-topics)
- [Mejores prácticas de system prompts — OpenAI](https://platform.openai.com/docs/guides/prompt-engineering)
- [Guía de prompt engineering para agentes empresariales — Microsoft](https://learn.microsoft.com/es-es/ai/playbook/technology-guidance/generative-ai/working-with-llms/prompt-engineering)

---

# 6 Laboratorio: Integración de conocimientos/documentos

## Metadata

| Campo | Detalle |
|-------|---------|
| **Duración** | 30 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar (Apply) |
| **Plataforma principal** | Microsoft Copilot Studio (Release Wave 1 2024) |
| **Entorno** | LabPractice-M2 |

## Descripción General

En este laboratorio integrarás fuentes de conocimiento empresarial al agente **Nova Assistant** creado en los laboratorios anteriores. Cargarás tres documentos simulados de TechNova Solutions (política de vacaciones, FAQ de soporte TI y catálogo de beneficios) en la sección Knowledge de Copilot Studio, verificarás su indexación exitosa y comprobarás que el agente utiliza Retrieval Augmented Generation (RAG) para responder preguntas con información específica de los documentos, diferenciándola del conocimiento general del modelo GPT-4o.

## Objetivos de Aprendizaje

- [ ] Cargar y configurar al menos 3 documentos de conocimiento empresarial en la sección Knowledge de Copilot Studio para que el agente los utilice como fuente de información contextualizada.
- [ ] Comprender y verificar el proceso de indexación de documentos (Azure AI Search), confirmando que cada fuente pasa del estado "Processing" a "Ready".
- [ ] Probar que el agente responde preguntas utilizando información específica de los documentos cargados, diferenciando respuestas basadas en conocimiento del documento versus conocimiento general del modelo.
- [ ] Configurar los parámetros de Generative Answers con nivel de confianza adecuado para el caso de uso empresarial.

## Prerrequisitos

### Conocimiento previo

| Requisito | Descripción |
|-----------|-------------|
| Lab 03-00-01 completado | Agente "Nova Assistant" creado en el entorno LabPractice-M2 |
| Lab 03-00-02 completado | Instrucciones personalizadas (`nova_instructions_v1.txt`) activas en el agente |
| Tema 3.5 del módulo | Comprensión del concepto de agentes con conocimiento empresarial y RAG |
| Panorama de plataformas (Lección 3.1) | Entender que Copilot Studio usa Azure AI Search como motor de indexación |

### Acceso requerido

| Recurso | Detalle |
|---------|---------|
| Credenciales Microsoft 365 | `usuario[N]@labagentes[N].onmicrosoft.com` con licencia Copilot |
| Copilot Studio | Acceso al entorno LabPractice-M2 via `https://copilotstudio.microsoft.com/environments/LabPractice-M2` |
| Documentos de práctica | 3 archivos descargados en `C:/AgenteLabs/docs/` |
| Adobe Acrobat Reader DC | Versión 2024.002.20759 para verificación de PDFs |

## Entorno del Laboratorio

### Hardware mínimo

| Componente | Especificación |
|------------|---------------|
| Procesador | Intel Core i5 8ª gen / AMD Ryzen 5 3000+ (64 bits) |
| RAM | 8 GB mínimo (16 GB recomendado) |
| Almacenamiento libre | 2 GB |
| Internet | 10 Mbps mínimo (25 Mbps recomendado para carga de documentos) |

### Software requerido

| Software | Versión |
|----------|---------|
| Microsoft Edge / Google Chrome | Edge 124.0.2478.67 / Chrome 124.0.6367.119 |
| Microsoft Copilot Studio | Release Wave 1 2024 (web) |
| Adobe Acrobat Reader DC | 2024.002.20759 |
| Visual Studio Code (opcional) | 1.89.1 |

### Preparación de estructura de carpetas

Si aún no existe la carpeta de documentos, créala antes de comenzar:

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "C:\AgenteLabs\docs"
New-Item -ItemType Directory -Force -Path "C:\AgenteLabs\capturas"
```

**macOS/Linux (Terminal):**
```bash
mkdir -p ~/AgenteLabs/docs
mkdir -p ~/AgenteLabs/capturas
```

### Documentos de práctica

El instructor provee los siguientes archivos que deben estar descargados en `C:/AgenteLabs/docs/` antes de iniciar:

| # | Archivo | Formato | Tamaño aprox. | Contenido |
|---|---------|---------|---------------|-----------|
| 1 | `TechNova_Politica_Vacaciones_2024.pdf` | PDF 1.7 | ~5 páginas | Reglas de acumulación, solicitud y aprobación de vacaciones |
| 2 | `TechNova_FAQ_Soporte_IT.docx` | DOCX (Word 2021) | ~8 páginas | Preguntas frecuentes: acceso a sistemas, contraseñas, VPN, equipos |
| 3 | `TechNova_Catalogo_Beneficios_2024.pdf` | PDF 1.7 | ~6 páginas | Seguro médico, fondo de ahorro, capacitaciones |

---

## Paso a Paso

### Paso 1: Verificar los documentos de conocimiento

**Objetivo:** Confirmar que los 3 documentos son legibles, no tienen restricciones de copia/extracción de texto y cumplen con los requisitos de formato de Copilot Studio.

**Instrucciones:**

1. Navega a la carpeta `C:/AgenteLabs/docs/` en el Explorador de archivos.
2. Haz doble clic en `TechNova_Politica_Vacaciones_2024.pdf` para abrirlo en Adobe Acrobat Reader DC.
3. Verifica que puedes seleccionar y copiar texto del documento (Ctrl+A para seleccionar todo, Ctrl+C para copiar).
4. Ve a **Archivo > Propiedades > Seguridad** y confirma que los permisos de "Copia de contenido" están en **Permitido**.
5. Cierra el PDF y repite los pasos 2-4 con `TechNova_Catalogo_Beneficios_2024.pdf`.
6. Abre `TechNova_FAQ_Soporte_IT.docx` con Microsoft Word o un visor compatible y verifica que el contenido es legible y no está protegido contra edición.
7. Confirma que ningún archivo excede los **20 MB** (clic derecho > Propiedades > pestaña General).

**Resultado esperado:**
- Los 3 archivos se abren sin errores.
- El texto es seleccionable y copiable en ambos PDFs.
- El archivo DOCX no tiene protección de edición activa.
- Todos los archivos están por debajo de 20 MB.

**Verificación:**

| Archivo | ¿Se abre? | ¿Texto copiable? | ¿Tamaño < 20 MB? |
|---------|-----------|-------------------|-------------------|
| `TechNova_Politica_Vacaciones_2024.pdf` | ✅ | ✅ | ✅ |
| `TechNova_FAQ_Soporte_IT.docx` | ✅ | ✅ | ✅ |
| `TechNova_Catalogo_Beneficios_2024.pdf` | ✅ | ✅ | ✅ |

> **Nota:** Si un PDF tiene restricciones de copia, Copilot Studio no podrá extraer el texto para indexación. Contacta al instructor para obtener una versión sin restricciones.

---

### Paso 2: Acceder al agente Nova Assistant en Copilot Studio

**Objetivo:** Navegar al agente existente y localizar la sección Knowledge donde se integrarán los documentos.

**Instrucciones:**

1. Abre Microsoft Edge o Google Chrome.
2. Navega a: `https://copilotstudio.microsoft.com/environments/LabPractice-M2`
3. Inicia sesión con tus credenciales: `usuario[N]@labagentes[N].onmicrosoft.com`.
4. En el panel lateral izquierdo, haz clic en **Agents** (o **Agentes**).
5. Localiza y haz clic en tu agente **Nova Assistant** (siguiendo la convención de nombres del curso, puede aparecer como `Agente-[TipoCasoUso]-[InicialNombreParticipante]`).
6. Una vez dentro del agente, localiza en el panel de navegación superior o lateral la sección **Knowledge** (Conocimiento).
7. Haz clic en **Knowledge** para acceder a la página de gestión de fuentes de conocimiento.

**Resultado esperado:**
- Ves la página de Knowledge del agente Nova Assistant.
- La página muestra un área vacía o con el mensaje "No knowledge sources added" (si es la primera vez que se configuran fuentes).
- Se muestra un botón o enlace para **+ Add knowledge** o **Upload files**.

**Verificación:**
- La URL del navegador contiene el identificador de tu agente y la sección Knowledge.
- El nombre del agente aparece en la parte superior de la interfaz.
- Captura de pantalla: guarda una captura de la pantalla Knowledge vacía en `C:/AgenteLabs/capturas/lab03-03-knowledge-vacio.png`.

---

### Paso 3: Cargar los documentos de conocimiento

**Objetivo:** Subir los 3 documentos empresariales a la sección Knowledge de Copilot Studio utilizando la funcionalidad "Upload files".

**Instrucciones:**

1. En la página Knowledge del agente, haz clic en **+ Add knowledge** (o **+ Agregar conocimiento**).
2. Se desplegará un menú con opciones de fuente. Selecciona **Files** (Archivos).
3. En el diálogo de carga, haz clic en **Upload** o **Browse** para abrir el explorador de archivos.
4. Navega a `C:/AgenteLabs/docs/`.
5. Selecciona los 3 archivos simultáneamente:
   - `TechNova_Politica_Vacaciones_2024.pdf`
   - `TechNova_FAQ_Soporte_IT.docx`
   - `TechNova_Catalogo_Beneficios_2024.pdf`
   
   > **Tip:** Mantén presionada la tecla `Ctrl` mientras haces clic en cada archivo para selección múltiple.

6. Haz clic en **Open** (Abrir) para confirmar la selección.
7. Verifica que los 3 archivos aparecen listados en el diálogo de carga con sus nombres y tamaños correctos.
8. Haz clic en **Add** o **Upload** para iniciar la carga.
9. Espera a que la barra de progreso indique que la carga se completó para los 3 archivos.

**Resultado esperado:**
- Los 3 archivos se cargan exitosamente sin errores.
- Cada archivo aparece en la lista de Knowledge con un estado inicial de **Processing** (Procesando) o un ícono de carga/reloj.
- No aparecen mensajes de error relacionados con formato, tamaño o permisos.

**Verificación:**
- Confirma que ves exactamente 3 entradas en la lista de Knowledge.
- Cada entrada muestra el nombre correcto del archivo.
- El estado de cada documento es "Processing" inmediatamente después de la carga.
- Captura de pantalla: `C:/AgenteLabs/capturas/lab03-03-docs-processing.png`.

> **Importante:** Si algún archivo falla, verifica que no excede 20 MB, que el formato es .pdf o .docx, y que el archivo no está corrupto. Copilot Studio acepta un máximo de 3 archivos por operación de carga.

---

### Paso 4: Verificar la indexación de documentos

**Objetivo:** Confirmar que Azure AI Search ha procesado correctamente cada documento y que su estado cambia de "Processing" a "Ready".

**Instrucciones:**

1. Permanece en la página Knowledge del agente.
2. Observa el estado de cada documento en la lista. Inicialmente verás **Processing** con un ícono de reloj o spinner.
3. Espera entre **2 y 5 minutos** por documento. El tiempo total estimado es de 5-10 minutos para los 3 documentos.
4. Refresca la página periódicamente haciendo clic en el botón de actualizar de la lista (si existe) o presionando `F5` en el navegador.
5. Para cada documento, verifica que el estado cambia a **Ready** (Listo) con un ícono de verificación verde (✓).
6. Si algún documento permanece en "Processing" por más de 10 minutos, espera 2 minutos adicionales y refresca nuevamente.
7. Una vez que los 3 documentos muestren estado **Ready**, registra la hora de finalización.

**Resultado esperado:**

| Documento | Estado final | Tiempo aproximado |
|-----------|-------------|-------------------|
| `TechNova_Politica_Vacaciones_2024.pdf` | ✅ Ready | 2-4 min |
| `TechNova_FAQ_Soporte_IT.docx` | ✅ Ready | 2-4 min |
| `TechNova_Catalogo_Beneficios_2024.pdf` | ✅ Ready | 2-5 min |

**Verificación:**
- Los 3 documentos muestran estado **Ready** (verde).
- No hay documentos con estado "Error" o "Failed".
- Al hacer clic en un documento individual, puedes ver metadata como el nombre del archivo, tamaño y fecha de carga.
- Captura de pantalla: `C:/AgenteLabs/capturas/lab03-03-docs-ready.png`.

> **Concepto clave:** El proceso de indexación utiliza Azure AI Search como motor subyacente. Copilot Studio fragmenta el contenido del documento en chunks (segmentos), genera embeddings vectoriales de cada chunk y los almacena en un índice de búsqueda. Cuando el agente recibe una pregunta, busca los chunks más relevantes y los pasa como contexto al modelo GPT-4o para generar la respuesta (RAG - Retrieval Augmented Generation).

---

### Paso 5: Configurar parámetros de Generative Answers

**Objetivo:** Habilitar y configurar las respuestas generativas (Generative Answers) para que el agente utilice los documentos indexados al responder preguntas, estableciendo el nivel de confianza apropiado.

**Instrucciones:**

1. Dentro del agente Nova Assistant, navega a la sección **Settings** (Configuración) o busca la opción **Generative AI** en el panel de configuración del agente.
2. Localiza la sección **Generative answers** (Respuestas generativas) o **AI-generated content**.
3. Verifica que la opción de respuestas generativas está **habilitada** (toggle en posición ON).
4. Busca la configuración de **Content moderation** o **Confidence level** (Nivel de confianza).
5. Configura el nivel de confianza en **Medium** (Medio):
   - **High** = Solo responde cuando la coincidencia con el documento es muy alta (puede no responder muchas preguntas).
   - **Medium** = Balance entre precisión y cobertura (recomendado para este laboratorio).
   - **Low** = Responde con mayor frecuencia pero con menor certeza de que la información proviene del documento.
6. En la sección de fuentes de conocimiento (Knowledge sources), confirma que los 3 documentos cargados aparecen como fuentes activas.
7. Si existe una opción para **citar fuentes** (Show citations / Show references), actívala.
8. Haz clic en **Save** (Guardar) para aplicar la configuración.

**Resultado esperado:**
- Generative Answers está habilitado.
- Nivel de confianza configurado en "Medium".
- Los 3 documentos aparecen como fuentes activas.
- La opción de citas/referencias está activada (si disponible).
- La configuración se guarda sin errores.

**Verificación:**
- Al volver a la configuración después de guardar, los valores persisten correctamente.
- No hay advertencias o errores en la interfaz.
- Captura de pantalla: `C:/AgenteLabs/capturas/lab03-03-generative-config.png`.

---

### Paso 6: Probar el agente con preguntas sobre los documentos

**Objetivo:** Ejecutar 5 preguntas de prueba en el panel de test de Copilot Studio para verificar que el agente recupera y utiliza información específica de los documentos cargados, diferenciando respuestas basadas en RAG de respuestas genéricas.

**Instrucciones:**

1. En Copilot Studio, localiza el panel de **Test** (Prueba) en la esquina inferior derecha o superior derecha de la interfaz. Haz clic en el ícono de chat/test para abrirlo.
2. Si el panel de prueba muestra un mensaje de bienvenida anterior, haz clic en **Reset** o **Start over** para iniciar una conversación limpia.
3. Escribe la **Pregunta 1** y presiona Enter:

```
¿Cuántos días de vacaciones me corresponden al cumplir 3 años en TechNova?
```

4. Observa la respuesta del agente. Verifica si:
   - Menciona información específica del documento de política de vacaciones.
   - Incluye una cita o referencia al documento fuente.
   - La respuesta es coherente y no genérica.

5. Escribe la **Pregunta 2**:

```
¿Cómo puedo restablecer mi contraseña de acceso al correo corporativo?
```

6. Verifica que la respuesta proviene del FAQ de Soporte TI (debe mencionar pasos específicos documentados en el DOCX).

7. Escribe la **Pregunta 3**:

```
¿Qué cobertura tiene el seguro médico para dependientes económicos?
```

8. Confirma que la respuesta incluye detalles del catálogo de beneficios.

9. Escribe la **Pregunta 4** (pregunta que NO está en los documentos):

```
¿Cuál es la capital de Francia?
```

10. Observa cómo responde el agente. Según las instrucciones del Lab 03-00-02, debería indicar que esa pregunta está fuera de su alcance o responder de forma limitada sin citar documentos.

11. Escribe la **Pregunta 5** (pregunta que combina información de múltiples documentos):

```
Si estoy de vacaciones y necesito conectarme a la VPN por una emergencia, ¿cuál es el procedimiento?
```

12. Evalúa si el agente combina información del documento de vacaciones y del FAQ de TI.

13. Para cada respuesta, observa en el panel de test si aparece una sección de **References** o **Citations** que indica qué documento se utilizó.

**Resultado esperado:**

| # | Pregunta | Fuente esperada | ¿Respuesta basada en documento? |
|---|----------|-----------------|----------------------------------|
| 1 | Días de vacaciones a 3 años | `TechNova_Politica_Vacaciones_2024.pdf` | ✅ Sí |
| 2 | Restablecer contraseña | `TechNova_FAQ_Soporte_IT.docx` | ✅ Sí |
| 3 | Cobertura seguro médico dependientes | `TechNova_Catalogo_Beneficios_2024.pdf` | ✅ Sí |
| 4 | Capital de Francia | Ninguna (fuera de alcance) | ❌ No / Respuesta limitada |
| 5 | VPN durante vacaciones | Combinación de FAQ TI + Vacaciones | ✅ Sí (parcial o completa) |

**Verificación:**
- Al menos 3 de las 5 preguntas obtienen respuestas con información específica de los documentos.
- Las respuestas basadas en documentos incluyen datos concretos (números, procedimientos, nombres) que solo existen en los archivos cargados.
- La Pregunta 4 NO genera una respuesta con cita de documento (confirma que el agente distingue entre conocimiento interno y externo).
- Captura de pantalla de cada respuesta: `C:/AgenteLabs/capturas/lab03-03-test-pregunta[N].png` (donde N = 1 a 5).

---

### Paso 7: Documentar resultados y análisis

**Objetivo:** Registrar los resultados de las pruebas en un formato estructurado que evidencie la correcta integración de conocimiento y permita identificar áreas de mejora.

**Instrucciones:**

1. Abre Visual Studio Code o Notepad++.
2. Crea un nuevo archivo en `C:/AgenteLabs/docs/` llamado `lab03-03-resultados-test.md`.
3. Completa el siguiente template con tus observaciones:

```markdown
# Resultados de Prueba - Integración de Conocimiento
## Agente: Nova Assistant
## Fecha: [FECHA]
## Participante: [NOMBRE]

## Documentos Cargados
| # | Documento | Estado | Tiempo indexación |
|---|-----------|--------|-------------------|
| 1 | TechNova_Politica_Vacaciones_2024.pdf | Ready | [X] min |
| 2 | TechNova_FAQ_Soporte_IT.docx | Ready | [X] min |
| 3 | TechNova_Catalogo_Beneficios_2024.pdf | Ready | [X] min |

## Configuración Generative Answers
- Habilitado: Sí
- Nivel de confianza: Medium
- Citas activadas: Sí/No

## Resultados de Preguntas de Prueba

### Pregunta 1: [Pregunta completa]
- **Respuesta del agente:** [Resumen de la respuesta]
- **¿Basada en documento?:** Sí/No
- **Documento citado:** [Nombre del archivo]
- **Calidad (1-5):** [Puntuación]
- **Observaciones:** [Notas]

### Pregunta 2: [Pregunta completa]
[Repetir estructura...]

### Pregunta 3: [Pregunta completa]
[Repetir estructura...]

### Pregunta 4: [Pregunta completa]
[Repetir estructura...]

### Pregunta 5: [Pregunta completa]
[Repetir estructura...]

## Análisis
- Preguntas con respuesta basada en documento: [X]/5
- Preguntas con respuesta genérica: [X]/5
- Preguntas fuera de alcance manejadas correctamente: [X]/1

## Conclusiones
[Escribir 2-3 oraciones sobre la efectividad de la integración de conocimiento]
```

4. Guarda el archivo.
5. Opcionalmente, copia este contenido a tu bitácora en Notion (`Bitácora-Lab-02-03` en el workspace `IA-Agentes-Lab-Workspace`).

**Resultado esperado:**
- Archivo `lab03-03-resultados-test.md` creado y completado con datos reales de las pruebas.
- Documentación clara que diferencia respuestas RAG de respuestas genéricas.

**Verificación:**
- El archivo existe en `C:/AgenteLabs/docs/lab03-03-resultados-test.md`.
- Contiene datos de las 5 preguntas con sus resultados.
- Las puntuaciones de calidad reflejan la evaluación honesta del participante.

---

## Validación y Pruebas Finales

Para considerar este laboratorio como **completado exitosamente**, verifica los siguientes criterios:

### Lista de verificación final

| # | Criterio | Estado |
|---|----------|--------|
| 1 | Los 3 documentos están cargados en la sección Knowledge del agente | ☐ |
| 2 | Los 3 documentos muestran estado "Ready" (indexación completada) | ☐ |
| 3 | Generative Answers está habilitado con nivel de confianza "Medium" | ☐ |
| 4 | Al menos 3 de 5 preguntas de prueba obtienen respuestas basadas en los documentos | ☐ |
| 5 | La pregunta fuera de alcance (Pregunta 4) NO genera una respuesta con cita de documento | ☐ |
| 6 | Las respuestas incluyen información específica (datos concretos) que solo existe en los documentos | ☐ |
| 7 | El archivo `lab03-03-resultados-test.md` está creado y documentado | ☐ |
| 8 | Las capturas de pantalla están guardadas en `C:/AgenteLabs/capturas/` | ☐ |

### Prueba adicional de validación

Realiza una pregunta final que combine un tema del documento con el tono configurado en las instrucciones del Lab 03-00-02:

```
Hola, soy nuevo en TechNova. ¿Podrías explicarme brevemente cómo solicitar mis vacaciones y qué beneficios de salud tengo disponibles?
```

**Criterio de éxito:** El agente debe responder con el tono definido en sus instrucciones personalizadas Y utilizar información de al menos 2 documentos diferentes (política de vacaciones + catálogo de beneficios).

---

## Solución de Problemas

### Problema 1: Documento permanece en estado "Processing" por más de 15 minutos

**Síntomas:**
- Uno o más documentos muestran estado "Processing" indefinidamente.
- El ícono de carga/spinner no desaparece después de refrescar múltiples veces.
- Los otros documentos sí cambiaron a "Ready".

**Causa:**
El archivo puede tener un formato interno no estándar (PDF con capas de imagen sin OCR, DOCX con macros o contenido embebido complejo) que dificulta la extracción de texto por parte de Azure AI Search. También puede deberse a una sobrecarga temporal del servicio de indexación en el tenant compartido.

**Solución:**
1. Espera 5 minutos adicionales y refresca con `F5`.
2. Si persiste, elimina el documento problemático haciendo clic en los tres puntos (`...`) junto al archivo y seleccionando **Delete** o **Remove**.
3. Abre el documento original y verifica que contiene texto seleccionable (no imágenes escaneadas).
4. Si el PDF es una imagen escaneada, solicita al instructor una versión con texto nativo.
5. Vuelve a cargar el documento individual usando **+ Add knowledge > Files**.
6. Si el problema persiste en todo el entorno, contacta al instructor — puede ser un límite temporal del tenant de práctica.

---

### Problema 2: El agente no utiliza los documentos y responde con información genérica

**Síntomas:**
- Al hacer preguntas sobre contenido de los documentos, el agente responde con información general (ej: "Generalmente las empresas ofrecen 15 días de vacaciones...").
- No aparecen citas ni referencias a los documentos cargados.
- El panel de test no muestra indicadores de que se consultaron fuentes de conocimiento.

**Causa:**
Las respuestas generativas (Generative Answers) pueden no estar correctamente habilitadas, el nivel de confianza puede estar configurado en "High" (demasiado restrictivo), o los documentos no están asociados como fuentes activas para el agente. También puede ocurrir si la pregunta no contiene términos suficientemente similares al contenido indexado.

**Solución:**
1. Navega a **Settings > Generative AI** y confirma que Generative Answers está en **ON**.
2. Verifica que el nivel de confianza está en **Medium** (no en High).
3. Ve a la sección **Knowledge** y confirma que los 3 documentos tienen estado "Ready" y están marcados como activos (no deshabilitados).
4. En el panel de test, haz clic en **Reset** para iniciar una conversación limpia (el contexto anterior puede interferir).
5. Reformula la pregunta usando términos más específicos que coincidan con el contenido del documento. Ejemplo: en lugar de "¿cuántas vacaciones tengo?", prueba "¿cuántos días de vacaciones corresponden según la política de TechNova 2024?".
6. Si ninguna solución funciona, verifica en la configuración del agente que no hay un Topic (tema) con mayor prioridad que intercepta la pregunta antes de que llegue al sistema de Generative Answers.

---

## Limpieza

> **⚠️ NO elimines los documentos ni el agente al finalizar este laboratorio.**

Los artefactos generados en este laboratorio son requisito para el **Lab 03-00-04**:

- **Agente Nova Assistant** con 3 fuentes de conocimiento indexadas y activas.
- **Configuración de Generative Answers** habilitada con nivel Medium.

### Acciones de limpieza permitidas:

1. Cierra las pestañas adicionales del navegador que no necesites.
2. Cierra Adobe Acrobat Reader DC si permanece abierto.
3. Organiza tus capturas de pantalla en `C:/AgenteLabs/capturas/` con nombres descriptivos.
4. Verifica que el archivo `lab03-03-resultados-test.md` está guardado correctamente.

---

## Resumen

### Lo que lograste en este laboratorio:

1. **Verificaste documentos** empresariales simulados para confirmar su compatibilidad con Copilot Studio (formato, permisos, tamaño).
2. **Cargaste 3 fuentes de conocimiento** en la sección Knowledge del agente, utilizando la funcionalidad de Upload files.
3. **Monitoreaste el proceso de indexación** de Azure AI Search, comprendiendo que Copilot Studio fragmenta documentos en chunks y genera embeddings vectoriales.
4. **Configuraste Generative Answers** con nivel de confianza Medium para balancear precisión y cobertura.
5. **Ejecutaste pruebas estructuradas** con 5 preguntas que validaron la capacidad RAG del agente, diferenciando respuestas basadas en documentos de respuestas genéricas.
6. **Documentaste los resultados** en un formato reproducible para análisis posterior.

### Conceptos clave reforzados:

| Concepto | Descripción |
|----------|-------------|
| **RAG (Retrieval Augmented Generation)** | Técnica que combina búsqueda en documentos indexados con generación de texto por el modelo LLM |
| **Azure AI Search** | Motor de indexación subyacente que procesa y almacena los chunks de documentos en Copilot Studio |
| **Chunks** | Fragmentos del documento original que se indexan individualmente para búsqueda semántica |
| **Embeddings** | Representaciones vectoriales del texto que permiten búsqueda por similitud semántica |
| **Nivel de confianza** | Umbral que determina cuánta similitud debe existir entre la pregunta y el documento para generar una respuesta |

### Conexión con la Lección 3.1:

Este laboratorio demuestra en la práctica por qué **Microsoft Copilot Studio** es la plataforma recomendada para agentes con conocimiento empresarial en organizaciones que usan Microsoft 365. La integración nativa con Azure AI Search para indexación de documentos, la configuración visual de Generative Answers y la publicación en Teams son capacidades que otras plataformas (como GPTs de OpenAI o Claude) no ofrecen con el mismo nivel de integración corporativa.

### Recursos adicionales:

- [Documentación: Add knowledge to your copilot](https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-copilot-studio)
- [Documentación: Generative answers in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-boost-conversations)
- [Conceptos de Azure AI Search](https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search)
- [Mejores prácticas para documentos de conocimiento en Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-best-practices)

---

---

# 7 Laboratorio: Verificación de comportamiento de agente (Test)

## Metadata

| Campo | Detalle |
|-------|---------|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |
| **Plataforma principal** | Microsoft Copilot Studio (Release Wave 1 2024) |
| **Modelo bajo prueba** | GPT-4o (gpt-4o-2024-05-13) |

## Descripción General

En este laboratorio someterás al agente **Nova Assistant** (configurado en el Lab 03-00-03) a un proceso sistemático de verificación de comportamiento. Diseñarás y ejecutarás 8 casos de prueba organizados en 4 categorías —positivos, sin respuesta explícita, fuera de alcance y casos de borde— registrando los resultados en una plantilla estructurada. El producto final es un plan de pruebas completado que servirá como insumo para el refinamiento de instrucciones en el Lab 03-00-05.

## Objetivos de Aprendizaje

- [ ] Diseñar y ejecutar un plan de pruebas estructurado con al menos 8 casos de prueba que cubran escenarios positivos, negativos y de borde para el agente «Nova Assistant».
- [ ] Identificar y documentar desviaciones entre el comportamiento esperado (definido en las instrucciones del Lab 03-00-02) y el comportamiento real del agente en cada caso de prueba.
- [ ] Priorizar al menos 3 áreas de mejora específicas en las instrucciones o configuración del agente basándose en los resultados de las pruebas.

## Prerrequisitos

### Conocimientos previos

| Requisito | Descripción |
|-----------|-------------|
| Lab 03-00-03 completado | Agente «Nova Assistant» con GPT-4o, instrucciones v1 y 3 documentos de conocimiento indexados con estado **Ready** |
| Instrucciones del agente | Comprensión del comportamiento esperado según `nova_instructions_v1.txt` (Lab 03-00-02) |
| Metodología de pruebas | Familiaridad con conceptos de casos positivos, negativos y de borde |

### Acceso y herramientas

| Herramienta | Versión / Detalle |
|-------------|-------------------|
| Microsoft Copilot Studio | Web GA — Release Wave 1 2024 |
| Entorno | `LabPractice-M2` (tipo Sandbox, región United States) |
| Microsoft Excel / Google Sheets | Excel 2021 (16.0.14332.20400) o Google Sheets |
| Plantilla de pruebas | `TechNova_TestPlan_Template.xlsx` (provista por el instructor) |
| Navegador | Edge 120+ o Chrome 120+ |

## Entorno de Laboratorio

### Estructura de carpetas requerida

```
C:\AgenteLabs\
└── testing\
    ├── TechNova_TestPlan_Template.xlsx   ← plantilla descargada
    └── TechNova_TestPlan_Completed_v1.xlsx  ← producto final
```

> **Nota (macOS/Linux):** Sustituir `C:\AgenteLabs\` por `~/AgenteLabs/`.

### Verificación previa rápida

1. Abre el navegador y navega a `https://copilotstudio.microsoft.com/environments/LabPractice-M2`.
2. Confirma que el agente **Nova Assistant** aparece en la lista de agentes y que los 3 documentos de conocimiento muestran estado **Ready**.
3. Confirma que el archivo `TechNova_TestPlan_Template.xlsx` existe en `C:\AgenteLabs\testing\`.

---

## Paso a Paso

### Paso 1 — Abrir la plantilla de plan de pruebas y comprender su estructura

**Objetivo:** Familiarizarse con las secciones de la plantilla para saber exactamente qué información registrar durante la ejecución.

**Instrucciones:**

1. Abre `C:\AgenteLabs\testing\TechNova_TestPlan_Template.xlsx` en Microsoft Excel o Google Sheets.
2. Identifica las siguientes columnas en la hoja **Casos de Prueba**:

| Columna | Contenido esperado |
|---------|--------------------|
| **ID** | Identificador único (TC-01, TC-02…) |
| **Categoría** | Una de: Alcance-Con Respuesta, Alcance-Sin Respuesta, Fuera de Alcance, Caso de Borde |
| **Pregunta exacta** | Texto literal que enviarás al agente |
| **Comportamiento esperado** | Lo que el agente debería hacer según sus instrucciones |
| **Respuesta obtenida** | Texto real devuelto por el agente |
| **Resultado** | Pasa / Falla |
| **Observaciones** | Notas sobre desviaciones o hallazgos |

3. Identifica la hoja **Áreas de Mejora** con columnas: ID, Descripción, Prioridad (Alta/Media/Baja), Justificación.
4. Guarda el archivo sin modificar por ahora.

**Resultado esperado:** Comprensión clara de la estructura de registro. La plantilla tiene dos hojas: «Casos de Prueba» y «Áreas de Mejora».

**Verificación:** Las columnas descritas coinciden con las que ves en la plantilla. Si la plantilla tiene variaciones menores del instructor, adapta los nombres de columna según corresponda.

---

### Paso 2 — Diseñar los 8 casos de prueba

**Objetivo:** Redactar las preguntas y definir el comportamiento esperado antes de ejecutar las pruebas, asegurando cobertura de las 4 categorías.

**Instrucciones:**

1. En la hoja **Casos de Prueba**, completa las columnas **ID**, **Categoría**, **Pregunta exacta** y **Comportamiento esperado** para 8 casos distribuidos así:

**Categoría A — Dentro del alcance CON respuesta en documentos (2 casos):**

| ID | Pregunta exacta | Comportamiento esperado |
|----|-----------------|------------------------|
| TC-01 | ¿Cuántos días de vacaciones tengo por año? | Responde con la cantidad de días según la política de RRHH documentada, citando o parafraseando la fuente. |
| TC-02 | ¿Cómo solicito acceso a la VPN corporativa? | Proporciona los pasos del procedimiento de TI documentado para solicitar acceso VPN. |

**Categoría B — Dentro del alcance SIN respuesta explícita en documentos (2 casos):**

| ID | Pregunta exacta | Comportamiento esperado |
|----|-----------------|------------------------|
| TC-03 | ¿Puedo tomar vacaciones en diciembre si acabo de entrar a la empresa? | Indica que no tiene información específica sobre restricciones por antigüedad y sugiere consultar a RRHH directamente. |
| TC-04 | ¿Qué pasa si pierdo mi laptop corporativa? | Si no hay procedimiento documentado, indica que no dispone de esa información y redirige al área correspondiente. |

**Categoría C — Fuera del alcance del agente (2 casos):**

| ID | Pregunta exacta | Comportamiento esperado |
|----|-----------------|------------------------|
| TC-05 | ¿Cuál es la capital de Francia? | Declina responder indicando que su función es asistir con temas internos de TechNova. No responde la pregunta general. |
| TC-06 | Escríbeme un poema sobre el mar | Declina la solicitud creativa y redirige al usuario a formular preguntas dentro de su ámbito. |

**Categoría D — Casos de borde o ambiguos (2 casos):**

| ID | Pregunta exacta | Comportamiento esperado |
|----|-----------------|------------------------|
| TC-07 | How many vacation days do I get per year? | Responde en español (o según las instrucciones del agente respecto al idioma) y proporciona la información solicitada, o bien solicita que reformule en español. |
| TC-08 | komo pido bacaciomes? | Interpreta la intención a pesar de los errores ortográficos y proporciona información sobre cómo solicitar vacaciones, o pide aclaración cortésmente. |

2. Ajusta las preguntas si tu agente tiene documentos con contenido diferente; lo importante es mantener la distribución 2-2-2-2 por categoría.
3. Guarda el archivo.

**Resultado esperado:** 8 filas completadas con ID, Categoría, Pregunta exacta y Comportamiento esperado. Las columnas de Respuesta obtenida, Resultado y Observaciones permanecen vacías.

**Verificación:** Cuenta las filas: deben ser exactamente 8. Verifica que cada categoría tiene exactamente 2 casos.

---

### Paso 3 — Ejecutar los casos de prueba en el panel de prueba de Copilot Studio

**Objetivo:** Obtener las respuestas reales del agente para cada caso de prueba y registrarlas fielmente.

**Instrucciones:**

1. En el navegador, accede a Copilot Studio: `https://copilotstudio.microsoft.com/environments/LabPractice-M2`.
2. Selecciona el agente **Nova Assistant** de la lista.
3. En la parte superior derecha del editor, haz clic en el botón **Test** (icono de chat con rayo) para abrir el panel de prueba lateral.
4. Si el panel muestra un mensaje de carga o actualización, espera hasta que indique **"Ready to chat"** o equivalente.
5. Ejecuta los casos de prueba en orden:

   **Para TC-01:**
   - Escribe en el panel de prueba: `¿Cuántos días de vacaciones tengo por año?`
   - Presiona **Enter** o el botón de enviar.
   - Espera la respuesta completa del agente.
   - Copia la respuesta textual y pégala en la columna **Respuesta obtenida** de TC-01 en la plantilla Excel.

   **Para TC-02:**
   - Escribe: `¿Cómo solicito acceso a la VPN corporativa?`
   - Registra la respuesta en la plantilla.

   **Para TC-03:**
   - Escribe: `¿Puedo tomar vacaciones en diciembre si acabo de entrar a la empresa?`
   - Registra la respuesta.

   **Para TC-04:**
   - Escribe: `¿Qué pasa si pierdo mi laptop corporativa?`
   - Registra la respuesta.

   **Para TC-05:**
   - Escribe: `¿Cuál es la capital de Francia?`
   - Registra la respuesta.

   **Para TC-06:**
   - Escribe: `Escríbeme un poema sobre el mar`
   - Registra la respuesta.

   **Para TC-07:**
   - Escribe: `How many vacation days do I get per year?`
   - Registra la respuesta.

   **Para TC-08:**
   - Escribe: `komo pido bacaciomes?`
   - Registra la respuesta.

6. Entre cada caso de prueba, haz clic en el ícono de **reiniciar conversación** (flecha circular) en la parte superior del panel de prueba para garantizar que cada caso se evalúa sin contexto residual de la pregunta anterior.

**Resultado esperado:** Las 8 celdas de la columna «Respuesta obtenida» contienen el texto real devuelto por el agente. Cada respuesta fue capturada en una conversación independiente (sin contexto previo).

**Verificación:** Revisa que ninguna celda de «Respuesta obtenida» esté vacía. Si alguna respuesta fue cortada, vuelve al panel de prueba y repite solo ese caso.

---

### Paso 4 — Evaluar resultados: Pasa o Falla

**Objetivo:** Comparar el comportamiento real contra el esperado y asignar un veredicto a cada caso.

**Instrucciones:**

1. Para cada fila (TC-01 a TC-08), compara la columna **Respuesta obtenida** con la columna **Comportamiento esperado**.
2. Asigna el valor en la columna **Resultado** según estos criterios:

   | Veredicto | Criterio |
   |-----------|----------|
   | **Pasa** | La respuesta cumple con el comportamiento esperado en su totalidad o con variaciones menores que no afectan la funcionalidad ni la adherencia a las instrucciones. |
   | **Falla** | La respuesta contradice el comportamiento esperado, omite información crítica, responde fuera de alcance cuando no debería, o no sigue las restricciones definidas en las instrucciones. |

3. En la columna **Observaciones**, documenta:
   - Para casos que **Pasan**: cualquier detalle positivo notable (ej: «Citó correctamente el documento de políticas de RRHH»).
   - Para casos que **Fallan**: la desviación específica (ej: «Respondió la pregunta general sobre Francia en lugar de declinar», «Respondió en inglés en vez de español»).

4. Guarda el archivo.

**Resultado esperado:** Las 8 filas tienen un valor en la columna Resultado (Pasa o Falla) y observaciones relevantes. Es normal que al menos 2-3 casos presenten fallas; esto es el insumo para la mejora.

**Verificación:** Suma los resultados. Un plan de pruebas realista típicamente muestra entre 50% y 80% de tasa de éxito en la primera iteración. Si obtienes 100% de éxito, revisa si tus criterios de evaluación son suficientemente rigurosos.

---

### Paso 5 — Analizar patrones e identificar áreas de mejora

**Objetivo:** Sintetizar los hallazgos en mejoras accionables y priorizadas que alimentarán el refinamiento del agente.

**Instrucciones:**

1. Revisa todos los casos marcados como **Falla** y agrupa las desviaciones por tipo:
   - ¿El agente respondió preguntas fuera de alcance? → Problema de **restricciones/guardrails**.
   - ¿El agente no supo manejar otro idioma? → Problema de **manejo de idioma** en instrucciones.
   - ¿El agente inventó información no presente en los documentos? → Problema de **alucinación/grounding**.
   - ¿El agente no redirigió al usuario cuando no tenía respuesta? → Problema de **fallback/escalación**.

2. Navega a la hoja **Áreas de Mejora** en la plantilla.

3. Completa al menos 3 filas con el siguiente formato:

| ID | Descripción | Prioridad | Justificación |
|----|-------------|-----------|---------------|
| AM-01 | Agregar instrucción explícita para declinar preguntas de conocimiento general no relacionadas con TechNova | Alta | TC-05 y TC-06 fallaron: el agente respondió preguntas fuera de alcance sin redirigir |
| AM-02 | Incluir directiva de manejo de idioma: responder siempre en español y solicitar reformulación si la pregunta viene en otro idioma | Media | TC-07 falló: el agente respondió en inglés en lugar de mantener el español |
| AM-03 | Añadir instrucción de tolerancia a errores ortográficos: interpretar la intención del usuario cuando hay typos evidentes | Baja | TC-08 presentó respuesta parcial; el agente pidió reformulación en vez de interpretar |

4. Ajusta las descripciones según tus resultados reales. Los ejemplos anteriores son orientativos.

5. Asegúrate de que las prioridades siguen esta lógica:
   - **Alta**: Afecta la confiabilidad o seguridad del agente (responde fuera de alcance, inventa datos).
   - **Media**: Afecta la experiencia del usuario pero no la seguridad (idioma incorrecto, tono inadecuado).
   - **Baja**: Mejora deseable pero no crítica (mejor formato de respuesta, manejo de edge cases poco frecuentes).

6. Guarda el archivo como `TechNova_TestPlan_Completed_v1.xlsx` en `C:\AgenteLabs\testing\`.

**Resultado esperado:** La hoja «Áreas de Mejora» contiene al menos 3 entradas con descripción clara, prioridad asignada y justificación basada en evidencia de los casos de prueba.

**Verificación:** Cada área de mejora referencia al menos un caso de prueba específico (TC-XX) como evidencia. Las prioridades están distribuidas (no todas son «Alta»).

---

## Validación y Pruebas Finales

Antes de considerar el laboratorio completado, verifica los siguientes criterios de aceptación:

| # | Criterio | Estado |
|---|----------|--------|
| 1 | El archivo `TechNova_TestPlan_Completed_v1.xlsx` existe en `C:\AgenteLabs\testing\` | ☐ |
| 2 | La hoja «Casos de Prueba» contiene exactamente 8 filas con todas las columnas completadas | ☐ |
| 3 | Las 4 categorías están representadas con 2 casos cada una | ☐ |
| 4 | Cada caso tiene una respuesta obtenida real (no inventada ni hipotética) | ☐ |
| 5 | Cada caso tiene un veredicto Pasa/Falla con observaciones | ☐ |
| 6 | La hoja «Áreas de Mejora» tiene al menos 3 entradas con prioridad y justificación | ☐ |
| 7 | Al menos una mejora tiene prioridad «Alta» | ☐ |
| 8 | Las justificaciones referencian casos de prueba específicos (TC-XX) | ☐ |

---

## Solución de Problemas

### Problema 1: El panel de prueba no responde o muestra "Something went wrong"

**Síntomas:** Al escribir una pregunta en el panel de prueba de Copilot Studio, aparece un mensaje de error genérico, el indicador de carga gira indefinidamente, o el panel no muestra ninguna respuesta después de 30 segundos.

**Causa:** Los documentos de conocimiento no terminaron de indexarse correctamente, o hay un problema temporal de conexión con el servicio de Azure OpenAI subyacente. También puede ocurrir si el agente fue modificado recientemente y no se publicó la versión actualizada al entorno de prueba.

**Solución:**
1. Cierra el panel de prueba haciendo clic en la **X**.
2. Navega a la sección **Knowledge** del agente y verifica que los 3 documentos muestran estado **Ready** (no «Indexing» ni «Error»).
3. Si algún documento muestra error, elimínalo y vuelve a cargarlo.
4. Regresa al editor principal y haz clic en **Test** nuevamente.
5. Si persiste, cierra el navegador completamente, borra la caché y vuelve a acceder a Copilot Studio.
6. Como último recurso, espera 2-3 minutos (el servicio puede tener latencia temporal) y reintenta.

---

### Problema 2: El agente responde "No tengo información sobre eso" a TODAS las preguntas, incluso las que están en los documentos

**Síntomas:** Tanto TC-01 como TC-02 (preguntas con respuesta esperada en documentos) reciben una respuesta genérica de tipo «No tengo información disponible para responder esa pregunta» o similar, como si el agente no tuviera acceso a ningún documento.

**Causa:** La conexión entre el agente y las fuentes de conocimiento se rompió o los documentos fueron indexados en un formato que el modelo no puede consultar. Otra causa frecuente es que las instrucciones del agente contengan una restricción demasiado agresiva que impide al modelo utilizar los documentos (ej: «Solo responde si estás 100% seguro»).

**Solución:**
1. Ve a **Knowledge** en el panel izquierdo de Copilot Studio.
2. Haz clic en cada documento y verifica que el toggle **"Use in conversations"** está activado.
3. Abre la sección **Topics** → **System** → **Conversational boosting** y confirma que está habilitado.
4. Revisa las instrucciones del agente (sección **Instructions**): busca frases que puedan estar bloqueando respuestas (restricciones excesivas). Temporalmente, suaviza la restricción para la prueba.
5. Reinicia el panel de prueba y ejecuta TC-01 nuevamente.
6. Si funciona, documenta en tus observaciones que las instrucciones necesitan ajuste de umbral de confianza (esto será una «Área de Mejora» válida).

---

## Limpieza

Este laboratorio no requiere eliminar recursos, ya que el agente **Nova Assistant** será utilizado en el Lab 03-00-05 para refinamiento. Los únicos elementos generados son:

- **Conservar:** `TechNova_TestPlan_Completed_v1.xlsx` en `C:\AgenteLabs\testing\` — es insumo obligatorio para el siguiente laboratorio.
- **Conservar:** La plantilla original `TechNova_TestPlan_Template.xlsx` como respaldo.
- **No modificar:** Las instrucciones ni documentos del agente en Copilot Studio. Los cambios se realizarán en el Lab 03-00-05 basándose en los hallazgos de este laboratorio.

---

## Resumen

En este laboratorio aplicaste una metodología estructurada de verificación de comportamiento para el agente **Nova Assistant**:

1. **Diseñaste** 8 casos de prueba distribuidos en 4 categorías que cubren el espectro completo de interacciones posibles.
2. **Ejecutaste** cada caso en el panel de prueba de Copilot Studio, registrando respuestas reales de forma aislada (sin contexto residual).
3. **Evaluaste** los resultados comparando comportamiento esperado vs. real, identificando desviaciones concretas.
4. **Priorizaste** al menos 3 áreas de mejora con justificación basada en evidencia.

### Conceptos clave reforzados

- La verificación sistemática de agentes requiere cobertura de escenarios positivos, negativos y de borde.
- Un agente rara vez funciona perfectamente en la primera iteración; las pruebas estructuradas son el mecanismo para identificar brechas antes de la puesta en producción.
- La priorización de mejoras permite enfocar el esfuerzo de refinamiento en los problemas de mayor impacto.

### Conexión con el siguiente laboratorio

El archivo `TechNova_TestPlan_Completed_v1.xlsx` será el insumo principal del **Lab 03-00-05**, donde refinarás las instrucciones del agente para corregir las desviaciones detectadas. Las áreas de mejora priorizadas como «Alta» serán las primeras en abordarse.

### Recursos adicionales

- [Documentación: Probar tu agente en Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-test-bot)
- [Mejores prácticas de pruebas para agentes conversacionales](https://learn.microsoft.com/es-es/microsoft-copilot-studio/guidance/testing-best-practices)
- [Metodología de testing para sistemas de IA generativa — NIST AI 600-1](https://www.nist.gov/artificial-intelligence)

---

# Refinamiento de Instrucciones y Pruebas con Diferentes Modelos de IA Generativa

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 20 minutos |
| **Complejidad** | Alta |
| **Nivel Bloom** | Aplicar |
| **Módulo** | 3 — Panorama de Plataformas y Modelos de IA Generativa |
| **Dependencias** | Labs 03-00-01, 03-00-02, 03-00-03, 03-00-04 |

---

## Descripción General

Este laboratorio de cierre del módulo integra todas las competencias desarrolladas en los laboratorios anteriores. Realizarás un ciclo completo de refinamiento iterativo de las instrucciones del agente **Nova Assistant**, compararás su comportamiento utilizando al menos dos modelos de IA generativa diferentes (GPT-4o vs GPT-3.5 Turbo en Copilot Studio, y opcionalmente GPT-4o en ChatGPT GPT Builder), y seleccionarás la configuración final óptima basándote en evidencia cuantitativa de las pruebas comparativas.

---

## Objetivos de Aprendizaje

Al completar este laboratorio serás capaz de:

- [ ] Refinar las instrucciones del agente en al menos 2 iteraciones documentadas, aplicando mejoras basadas en los resultados del Lab 03-00-04
- [ ] Comparar el comportamiento del agente con al menos 2 modelos de IA generativa diferentes utilizando los mismos 8 casos de prueba estandarizados
- [ ] Documentar un changelog de instrucciones que registre qué cambió entre cada versión y por qué
- [ ] Analizar cuantitativamente las diferencias de rendimiento entre modelos mediante una tabla comparativa multidimensional
- [ ] Seleccionar y justificar la configuración final óptima (instrucciones + modelo) con base en evidencia empírica

---

## Prerrequisitos

### Conocimientos Previos

| Requisito | Fuente |
|-----------|--------|
| Diseño de instrucciones (system prompt) para agentes | Lab 03-00-02 |
| Integración de documentos de conocimiento en Copilot Studio | Lab 03-00-03 |
| Ejecución de plan de pruebas con 8 casos y documentación de resultados | Lab 03-00-04 |
| Comprensión de diferencias entre modelos GPT-4o y GPT-3.5 Turbo | Lección 3.1 y 3.2 |

### Acceso y Archivos Requeridos

| Elemento | Ubicación / Detalle |
|----------|---------------------|
| Agente **Nova Assistant** activo | Copilot Studio — entorno `LabPractice-M2` |
| `nova_instructions_v1.txt` | `C:/AgenteLabs/` |
| `TechNova_TestPlan_Completed_v1.xlsx` | `C:/AgenteLabs/` (con 8 casos ejecutados y áreas de mejora) |
| 3 documentos de conocimiento activos | Cargados en el agente (Labs 03-00-01/03) |
| Cuenta Microsoft 365 con Copilot Studio | `usuario[N]@labagentes[N].onmicrosoft.com` |
| Cuenta ChatGPT Plus (opcional) | `chat.openai.com/gpts` |

---

## Entorno de Laboratorio

### Software Requerido

| Software | Versión | Propósito |
|----------|---------|-----------|
| Microsoft Copilot Studio | Release Wave 1 2024 | Plataforma principal del agente |
| Notepad++ o Visual Studio Code | 8.6.4 / 1.89.1 | Edición y versionado de instrucciones |
| Microsoft Excel | 2021 (16.0.14332.20400) | Análisis comparativo de resultados |
| Google Chrome o Microsoft Edge | 124.x | Navegador para acceso a plataformas |
| ChatGPT (opcional) | GPT-4o (gpt-4o-2024-05-13) | Comparación en plataforma alternativa |

### Verificación del Entorno

Antes de iniciar, confirma que todos los archivos están en su lugar:

```
dir C:\AgenteLabs\
```

**Salida esperada:**
```
nova_instructions_v1.txt
TechNova_TestPlan_Completed_v1.xlsx
[documentos de conocimiento .pdf/.docx]
```

Si usas macOS/Linux:
```bash
ls ~/AgenteLabs/
```

---

## Procedimiento Paso a Paso

### Paso 1: Análisis de Áreas de Mejora del Lab 03-00-04

**Objetivo:** Identificar y priorizar las mejoras específicas a implementar en las instrucciones del agente.

**Instrucciones:**

1. Abre `TechNova_TestPlan_Completed_v1.xlsx` en Microsoft Excel.
2. Filtra los casos de prueba con resultado **"Fallo"** o **"Parcial"** en la columna de estado.
3. Revisa la columna de observaciones/áreas de mejora para cada caso problemático.
4. Clasifica las mejoras necesarias en tres categorías:
   - **Tono/Estilo:** El agente no respeta el tono definido (ej. demasiado informal, demasiado extenso).
   - **Precisión de contenido:** El agente proporciona información incorrecta o incompleta.
   - **Manejo de límites:** El agente responde preguntas fuera de alcance o no redirige adecuadamente.
5. Crea una lista priorizada (de mayor a menor impacto) en un nuevo archivo de texto:

```
Archivo: C:\AgenteLabs\mejoras_priorizadas.txt
```

Contenido de ejemplo:
```
PRIORIDAD 1 - [Categoría]: [Descripción del problema] → [Mejora propuesta]
PRIORIDAD 2 - [Categoría]: [Descripción del problema] → [Mejora propuesta]
PRIORIDAD 3 - [Categoría]: [Descripción del problema] → [Mejora propuesta]
```

**Resultado esperado:** Un archivo con al menos 3 mejoras priorizadas que guiarán la iteración de instrucciones.

**Verificación:** Confirma que cada mejora tiene una categoría clara, una descripción del problema observado y una acción correctiva específica.

---

### Paso 2: Creación de nova_instructions_v2.txt (Primera Iteración)

**Objetivo:** Implementar las mejoras de mayor prioridad en una nueva versión de las instrucciones del agente.

**Instrucciones:**

1. Abre `nova_instructions_v1.txt` en Visual Studio Code o Notepad++:

```
code C:\AgenteLabs\nova_instructions_v1.txt
```

2. Guarda inmediatamente una copia como `nova_instructions_v2.txt`:

```
Archivo → Guardar como → C:\AgenteLabs\nova_instructions_v2.txt
```

3. Implementa las mejoras de **Prioridad 1 y 2** identificadas en el Paso 1. Los tipos de refinamiento más comunes incluyen:

**a) Si el problema es de tono/estilo**, agrega o modifica la sección de personalidad:

```markdown
## Estilo de Comunicación
- Responde siempre en un tono profesional pero accesible.
- Limita las respuestas a un máximo de 150 palabras a menos que el usuario solicite más detalle.
- Usa viñetas para listas de más de 3 elementos.
- Nunca uses jerga técnica sin explicarla brevemente.
```

**b) Si el problema es de precisión**, refuerza las instrucciones de uso de fuentes:

```markdown
## Uso de Fuentes de Conocimiento
- SIEMPRE basa tus respuestas en los documentos cargados como fuente de conocimiento.
- Si la información solicitada NO está en los documentos disponibles, indica: "No tengo información verificada sobre ese tema en mis fuentes actuales."
- NO inventes datos, cifras, nombres de productos o políticas que no estén explícitamente en los documentos.
- Cuando cites información de un documento, menciona la fuente: "Según [nombre del documento]..."
```

**c) Si el problema es de manejo de límites**, añade restricciones explícitas:

```markdown
## Restricciones y Límites
- Tu alcance se limita EXCLUSIVAMENTE a temas de [dominio definido para TechNova].
- Si el usuario pregunta sobre temas fuera de tu alcance, responde: "Esa consulta está fuera de mi área de especialización. Te recomiendo contactar a [canal alternativo]."
- Nunca proporciones asesoría legal, médica o financiera específica.
- Si detectas ambigüedad en la pregunta, solicita clarificación antes de responder.
```

4. Revisa que la estructura general del prompt mantenga coherencia (rol → objetivos → restricciones → formato → ejemplos).

5. Guarda el archivo `nova_instructions_v2.txt`.

**Resultado esperado:** Un archivo `nova_instructions_v2.txt` con mejoras concretas y trazables respecto a v1.

**Verificación:** Compara lado a lado ambos archivos. Cada cambio debe corresponder directamente a un problema identificado en el Paso 1.

---

### Paso 3: Actualización y Prueba en Copilot Studio (v2)

**Objetivo:** Aplicar las instrucciones v2 al agente y re-ejecutar los casos de prueba que fallaron.

**Instrucciones:**

1. Accede a Copilot Studio: `https://copilotstudio.microsoft.com/environments/LabPractice-M2`

2. Abre el agente **Nova Assistant** desde la lista de agentes.

3. Navega a la sección **Instrucciones** (Instructions) del agente:
   - En el panel lateral izquierdo, selecciona **Descripción general** o **Overview**.
   - Localiza el campo de instrucciones del sistema.

4. Selecciona todo el texto existente y reemplázalo con el contenido completo de `nova_instructions_v2.txt`.

5. Haz clic en **Guardar** (Save) en la parte superior.

6. Abre el panel de **Prueba** (Test) en la esquina inferior izquierda de Copilot Studio.

7. Re-ejecuta **únicamente** los casos de prueba que fallaron o fueron parciales en el Lab 03-00-04. Para cada caso:
   - Escribe el prompt de prueba exacto del TestPlan.
   - Evalúa la respuesta contra los criterios de aceptación definidos.
   - Registra el resultado en `TechNova_TestPlan_Completed_v1.xlsx` en una nueva columna titulada **"Resultado v2 (GPT-4o)"**.

8. Anota si los problemas se resolvieron, mejoraron parcialmente o persisten.

**Resultado esperado:** Al menos el 50% de los casos previamente fallidos deben mostrar mejora con las instrucciones v2.

**Verificación:** La columna "Resultado v2 (GPT-4o)" en el Excel muestra los nuevos resultados. Si todos los casos pasan, puedes omitir el Paso 4. Si persisten fallos, continúa al Paso 4.

---

### Paso 4: Creación de nova_instructions_v3.txt (Segunda Iteración — si es necesario)

**Objetivo:** Realizar una segunda iteración de refinamiento para los casos que aún no pasan las pruebas.

**Instrucciones:**

1. Identifica los casos que aún fallan con v2. Analiza el patrón:
   - ¿El agente ignora una instrucción específica? → Hazla más explícita y posiciónala al inicio.
   - ¿El agente confunde el alcance? → Agrega ejemplos concretos de qué SÍ y qué NO responder.
   - ¿Las respuestas son genéricas? → Añade instrucciones de formato y profundidad esperada.

2. Copia `nova_instructions_v2.txt` a `nova_instructions_v3.txt`:

```
copy C:\AgenteLabs\nova_instructions_v2.txt C:\AgenteLabs\nova_instructions_v3.txt
```

3. Abre `nova_instructions_v3.txt` y aplica ajustes adicionales. Técnicas avanzadas de refinamiento:

**Técnica de ejemplos (Few-shot en instrucciones):**
```markdown
## Ejemplos de Comportamiento Esperado

Pregunta del usuario: "¿Cuál es la política de devoluciones?"
Respuesta esperada: "Según el documento de Políticas Comerciales de TechNova, las devoluciones se aceptan dentro de los 30 días posteriores a la compra, siempre que el producto esté en su empaque original. ¿Necesitas más detalles sobre algún caso específico?"

Pregunta del usuario: "¿Me puedes ayudar con mi declaración de impuestos?"
Respuesta esperada: "Esa consulta está fuera de mi área de especialización. Mi función es asistirte con temas relacionados con [dominio TechNova]. Para temas fiscales, te recomiendo consultar con un contador certificado."
```

**Técnica de priorización explícita:**
```markdown
## Orden de Prioridad en Respuestas
1. PRIMERO: Verifica si la pregunta está dentro de tu alcance.
2. SEGUNDO: Busca la respuesta en los documentos de conocimiento cargados.
3. TERCERO: Formula la respuesta respetando el tono y formato definidos.
4. CUARTO: Si no encuentras información, indícalo transparentemente.
```

4. Guarda `nova_instructions_v3.txt`.

5. Actualiza las instrucciones en Copilot Studio (mismo procedimiento del Paso 3).

6. Re-ejecuta los casos aún pendientes y registra resultados en columna **"Resultado v3 (GPT-4o)"**.

**Resultado esperado:** Archivo `nova_instructions_v3.txt` con refinamientos adicionales y mejora demostrable en los casos de prueba.

**Verificación:** Compara los resultados v1 → v2 → v3. Debe haber una tendencia clara de mejora progresiva.

---

### Paso 5: Documentación del Changelog de Instrucciones

**Objetivo:** Crear un registro formal de los cambios realizados entre cada versión de instrucciones.

**Instrucciones:**

1. Crea el archivo `C:\AgenteLabs\instructions_changelog.txt` en VS Code o Notepad++:

```
code C:\AgenteLabs\instructions_changelog.txt
```

2. Completa el archivo con la siguiente estructura:

```markdown
# Changelog de Instrucciones - Nova Assistant
# Autor: [Tu nombre]
# Fecha: [Fecha actual]

## v1 → v2 (Primera iteración)
### Fecha: [YYYY-MM-DD]
### Problemas abordados:
- [Caso de prueba #X]: [Descripción del problema]
- [Caso de prueba #Y]: [Descripción del problema]

### Cambios realizados:
1. [Sección modificada]: [Descripción del cambio y justificación]
2. [Sección agregada]: [Descripción del cambio y justificación]
3. [Sección eliminada/reformulada]: [Descripción del cambio y justificación]

### Resultado:
- Casos mejorados: [lista]
- Casos sin cambio: [lista]

---

## v2 → v3 (Segunda iteración)
### Fecha: [YYYY-MM-DD]
### Problemas abordados:
- [Caso de prueba #Z]: [Descripción del problema persistente]

### Cambios realizados:
1. [Descripción del cambio y justificación]
2. [Descripción del cambio y justificación]

### Resultado:
- Casos mejorados: [lista]
- Casos sin cambio: [lista]

---

## Versión Final Seleccionada: v[N]
### Justificación: [Por qué esta versión es la óptima]
```

3. Guarda el archivo.

**Resultado esperado:** Un changelog completo que permita a cualquier persona entender la evolución de las instrucciones y las razones detrás de cada cambio.

**Verificación:** El archivo contiene al menos 2 secciones de cambios (v1→v2 y v2→v3) con justificaciones claras.

---

### Paso 6: Comparación con Modelo GPT-3.5 Turbo en Copilot Studio

**Objetivo:** Evaluar el impacto del cambio de modelo en el rendimiento del agente manteniendo las mismas instrucciones.

**Instrucciones:**

1. En Copilot Studio, con el agente **Nova Assistant** abierto, navega a **Configuración** (Settings) → **IA Generativa** (Generative AI) o **Modelo** (Model).

2. Localiza la opción de selección de modelo. Cambia de **GPT-4o** a **GPT-3.5 Turbo**.

   > **Nota:** La ubicación exacta puede variar según la versión de Copilot Studio. Busca en:
   > - Settings → AI capabilities → Model selection
   > - O en la configuración del tema generativo (Generative answers)

3. Asegúrate de que las instrucciones sean la **última versión refinada** (v2 o v3, la que mejor resultados haya dado con GPT-4o).

4. Guarda la configuración.

5. Abre el panel de **Prueba** y ejecuta los **8 casos de prueba completos** del TestPlan original (no solo los que fallaron). Para cada caso:
   - Usa exactamente el mismo prompt de entrada.
   - Evalúa contra los mismos criterios de aceptación.
   - Registra el resultado.

6. En `TechNova_TestPlan_Completed_v1.xlsx`, agrega una nueva columna titulada **"Resultado GPT-3.5 Turbo"** y registra los 8 resultados.

7. Para cada caso, anota observaciones cualitativas:
   - ¿La respuesta es más corta/larga que con GPT-4o?
   - ¿Se pierde precisión o coherencia?
   - ¿El agente sigue las instrucciones de formato?
   - ¿El manejo de casos fuera de alcance es adecuado?

**Resultado esperado:** Una columna completa con 8 resultados para GPT-3.5 Turbo, con observaciones cualitativas que permitan comparar con GPT-4o.

**Verificación:** Todos los 8 casos tienen resultado registrado. Las diferencias observadas están documentadas en la columna de observaciones.

---

### Paso 7: Comparación con ChatGPT GPT Builder (Opcional pero Recomendado)

**Objetivo:** Evaluar el mismo conjunto de instrucciones en una plataforma alternativa para validar la portabilidad y el rendimiento cross-platform.

**Instrucciones:**

> **Nota:** Este paso requiere una cuenta ChatGPT Plus o Team. Si no la tienes, salta al Paso 8.

1. Accede a `https://chat.openai.com/gpts/editor` o navega a ChatGPT → Explorar GPTs → Crear.

2. En la pestaña **Configurar** (Configure):
   - **Nombre:** `Nova Assistant - Lab Test`
   - **Descripción:** `Agente de prueba para comparación de plataformas - Lab 03-00-05`
   - **Instrucciones:** Copia y pega el contenido completo de tu versión final de instrucciones (`nova_instructions_v2.txt` o `nova_instructions_v3.txt`).

3. En la sección **Conocimiento** (Knowledge):
   - Sube los mismos 3 documentos de conocimiento que están en Copilot Studio.
   - Espera a que se procesen completamente.

4. En **Capacidades** (Capabilities):
   - Desactiva "Navegación web" (Web Browsing) para mantener la comparación justa.
   - Desactiva "Generación de imágenes" (DALL·E).
   - Mantén activo "Intérprete de código" solo si es relevante para tu caso de uso.

5. Haz clic en **Guardar** → **Solo yo** (Only me).

6. Abre el GPT creado y ejecuta los **8 casos de prueba**:
   - Para cada caso, inicia una nueva conversación (para evitar contaminación de contexto).
   - Usa exactamente el mismo prompt.
   - Evalúa contra los mismos criterios.

7. Registra los resultados en una nueva columna **"ChatGPT GPT-4o"** en tu Excel.

**Resultado esperado:** 8 resultados documentados en la columna "ChatGPT GPT-4o" con observaciones comparativas.

**Verificación:** Los resultados permiten identificar diferencias atribuibles a la plataforma (no al modelo, ya que ambos usan GPT-4o).

---

### Paso 8: Análisis Comparativo y Selección de Configuración Final

**Objetivo:** Consolidar los resultados en una tabla comparativa multidimensional y seleccionar la configuración óptima con justificación.

**Instrucciones:**

1. En `TechNova_TestPlan_Completed_v1.xlsx`, crea una nueva hoja llamada **"Comparativa de Modelos"**.

2. Construye la siguiente tabla de análisis (ajusta según los modelos que hayas probado):

| Dimensión | GPT-4o (Copilot Studio) | GPT-3.5 Turbo (Copilot Studio) | ChatGPT GPT-4o (opcional) |
|-----------|------------------------|-------------------------------|--------------------------|
| Casos aprobados (de 8) | _/8 | _/8 | _/8 |
| Calidad de respuesta (1-5) | | | |
| Adherencia a instrucciones (1-5) | | | |
| Manejo de fuera de alcance (1-5) | | | |
| Velocidad percibida | Rápida/Media/Lenta | | |
| Precisión factual | Alta/Media/Baja | | |
| Formato de respuesta | Cumple/Parcial/No cumple | | |

**Escala de evaluación:**
- 5 = Excelente, cumple completamente los criterios
- 4 = Bueno, cumple con observaciones menores
- 3 = Aceptable, cumple parcialmente
- 2 = Deficiente, fallos significativos
- 1 = Inaceptable, no cumple

3. Calcula un **puntaje total** para cada configuración sumando las puntuaciones de las dimensiones numéricas.

4. En la misma hoja, agrega una sección de **"Decisión Final"** con el siguiente formato:

```
CONFIGURACIÓN FINAL SELECCIONADA:
- Modelo: [GPT-4o / GPT-3.5 Turbo]
- Plataforma: [Copilot Studio / ChatGPT GPT Builder]
- Versión de instrucciones: [v2 / v3]

JUSTIFICACIÓN:
[Párrafo de 3-5 oraciones explicando por qué esta combinación es la óptima,
referenciando datos específicos de la tabla comparativa. Ejemplo: "Se selecciona
GPT-4o en Copilot Studio con instrucciones v3 porque obtuvo 8/8 casos aprobados,
un puntaje de adherencia de 5/5 y ofrece integración nativa con el ecosistema
Microsoft 365 de la organización. GPT-3.5 Turbo, aunque más rápido, falló en
2 casos de manejo de límites y mostró menor precisión factual (3/5 vs 5/5)."]

TRADE-OFFS ACEPTADOS:
- [Ej: Mayor costo de tokens con GPT-4o vs menor precisión con GPT-3.5]
- [Ej: Dependencia de plataforma Microsoft vs portabilidad de ChatGPT]
```

5. Guarda el archivo como `TechNova_TestPlan_Completed_v2.xlsx`:

```
Archivo → Guardar como → C:\AgenteLabs\TechNova_TestPlan_Completed_v2.xlsx
```

**Resultado esperado:** Archivo Excel con tabla comparativa completa, puntajes calculados y decisión final justificada.

**Verificación:** La decisión final está respaldada por al menos 3 datos cuantitativos de la tabla comparativa.

---

### Paso 9: Restauración de Configuración Final y Entrega

**Objetivo:** Dejar el agente en su estado óptimo final y preparar los entregables.

**Instrucciones:**

1. En Copilot Studio, restaura el modelo al seleccionado como ganador:
   - Si la configuración ganadora es GPT-4o, asegúrate de que esté configurado como modelo activo.
   - Si habías cambiado a GPT-3.5 Turbo en el Paso 6, revierte el cambio.

2. Confirma que las instrucciones corresponden a la versión final seleccionada.

3. Crea la copia final de las instrucciones:

```
copy C:\AgenteLabs\nova_instructions_v[N].txt C:\AgenteLabs\nova_instructions_vFinal.txt
```

Donde `[N]` es el número de versión seleccionada (2 o 3).

4. Realiza una prueba de humo final: ejecuta 2 casos de prueba representativos (uno dentro de alcance y uno fuera de alcance) para confirmar que el agente funciona correctamente.

5. Verifica que todos los entregables están en `C:\AgenteLabs\`:

```
dir C:\AgenteLabs\
```

**Entregables finales requeridos:**

| Archivo | Descripción |
|---------|-------------|
| `nova_instructions_v1.txt` | Versión original (del Lab 03-00-02) |
| `nova_instructions_v2.txt` | Primera iteración de refinamiento |
| `nova_instructions_v3.txt` | Segunda iteración (si aplica) |
| `nova_instructions_vFinal.txt` | Versión final seleccionada |
| `instructions_changelog.txt` | Registro de cambios entre versiones |
| `TechNova_TestPlan_Completed_v2.xlsx` | Plan de pruebas con comparativa de modelos |
| `mejoras_priorizadas.txt` | Lista de mejoras del Paso 1 |

**Resultado esperado:** Directorio `C:\AgenteLabs\` con todos los archivos listados y el agente en Copilot Studio configurado con la versión final.

**Verificación:** Ejecuta `dir C:\AgenteLabs\*.txt C:\AgenteLabs\*.xlsx` y confirma que aparecen al menos 6 archivos.

---

## Validación y Pruebas

### Criterios de Éxito del Laboratorio

| # | Criterio | Método de Verificación | Cumple (Sí/No) |
|---|----------|----------------------|-----------------|
| 1 | Existen al menos 2 versiones de instrucciones (v2 y v3 o v2 y vFinal) | Verificar archivos en `C:\AgenteLabs\` | |
| 2 | El changelog documenta cambios específicos con justificación | Revisar `instructions_changelog.txt` | |
| 3 | Se probaron al menos 2 modelos diferentes | Verificar columnas en Excel | |
| 4 | Los 8 casos de prueba se ejecutaron con cada modelo | Contar resultados en Excel | |
| 5 | La tabla comparativa tiene puntajes en todas las dimensiones | Revisar hoja "Comparativa de Modelos" | |
| 6 | La decisión final incluye justificación con datos cuantitativos | Leer sección "Decisión Final" en Excel | |
| 7 | El agente está restaurado a la configuración ganadora | Probar en panel de Test de Copilot Studio | |

### Prueba de Validación Final

Ejecuta esta secuencia en el panel de prueba de Copilot Studio para confirmar el estado final:

1. **Pregunta dentro de alcance:** Escribe una pregunta que sabes está cubierta por los documentos de conocimiento.
   - ✅ Esperado: Respuesta precisa, con tono correcto, citando la fuente.

2. **Pregunta fuera de alcance:** "¿Cuál es la capital de Francia?"
   - ✅ Esperado: Redirección educada indicando que está fuera de su alcance.

3. **Pregunta ambigua:** Escribe algo vago como "¿Qué opciones tengo?"
   - ✅ Esperado: Solicitud de clarificación antes de responder.

---

## Solución de Problemas

### Problema 1: No se encuentra la opción de cambiar modelo en Copilot Studio

**Síntomas:** Al navegar a la configuración del agente, no aparece la opción para seleccionar entre GPT-4o y GPT-3.5 Turbo. Solo se muestra un modelo predeterminado sin posibilidad de cambio.

**Causa:** La selección de modelo en Copilot Studio depende de la configuración del entorno y las licencias disponibles. En algunos tenants, la opción de modelo está controlada a nivel de administrador y no está expuesta en la interfaz del creador de agentes. También puede ocurrir que la funcionalidad esté en una sección diferente según la versión de Release Wave.

**Solución:**
1. Navega a **Configuración** (Settings) → **IA Generativa** (Generative AI).
2. Si no aparece allí, busca en **Temas** (Topics) → selecciona un tema con respuesta generativa → **Nodo de respuesta generativa** → icono de configuración (engranaje) → opción de modelo.
3. Si la opción no está disponible en ninguna ubicación, documenta esta limitación en tu tabla comparativa y realiza la comparación únicamente entre Copilot Studio (GPT-4o) y ChatGPT GPT Builder (GPT-4o), enfocando el análisis en diferencias de plataforma en lugar de diferencias de modelo.
4. Consulta con el instructor si el tenant tiene restricciones de modelo.

---

### Problema 2: Las instrucciones refinadas empeoran el rendimiento en algunos casos

**Síntomas:** Después de aplicar `nova_instructions_v2.txt`, algunos casos que antes pasaban ahora fallan. Por ejemplo, al agregar restricciones más estrictas para manejar preguntas fuera de alcance, el agente comienza a rechazar preguntas legítimas que están dentro de su dominio.

**Causa:** Las instrucciones son demasiado restrictivas o contradictorias. Agregar reglas de exclusión muy amplias puede causar que el modelo interprete preguntas válidas como fuera de alcance. Esto es un efecto secundario conocido del "over-constraining" en prompt engineering.

**Solución:**
1. No descartes la v2 completa. Identifica específicamente qué instrucción nueva causa la regresión.
2. En `nova_instructions_v3.txt`, modifica la restricción problemática agregando **excepciones explícitas**:
   ```markdown
   ## Restricciones (ACTUALIZADO)
   - Rechaza preguntas sobre temas NO relacionados con TechNova.
   - EXCEPCIÓN: Preguntas sobre productos, servicios, políticas o procesos de TechNova SIEMPRE están dentro de tu alcance, incluso si el usuario las formula de manera indirecta o genérica.
   ```
3. Agrega un ejemplo positivo (qué SÍ responder) junto a cada ejemplo negativo (qué NO responder) para dar al modelo un marco de referencia balanceado.
4. Re-ejecuta tanto los casos que fallaron originalmente como los que regresaron para confirmar que la v3 resuelve ambos problemas sin crear nuevas regresiones.

---

## Limpieza

### Acciones Post-Laboratorio

1. **En Copilot Studio:**
   - Confirma que el agente **Nova Assistant** queda con la configuración final (modelo ganador + instrucciones vFinal).
   - NO elimines el agente — será referenciado en evaluaciones posteriores.

2. **En ChatGPT (si aplica):**
   - El GPT de prueba `Nova Assistant - Lab Test` puede eliminarse si no se requiere para evaluación:
     - Navega a `chat.openai.com/gpts/mine`
     - Selecciona el GPT → Editar → Eliminar GPT

3. **Archivos locales:**
   - Mantén todos los archivos en `C:\AgenteLabs\` hasta que el instructor confirme la entrega.
   - Opcionalmente, crea un respaldo comprimido:

```
cd C:\AgenteLabs
tar -czf C:\AgenteLabs\backup_lab05_final.zip *.txt *.xlsx
```

4. **Registro en Notion (si aplica):**
   - Actualiza la página `Bitácora-Lab-02-04` (o la página designada por el instructor) con un resumen de los resultados comparativos y la decisión final.

---

## Resumen

### Logros Completados

En este laboratorio has ejecutado un ciclo profesional completo de refinamiento de agentes de IA:

| Fase | Actividad Realizada | Entregable |
|------|---------------------|------------|
| Diagnóstico | Análisis de fallos del Lab 03-00-04 | `mejoras_priorizadas.txt` |
| Iteración 1 | Refinamiento de instrucciones | `nova_instructions_v2.txt` |
| Iteración 2 | Ajustes adicionales basados en pruebas | `nova_instructions_v3.txt` |
| Comparación | Pruebas con múltiples modelos | Columnas adicionales en Excel |
| Decisión | Selección justificada de configuración | Tabla comparativa + justificación |
| Documentación | Registro de evolución | `instructions_changelog.txt` |

### Conceptos Clave Aplicados

- **Refinamiento iterativo:** Las instrucciones de un agente rara vez son óptimas en su primera versión. El ciclo prueba → análisis → mejora → prueba es la metodología estándar.
- **Comparación controlada de modelos:** Cambiar una sola variable (el modelo) manteniendo todo lo demás constante permite atribuir diferencias de rendimiento al modelo específico.
- **Decisión basada en evidencia:** La selección de tecnología en entornos empresariales debe fundamentarse en datos, no en preferencias subjetivas.
- **Trazabilidad:** El changelog permite auditar decisiones y facilita la colaboración en equipos donde múltiples personas iteran sobre el mismo agente.

### Conexión con el Panorama de Plataformas (Lección 3.1)

Este laboratorio demuestra en la práctica los criterios de comparación estudiados en la Lección 3.1:
- **Facilidad de configuración:** Experimentaste la diferencia entre actualizar instrucciones en Copilot Studio vs crear un GPT en ChatGPT.
- **Adherencia a instrucciones:** Verificaste cómo diferentes modelos interpretan el mismo prompt de maneras distintas.
- **Integración empresarial:** La decisión final probablemente favorece a Copilot Studio por su integración con el ecosistema Microsoft 365 del entorno de práctica.

### Recursos Adicionales

- [Mejores prácticas para instrucciones en Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/authoring-create-edit-topics)
- [Guía de selección de modelos en Azure OpenAI](https://learn.microsoft.com/es-es/azure/ai-services/openai/concepts/models)
- [Prompt Engineering Guide — OpenAI](https://platform.openai.com/docs/guides/prompt-engineering)
- [Comparativa de modelos GPT-4o vs GPT-3.5 Turbo](https://platform.openai.com/docs/models)

---
