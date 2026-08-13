# Integración de herramientas en un agente

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 50 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |
| **Entorno** | Microsoft Copilot Studio – Entorno `LabPractice-M2` |
| **Agente resultante** | `AgenteEmpresarialLab` publicado en `LabAgentesEnv` |

---

## Descripción General

En este laboratorio crearás un agente funcional en Microsoft Copilot Studio denominado **AgenteEmpresarialLab** y lo conectarás con dos herramientas externas: un conector nativo de SharePoint Online para leer ítems de la lista `SolicitudesLab`, y un conector HTTP personalizado que consume la API pública JSONPlaceholder. Aplicarás el patrón ReAct (Razonar → Actuar → Observar) de forma práctica al definir acciones que el agente invoca durante la conversación. Al finalizar, el agente será capaz de responder preguntas sobre solicitudes empresariales y datos de una API externa, estableciendo la base tecnológica para los laboratorios subsiguientes del módulo 4.

---

## Objetivos de Aprendizaje

- [ ] Configurar y conectar al menos dos herramientas externas (conector de SharePoint y conector HTTP personalizado) dentro de un agente en Microsoft Copilot Studio.
- [ ] Definir acciones del agente (`ObtenerSolicitudes` y `ConsultarAPI`) que invoquen herramientas externas para recuperar información de fuentes de datos empresariales.
- [ ] Probar la integración de herramientas verificando que el agente responde correctamente usando datos obtenidos de SharePoint Online y de la API JSONPlaceholder.
- [ ] Documentar el esquema de acciones configuradas en formato reutilizable para laboratorios posteriores.

---

## Prerrequisitos

### Conocimientos previos

| Requisito | Detalle |
|-----------|---------|
| Agentes conversacionales | Comprensión de qué es un agente y cómo funciona Copilot Studio (Módulos 1-3) |
| SharePoint Online | Navegación básica en sitios y listas |
| APIs REST | Concepto de endpoint, métodos HTTP GET, códigos de respuesta |
| JSON | Lectura e interpretación de objetos y arrays |

### Acceso y cuentas

| Recurso | Detalle |
|---------|---------|
| Cuenta Microsoft 365 | `usuario[N]@labagentes[N].onmicrosoft.com` con licencia E3 + Copilot |
| Copilot Studio | Acceso a `https://copilotstudio.microsoft.com/environments/LabPractice-M2` |
| SharePoint | Sitio `https://labagentes[N].sharepoint.com/sites/LabAgentes` con lista `SolicitudesLab` (≥5 ítems) |
| Postman | Versión 10.22.0 instalada con cuenta activa |
| Visual Studio Code | Versión 1.85.1+ con extensión Power Platform Tools 2.0.19 |

---

## Entorno del Laboratorio

### Estructura de carpetas local

Antes de iniciar, verifica que la estructura de directorios exista:

**Windows:**
```powershell
# Ejecutar en PowerShell
New-Item -ItemType Directory -Force -Path "C:\LabAgentes\prompts"
New-Item -ItemType Directory -Force -Path "C:\LabAgentes\capturas"
New-Item -ItemType Directory -Force -Path "C:\LabAgentes\docs"
```

**macOS/Linux:**
```bash
mkdir -p ~/LabAgentes/{prompts,capturas,docs}
```

### Lista SharePoint `SolicitudesLab` — estructura requerida

| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Título | Línea de texto | Solicitud de acceso VPN |
| Descripción | Múltiples líneas | Requiero acceso VPN para trabajo remoto |
| Estado | Opción (Pendiente/En Proceso/Completada) | Pendiente |
| Solicitante | Línea de texto | María García |

> **Nota:** El instructor debe haber creado el sitio y la lista con al menos 5 ítems antes del inicio del laboratorio. Verifica accediendo a `https://labagentes[N].sharepoint.com/sites/LabAgentes/Lists/SolicitudesLab`.

### Software requerido

| Herramienta | Versión | Propósito |
|-------------|---------|-----------|
| Microsoft Edge / Chrome | 120+ | Acceso a Copilot Studio y SharePoint |
| Postman | 10.22.0 | Validación de llamadas HTTP previas a integración |
| Visual Studio Code | 1.85.1+ | Edición de esquemas JSON de acciones |
| Microsoft Copilot Studio | GA (Wave 1 2024) | Plataforma principal de construcción del agente |

---

## Paso a Paso

### Paso 1: Validar la API externa con Postman

**Objetivo:** Confirmar que el endpoint público JSONPlaceholder responde correctamente antes de integrarlo al agente.

**Instrucciones:**

1. Abre **Postman** en tu equipo.
2. Crea una nueva colección llamada `Lab04-Integraciones`.
3. Dentro de la colección, crea una nueva petición (request):
   - **Nombre:** `GET Todos`
   - **Método:** `GET`
   - **URL:** `https://jsonplaceholder.typicode.com/todos`
4. Haz clic en **Send**.
5. Observa la respuesta. Debes recibir un array JSON con 200 objetos.
6. Ahora crea una segunda petición para probar con filtro:
   - **Nombre:** `GET Todo por ID`
   - **Método:** `GET`
   - **URL:** `https://jsonplaceholder.typicode.com/todos/1`
7. Haz clic en **Send**.
8. Guarda la colección.

**Resultado esperado:**

Respuesta de la petición `GET Todo por ID`:
```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

Código de estado: `200 OK`. Tiempo de respuesta: < 2 segundos.

**Verificación:**
- [ ] La petición `GET Todos` retorna status `200` con un array de 200 elementos.
- [ ] La petición `GET Todo por ID` retorna status `200` con un objeto individual.
- [ ] Captura de pantalla guardada en `C:\LabAgentes\capturas\paso1-postman-validacion.png`.

---

### Paso 2: Crear el agente en Microsoft Copilot Studio

**Objetivo:** Crear el agente base `AgenteEmpresarialLab` en el entorno `LabPractice-M2` con configuración inicial.

**Instrucciones:**

1. Abre el navegador y navega a:
   ```
   https://copilotstudio.microsoft.com/environments/LabPractice-M2
   ```
2. Inicia sesión con tus credenciales `usuario[N]@labagentes[N].onmicrosoft.com`.
3. En la barra lateral izquierda, verifica que el entorno seleccionado sea **LabPractice-M2** (esquina superior derecha del portal).
4. Haz clic en **+ Crear** (o **+ Create**) en el panel de navegación.
5. Selecciona **Nuevo agente** (New agent).
6. En el campo **Nombre**, escribe: `AgenteEmpresarialLab`
7. En el campo **Descripción**, escribe:
   ```
   Agente empresarial de laboratorio que integra herramientas de SharePoint y APIs externas para recuperar información de solicitudes y tareas.
   ```
8. En la sección **Instrucciones** (Instructions), ingresa el siguiente system prompt:
   ```
   Eres un asistente empresarial llamado AgenteEmpresarialLab. Tu función es ayudar a los usuarios a consultar solicitudes internas almacenadas en SharePoint y verificar tareas pendientes desde sistemas externos. 
   
   Reglas:
   - Responde siempre en español.
   - Cuando el usuario pregunte por solicitudes, usa la acción ObtenerSolicitudes.
   - Cuando el usuario pregunte por tareas o todos, usa la acción ConsultarAPI.
   - Si no tienes información suficiente, pide aclaraciones.
   - No inventes datos; solo reporta lo que las herramientas devuelvan.
   ```
9. Haz clic en **Crear** (Create).
10. Espera a que el agente se aprovisione (10-30 segundos).

**Resultado esperado:**

Se muestra el canvas de edición del agente `AgenteEmpresarialLab` con el panel de temas a la izquierda y el panel de pruebas a la derecha.

**Verificación:**
- [ ] El agente aparece listado en el entorno `LabPractice-M2`.
- [ ] El nombre es exactamente `AgenteEmpresarialLab`.
- [ ] Las instrucciones del sistema están guardadas correctamente.
- [ ] Captura de pantalla: `C:\LabAgentes\capturas\paso2-agente-creado.png`.

---

### Paso 3: Configurar el conector de SharePoint (Acción `ObtenerSolicitudes`)

**Objetivo:** Crear una acción basada en Power Automate que conecte el agente con la lista `SolicitudesLab` de SharePoint Online.

**Instrucciones:**

1. En el canvas del agente `AgenteEmpresarialLab`, navega a la sección **Acciones** (Actions) en el panel superior.
2. Haz clic en **+ Agregar una acción** (+ Add an action).
3. Selecciona **Crear un nuevo flujo de Power Automate** (Create a new Power Automate flow).
4. Se abrirá Power Automate en una nueva pestaña con un flujo preconfigurado para Copilot Studio.
5. Renombra el flujo como: `ObtenerSolicitudes-Lab04`
6. El flujo ya tiene un trigger **"Cuando Copilot Studio llama a un flujo"** (When Copilot Studio calls a flow). Mantén este trigger.
7. Haz clic en **+ Nuevo paso** (+ New step) y busca el conector **SharePoint**.
8. Selecciona la acción **Obtener elementos** (Get items).
9. Configura los parámetros:
   - **Dirección del sitio:** `https://labagentes[N].sharepoint.com/sites/LabAgentes`
   - **Nombre de lista:** `SolicitudesLab`
   - **Número máximo de elementos:** `10`
10. Haz clic en **+ Nuevo paso** y selecciona **Responder a Copilot Studio** (Respond to Copilot Studio).
11. En la respuesta, agrega una variable de salida:
    - **Nombre:** `solicitudes`
    - **Tipo:** String
    - Haz clic en el campo de valor y selecciona la expresión:
      ```
      body('Obtener_elementos')?['value']
      ```
    - Si la interfaz lo permite, usa la expresión completa:
      ```
      string(body('Obtener_elementos')?['value'])
      ```
12. Guarda el flujo haciendo clic en **Guardar** (Save).
13. Regresa a la pestaña de Copilot Studio.
14. En el diálogo de selección de acción, busca y selecciona `ObtenerSolicitudes-Lab04`.
15. Configura la descripción de la acción para el agente:
    ```
    Obtiene la lista de solicitudes empresariales almacenadas en SharePoint. Usa esta acción cuando el usuario pregunte por solicitudes, peticiones o requerimientos internos.
    ```
16. Haz clic en **Agregar** (Add) para vincular la acción al agente.

**Resultado esperado:**

La acción `ObtenerSolicitudes-Lab04` aparece listada en la sección de Acciones del agente. El flujo de Power Automate muestra estado **Activado** (On).

**Verificación:**
- [ ] El flujo `ObtenerSolicitudes-Lab04` existe en Power Automate y está activado.
- [ ] La acción aparece en la lista de acciones del agente en Copilot Studio.
- [ ] La conexión a SharePoint muestra estado válido (ícono verde o sin errores).
- [ ] Captura de pantalla: `C:\LabAgentes\capturas\paso3-accion-sharepoint.png`.

---

### Paso 4: Configurar el conector HTTP personalizado (Acción `ConsultarAPI`)

**Objetivo:** Crear una segunda acción que consuma el endpoint JSONPlaceholder mediante un conector HTTP dentro de un flujo de Power Automate.

**Instrucciones:**

1. En Copilot Studio, dentro del agente `AgenteEmpresarialLab`, ve nuevamente a **Acciones** > **+ Agregar una acción** > **Crear un nuevo flujo de Power Automate**.
2. Renombra el nuevo flujo como: `ConsultarAPI-Lab04`
3. Mantén el trigger de Copilot Studio.
4. Agrega un paso de entrada al trigger:
   - **Nombre del parámetro:** `taskId`
   - **Tipo:** Texto (Text)
   - **Descripción:** `ID de la tarea a consultar (opcional, si está vacío retorna todas)`
5. Haz clic en **+ Nuevo paso** y busca la acción **HTTP**.

   > **Importante:** La acción HTTP requiere una licencia Premium de Power Automate. Si no está disponible, usa la alternativa **HTTP con Azure AD** o solicita al instructor la habilitación del conector.

6. Configura la acción HTTP:
   - **Método:** `GET`
   - **URI:** Usa la siguiente expresión para manejar el parámetro opcional:
     ```
     https://jsonplaceholder.typicode.com/todos/@{if(empty(triggerBody()?['text']), '', triggerBody()?['text'])}
     ```
   - Si la expresión condicional no es soportada, usa la URI simple:
     ```
     https://jsonplaceholder.typicode.com/todos/1
     ```
   - **Headers:** (dejar vacío)
   - **Body:** (dejar vacío)

7. Haz clic en **+ Nuevo paso** y selecciona **Responder a Copilot Studio**.
8. Agrega una variable de salida:
   - **Nombre:** `apiResponse`
   - **Tipo:** String
   - **Valor:** `string(body('HTTP'))`
9. Guarda el flujo.
10. Regresa a Copilot Studio y selecciona el flujo `ConsultarAPI-Lab04`.
11. Configura la descripción de la acción:
    ```
    Consulta tareas desde una API externa. Usa esta acción cuando el usuario pregunte por tareas pendientes, TODOs o actividades del sistema externo.
    ```
12. En la configuración de entradas, mapea el parámetro `taskId`:
    - **Descripción para el agente:** `El identificador numérico de la tarea que el usuario quiere consultar`
13. Haz clic en **Agregar** (Add).

**Resultado esperado:**

Dos acciones configuradas en el agente:
- `ObtenerSolicitudes-Lab04` → SharePoint
- `ConsultarAPI-Lab04` → HTTP/JSONPlaceholder

**Verificación:**
- [ ] El flujo `ConsultarAPI-Lab04` está guardado y activado en Power Automate.
- [ ] La acción aparece en la lista de acciones del agente junto a `ObtenerSolicitudes-Lab04`.
- [ ] El parámetro de entrada `taskId` está correctamente definido.
- [ ] Captura de pantalla: `C:\LabAgentes\capturas\paso4-accion-http.png`.

---

### Paso 5: Configurar el tema de bienvenida y orquestación

**Objetivo:** Crear un tema de bienvenida que informe al usuario sobre las capacidades del agente y asegurar que la orquestación de acciones esté habilitada.

**Instrucciones:**

1. En el canvas del agente, navega a **Temas** (Topics) en el panel lateral.
2. Localiza el tema **Greeting** (Saludo) que se crea por defecto.
3. Haz clic para editarlo y modifica el mensaje de bienvenida:
   ```
   ¡Hola! Soy AgenteEmpresarialLab. Puedo ayudarte con:
   
   📋 **Solicitudes internas** — Consulta el estado de solicitudes empresariales registradas en SharePoint.
   ✅ **Tareas externas** — Verifica tareas pendientes desde nuestro sistema de gestión.
   
   ¿En qué puedo ayudarte hoy?
   ```
4. Guarda el tema.
5. Navega a **Configuración** (Settings) > **IA generativa** (Generative AI).
6. Verifica que la opción **Orquestación generativa** (Generative orchestration) esté **habilitada**. Esta configuración permite al agente decidir automáticamente cuándo invocar una acción basándose en el contexto de la conversación (patrón ReAct).
7. Si no está habilitada, actívala y guarda.

**Resultado esperado:**

El tema de saludo muestra el mensaje personalizado y la orquestación generativa está activa, permitiendo al agente decidir de forma autónoma cuándo invocar `ObtenerSolicitudes` o `ConsultarAPI`.

**Verificación:**
- [ ] El tema Greeting muestra el mensaje personalizado al abrir el panel de pruebas.
- [ ] La orquestación generativa está habilitada en la configuración de IA.
- [ ] Captura de pantalla: `C:\LabAgentes\capturas\paso5-tema-bienvenida.png`.

---

### Paso 6: Probar la integración en el panel de pruebas

**Objetivo:** Validar que el agente invoca correctamente ambas acciones y retorna datos reales.

**Instrucciones:**

1. En Copilot Studio, abre el **Panel de prueba** (Test pane) en la esquina inferior derecha.
2. Si el panel no se actualiza, haz clic en **Reiniciar conversación** (Reset conversation) en la parte superior del panel.
3. Escribe el siguiente mensaje de prueba:
   ```
   ¿Cuáles son las solicitudes pendientes?
   ```
4. Observa la respuesta del agente. Debe invocar la acción `ObtenerSolicitudes-Lab04` y retornar información de la lista SharePoint.
5. Verifica en el panel de seguimiento (debajo de la respuesta) que aparece la indicación de que se ejecutó la acción.
6. Ahora escribe:
   ```
   Consulta la tarea número 3 del sistema externo
   ```
7. El agente debe invocar `ConsultarAPI-Lab04` con `taskId = 3` y retornar:
   ```json
   {
     "userId": 1,
     "id": 3,
     "title": "fugiat veniam minus",
     "completed": false
   }
   ```
8. Escribe una tercera prueba para verificar el manejo conversacional:
   ```
   ¿Qué puedes hacer?
   ```
9. El agente debe responder describiendo sus capacidades sin invocar ninguna acción.

**Resultado esperado:**

| Prueba | Acción invocada | Resultado |
|--------|----------------|-----------|
| Solicitudes pendientes | `ObtenerSolicitudes-Lab04` | Lista de ítems de SharePoint |
| Tarea número 3 | `ConsultarAPI-Lab04` | Objeto JSON con id=3 |
| ¿Qué puedes hacer? | Ninguna | Respuesta conversacional |

**Verificación:**
- [ ] La primera prueba retorna datos reales de la lista `SolicitudesLab`.
- [ ] La segunda prueba retorna el objeto JSON de la tarea 3.
- [ ] La tercera prueba no invoca acciones y responde conversacionalmente.
- [ ] Capturas de pantalla de las tres pruebas guardadas en `C:\LabAgentes\capturas\paso6-prueba-[1|2|3].png`.

---

### Paso 7: Publicar el agente

**Objetivo:** Publicar el agente en el entorno de desarrollo para que esté disponible como punto de partida del laboratorio 04-00-02.

**Instrucciones:**

1. En el canvas del agente, haz clic en **Publicar** (Publish) en la esquina superior derecha.
2. En el diálogo de confirmación, haz clic en **Publicar** nuevamente.
3. Espera a que el proceso de publicación complete (indicador de progreso verde).
4. Una vez publicado, verifica el estado en la sección **Canales** (Channels) — debe mostrar al menos el canal de prueba web disponible.

**Resultado esperado:**

Mensaje de confirmación: *"Tu agente ha sido publicado correctamente"* (o equivalente en inglés: *"Your agent has been published successfully"*).

**Verificación:**
- [ ] El agente muestra estado **Publicado** (Published) en la lista de agentes del entorno.
- [ ] Captura de pantalla: `C:\LabAgentes\capturas\paso7-agente-publicado.png`.

---

### Paso 8: Documentar el esquema de acciones

**Objetivo:** Crear un documento de referencia con la configuración de las acciones para reutilización en laboratorios posteriores.

**Instrucciones:**

1. Abre **Visual Studio Code**.
2. Crea un nuevo archivo en la ruta:
   - Windows: `C:\LabAgentes\docs\esquema-acciones-lab04.md`
   - macOS: `~/LabAgentes/docs/esquema-acciones-lab04.md`
3. Escribe el siguiente contenido (completando los valores con tu configuración):

```markdown
# Esquema de Acciones — AgenteEmpresarialLab

## Fecha de creación
[Fecha actual]

## Entorno
- **Nombre:** LabPractice-M2
- **Agente:** AgenteEmpresarialLab
- **Estado:** Publicado

## Acción 1: ObtenerSolicitudes-Lab04

| Propiedad | Valor |
|-----------|-------|
| **Tipo** | Flujo de Power Automate |
| **Trigger** | Copilot Studio |
| **Conector** | SharePoint Online |
| **Operación** | Get Items |
| **Sitio** | https://labagentes[N].sharepoint.com/sites/LabAgentes |
| **Lista** | SolicitudesLab |
| **Parámetros de entrada** | Ninguno |
| **Salida** | `solicitudes` (string — JSON array de ítems) |
| **Descripción para orquestación** | Obtiene la lista de solicitudes empresariales almacenadas en SharePoint |

## Acción 2: ConsultarAPI-Lab04

| Propiedad | Valor |
|-----------|-------|
| **Tipo** | Flujo de Power Automate |
| **Trigger** | Copilot Studio |
| **Conector** | HTTP |
| **Método** | GET |
| **Endpoint** | https://jsonplaceholder.typicode.com/todos/{taskId} |
| **Parámetros de entrada** | `taskId` (texto — ID numérico de la tarea) |
| **Salida** | `apiResponse` (string — JSON del objeto tarea) |
| **Descripción para orquestación** | Consulta tareas desde una API externa |

## Diagrama de flujo conceptual

```
Usuario → Pregunta → Agente (Razonamiento/ReAct)
                         ├── ¿Solicitudes? → ObtenerSolicitudes → SharePoint → Respuesta
                         ├── ¿Tareas? → ConsultarAPI → JSONPlaceholder → Respuesta
                         └── ¿General? → Respuesta conversacional directa
```

## Notas para laboratorios siguientes
- Este agente es el punto de partida para Lab 04-00-02.
- No modificar las acciones existentes; agregar nuevas acciones en labs posteriores.
```

4. Guarda el archivo.
5. Adicionalmente, abre tu workspace de Notion **IA-Agentes-Lab-Workspace** y registra en la bitácora correspondiente:
   - Nombre del agente creado
   - Acciones configuradas
   - Resultado de las pruebas (éxito/fallo)
   - Observaciones relevantes

**Resultado esperado:**

Archivo `esquema-acciones-lab04.md` guardado con la documentación completa de ambas acciones.

**Verificación:**
- [ ] El archivo existe en la ruta correcta y contiene información de ambas acciones.
- [ ] La bitácora en Notion está actualizada.

---

## Validación y Pruebas Finales

Ejecuta la siguiente lista de verificación integral antes de considerar el laboratorio completado:

| # | Criterio | Estado |
|---|----------|--------|
| 1 | El agente `AgenteEmpresarialLab` existe en el entorno `LabPractice-M2` | ☐ |
| 2 | La acción `ObtenerSolicitudes-Lab04` está vinculada y funcional | ☐ |
| 3 | La acción `ConsultarAPI-Lab04` está vinculada y funcional | ☐ |
| 4 | Al preguntar por solicitudes, el agente retorna datos reales de SharePoint | ☐ |
| 5 | Al preguntar por una tarea específica, el agente retorna datos de JSONPlaceholder | ☐ |
| 6 | El agente responde en español según las instrucciones del system prompt | ☐ |
| 7 | La orquestación generativa está habilitada | ☐ |
| 8 | El agente está publicado | ☐ |
| 9 | El archivo de documentación `esquema-acciones-lab04.md` está completo | ☐ |
| 10 | Todas las capturas de pantalla están guardadas en `capturas/` | ☐ |

**Prueba de integración final:**

Escribe en el panel de pruebas la siguiente secuencia conversacional completa:

```
Usuario: Hola
Agente: [Mensaje de bienvenida con capacidades]

Usuario: Muéstrame las solicitudes que están pendientes
Agente: [Invoca ObtenerSolicitudes → muestra datos de SharePoint]

Usuario: Ahora dime qué dice la tarea 5 del sistema externo
Agente: [Invoca ConsultarAPI con taskId=5 → muestra resultado]

Usuario: Gracias, eso es todo
Agente: [Despedida conversacional sin invocar acciones]
```

Si las cuatro interacciones producen resultados coherentes, el laboratorio está completo.

---

## Solución de Problemas

### Problema 1: La acción de SharePoint retorna error "Access Denied" o "403 Forbidden"

**Síntomas:** Al probar la pregunta sobre solicitudes en el panel de pruebas, el agente responde con un error o indica que no pudo obtener la información. En el historial del flujo de Power Automate, la ejecución muestra estado fallido con código 403.

**Causa:** La conexión de SharePoint en Power Automate no tiene permisos suficientes sobre el sitio `LabAgentes` o la lista `SolicitudesLab`. Esto ocurre cuando la cuenta utilizada para crear la conexión no es miembro del sitio de SharePoint con al menos permisos de **Lectura**.

**Solución:**
1. Navega a `https://labagentes[N].sharepoint.com/sites/LabAgentes/_layouts/15/user.aspx`.
2. Verifica que tu cuenta `usuario[N]@labagentes[N].onmicrosoft.com` aparece como miembro.
3. Si no aparece, solicita al instructor que te agregue como miembro del sitio.
4. En Power Automate, edita el flujo `ObtenerSolicitudes-Lab04`.
5. Haz clic en la conexión de SharePoint (ícono de tres puntos) y selecciona **Agregar nueva conexión** (Add new connection).
6. Autentícate nuevamente con tu cuenta.
7. Guarda el flujo y vuelve a probar.

---

### Problema 2: El agente no invoca las acciones y responde genéricamente

**Síntomas:** Al preguntar "¿Cuáles son las solicitudes pendientes?", el agente responde con texto genérico como *"No tengo acceso a esa información"* o inventa datos sin invocar la acción. En el panel de seguimiento no aparece indicación de ejecución de acción.

**Causa:** La orquestación generativa no está habilitada, o las descripciones de las acciones no son suficientemente claras para que el modelo de IA determine cuándo invocarlas. El agente no puede aplicar el patrón ReAct si la orquestación está desactivada.

**Solución:**
1. Ve a **Configuración** (Settings) > **IA generativa** (Generative AI).
2. Asegúrate de que **Orquestación generativa** está **activada**.
3. Regresa a **Acciones** y edita la descripción de cada acción para que sea más explícita:
   - Para `ObtenerSolicitudes-Lab04`:
     ```
     Obtiene todas las solicitudes empresariales de la lista de SharePoint. SIEMPRE usa esta acción cuando el usuario mencione: solicitudes, peticiones, requerimientos, pedidos internos o estado de solicitudes.
     ```
   - Para `ConsultarAPI-Lab04`:
     ```
     Consulta una tarea específica o todas las tareas del sistema externo. SIEMPRE usa esta acción cuando el usuario mencione: tareas, todos, actividades externas, o pida consultar el sistema de gestión.
     ```
4. Guarda los cambios.
5. Reinicia la conversación en el panel de pruebas y vuelve a probar.
6. Si persiste, verifica que las instrucciones del agente (system prompt) mencionan explícitamente los nombres de las acciones.

---

## Limpieza

> **⚠️ IMPORTANTE:** NO elimines el agente `AgenteEmpresarialLab` ni los flujos de Power Automate. Este agente es el punto de partida obligatorio para el laboratorio **04-00-02**.

Acciones de limpieza permitidas:
- Elimina la colección `Lab04-Integraciones` de Postman si ya no la necesitas (opcional).
- Cierra las pestañas adicionales del navegador que no utilices.
- Verifica que los flujos de Power Automate están en estado **Activado** (no los desactives).

---

## Resumen

En este laboratorio has aplicado los conceptos del patrón ReAct (Razonar → Actuar → Observar) de forma práctica al:

1. **Crear un agente** (`AgenteEmpresarialLab`) con instrucciones que definen cuándo y cómo usar herramientas externas.
2. **Integrar un conector de SharePoint** que permite al agente leer datos empresariales reales de la lista `SolicitudesLab`.
3. **Integrar un conector HTTP** que consume una API REST externa (JSONPlaceholder), simulando la conexión con sistemas de terceros.
4. **Configurar la orquestación generativa** para que el agente decida autónomamente cuándo invocar cada herramienta basándose en el contexto conversacional.
5. **Documentar el esquema** de acciones para garantizar la reproducibilidad y continuidad en laboratorios posteriores.

### Conceptos clave reforzados

| Concepto teórico | Aplicación en el laboratorio |
|------------------|------------------------------|
| Herramienta (Tool) | Conector SharePoint, Conector HTTP |
| Acción (Action) | `ObtenerSolicitudes-Lab04`, `ConsultarAPI-Lab04` |
| Ciclo ReAct | Orquestación generativa decidiendo cuándo invocar acciones |
| Esquema estructurado | Descripciones y parámetros de entrada/salida de cada acción |
| Multi-step reasoning | Agente procesando pregunta → invocando herramienta → sintetizando respuesta |

### Recursos adicionales

- [Documentación oficial: Acciones en Microsoft Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/advanced-plugin-actions)
- [Conectores de Power Platform — SharePoint](https://learn.microsoft.com/es-es/connectors/sharepointonline/)
- [API JSONPlaceholder — Documentación](https://jsonplaceholder.typicode.com/guide/)
- [Patrón ReAct — Paper original (Yao et al., 2022)](https://arxiv.org/abs/2210.03629)

---

---

# 5 Laboratorio: Automatización de tareas con agentes

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 50 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Crear |
| **Entorno** | LabPractice-M2 (Sandbox) |
| **Agente base** | AgenteEmpresarialLab (del Lab 04-00-01) |

---

## Descripción General

En este laboratorio crearás un flujo de automatización completo que conecta el agente conversacional `AgenteEmpresarialLab` con Power Automate para registrar solicitudes en SharePoint y enviar confirmaciones por correo electrónico. Aplicarás el patrón ReAct (Razonar → Actuar → Observar) de forma práctica: el agente recopilará datos mediante conversación, invocará un flujo externo como herramienta, y el usuario recibirá confirmación automática. Adicionalmente, configurarás un flujo programado que demuestra la automatización de tareas repetitivas sin intervención humana.

---

## Objetivos de Aprendizaje

- [ ] Crear un flujo Instant cloud flow en Power Automate con trigger `Run a flow from Copilot Studio` que reciba parámetros conversacionales
- [ ] Configurar un tema conversacional en Copilot Studio que recopile datos del usuario y los pase como parámetros al flujo
- [ ] Implementar la creación automática de ítems en SharePoint y el envío de correos de confirmación vía Outlook
- [ ] Validar el flujo end-to-end desde la conversación del agente hasta la recepción del correo electrónico
- [ ] Configurar un flujo programado (Scheduled cloud flow) para automatización de tareas repetitivas

---

## Prerrequisitos

### Conocimientos Previos

| Requisito | Descripción |
|-----------|-------------|
| Lab 04-00-01 completado | Agente `AgenteEmpresarialLab` publicado en `LabPractice-M2` con acciones `ObtenerSolicitudes` y `ConsultarAPI` funcionales |
| Power Automate básico | Comprensión de triggers, acciones y conectores |
| SharePoint Online | Lista `SolicitudesLab` existente con al menos 5 ítems |
| Outlook Web | Acceso verificado para confirmar recepción de correos |

### Accesos Requeridos

| Recurso | URL / Ruta |
|---------|------------|
| Copilot Studio | https://copilotstudio.microsoft.com/environments/LabPractice-M2 |
| Power Automate | https://make.powerautomate.com |
| SharePoint | https://labagentes[N].sharepoint.com/sites/LabSite |
| Outlook Web | https://outlook.office.com |
| Credenciales | usuario[N]@labagentes[N].onmicrosoft.com |

---

## Entorno del Laboratorio

### Software Requerido

| Herramienta | Versión | Propósito |
|-------------|---------|-----------|
| Microsoft Copilot Studio | GA Wave 1 2024 | Configuración del tema y acción del agente |
| Microsoft Power Automate | Cloud - Web App | Creación de flujos Instant y Scheduled |
| SharePoint Online | Microsoft 365 | Lista destino para solicitudes |
| Outlook (Office 365) | Web App | Verificación de correos de confirmación |
| Microsoft Edge / Chrome | 120+ | Navegador principal |

### Verificación Previa del Entorno

Antes de iniciar, confirma los siguientes elementos:

1. Abre https://copilotstudio.microsoft.com y verifica que el entorno `LabPractice-M2` aparece en el selector superior.
2. Localiza el agente `AgenteEmpresarialLab` en la lista de agentes.
3. Abre https://make.powerautomate.com y confirma que el entorno `LabPractice-M2` está seleccionado.
4. Navega a SharePoint > sitio `LabSite` > lista `SolicitudesLab` y confirma que tiene las columnas: `Title`, `Descripcion`, `Prioridad`, `Estado`, `Solicitante`.

> **Nota:** Si la lista `SolicitudesLab` no tiene la columna `Estado`, créala como columna de tipo "Opción" con valores: `Pendiente`, `En Proceso`, `Completada`.

---

## Instrucciones Paso a Paso

### Paso 1: Crear el flujo `RegistrarSolicitudFlow` en Power Automate

**Objetivo:** Construir un flujo Instant cloud flow que reciba datos del agente, cree un registro en SharePoint y envíe un correo de confirmación.

**Instrucciones:**

1. Navega a https://make.powerautomate.com y asegúrate de estar en el entorno `LabPractice-M2`.

2. Haz clic en **+ Crear** (menú lateral izquierdo) > **Flujo de nube instantáneo** (Instant cloud flow).

3. Configura el flujo:
   - **Nombre del flujo:** `RegistrarSolicitudFlow`
   - **Trigger:** Selecciona `Run a flow from Copilot` (anteriormente "Run a flow from Copilot Studio")
   - Haz clic en **Crear**.

4. En el trigger `Run a flow from Copilot`, haz clic en **+ Agregar una entrada** y añade tres parámetros de texto:

   | Nombre del parámetro | Tipo | Descripción |
   |---------------------|------|-------------|
   | `NombreSolicitante` | Texto | Nombre completo de quien realiza la solicitud |
   | `DescripcionSolicitud` | Texto | Descripción detallada de la solicitud |
   | `PrioridadSolicitud` | Texto | Nivel de prioridad: Alta, Media o Baja |

5. Haz clic en **+ Nuevo paso** y busca el conector **SharePoint**. Selecciona la acción **Crear elemento** (Create item).

6. Configura la acción de SharePoint:
   - **Dirección del sitio:** Selecciona `https://labagentes[N].sharepoint.com/sites/LabSite`
   - **Nombre de lista:** `SolicitudesLab`
   - Mapea los campos:
     - **Title** → Contenido dinámico: `NombreSolicitante`
     - **Descripcion** → Contenido dinámico: `DescripcionSolicitud`
     - **Prioridad** → Contenido dinámico: `PrioridadSolicitud`
     - **Estado** → Escribe manualmente: `Pendiente`
     - **Solicitante** → Contenido dinámico: `NombreSolicitante`

7. Haz clic en **+ Nuevo paso** y busca el conector **Office 365 Outlook**. Selecciona la acción **Enviar un correo electrónico (V2)** (Send an email V2).

8. Configura la acción de correo:
   - **Para:** Escribe el correo del instructor (proporcionado en clase, ejemplo: `instructor@labagentes[N].onmicrosoft.com`)
   - **Asunto:** `Confirmación: Nueva solicitud registrada - ` seguido del contenido dinámico `NombreSolicitante`
   - **Cuerpo:** Configura el siguiente contenido:

   ```
   Se ha registrado exitosamente una nueva solicitud:

   Solicitante: [NombreSolicitante]
   Descripción: [DescripcionSolicitud]
   Prioridad: [PrioridadSolicitud]
   Estado: Pendiente
   Fecha de registro: [utcNow()]

   Este correo fue generado automáticamente por AgenteEmpresarialLab.
   ```

   > Reemplaza los valores entre corchetes con el contenido dinámico correspondiente. Para la fecha, usa la expresión `utcNow()` disponible en la pestaña **Expresión**.

9. Haz clic en **+ Nuevo paso** y selecciona **Respond to Copilot** (Responder a Copilot). Añade una salida de tipo texto:
   - **Nombre:** `MensajeConfirmacion`
   - **Valor:** Escribe la expresión:
   ```
   Solicitud registrada exitosamente para [NombreSolicitante]. Se ha enviado un correo de confirmación. ID del registro: [ID del elemento creado en SharePoint]
   ```
   > Usa contenido dinámico `ID` de la acción "Crear elemento" de SharePoint.

10. Haz clic en **Guardar** en la esquina superior derecha.

**Resultado Esperado:**

El flujo debe mostrar la siguiente estructura en el diseñador:

```
Run a flow from Copilot (Trigger)
    ├── Inputs: NombreSolicitante, DescripcionSolicitud, PrioridadSolicitud
    │
├── Create item (SharePoint - SolicitudesLab)
    │
├── Send an email (V2) (Office 365 Outlook)
    │
└── Respond to Copilot
        └── Output: MensajeConfirmacion
```

**Verificación:**

- El flujo aparece con estado "Activado" (sin errores de validación).
- Todos los parámetros de entrada muestran el icono de tipo texto.
- La acción de SharePoint muestra la conexión al sitio correcta.
- La acción de Outlook muestra una conexión activa con las credenciales del participante.

---

### Paso 2: Crear el tema `NuevaSolicitud` en Copilot Studio

**Objetivo:** Diseñar un tema conversacional que guíe al usuario para recopilar los tres parámetros necesarios y luego invoque el flujo creado en el Paso 1.

**Instrucciones:**

1. Navega a https://copilotstudio.microsoft.com/environments/LabPractice-M2.

2. Abre el agente `AgenteEmpresarialLab`.

3. En el panel lateral, selecciona **Temas** (Topics) > **+ Agregar un tema** > **Desde cero** (From blank).

4. Nombra el tema: `NuevaSolicitud`.

5. Configura las **frases desencadenadoras** (Trigger phrases). Añade las siguientes:
   - `Quiero registrar una nueva solicitud`
   - `Necesito crear una solicitud`
   - `Nueva solicitud`
   - `Registrar solicitud`
   - `Tengo una solicitud nueva`
   - `Quiero levantar un ticket`

6. En el nodo de **Mensaje** inicial, escribe:
   ```
   ¡Por supuesto! Voy a ayudarte a registrar una nueva solicitud. Necesito algunos datos para completar el registro.
   ```

7. Añade un nodo **Hacer una pregunta** (Ask a question):
   - **Pregunta:** `¿Cuál es tu nombre completo?`
   - **Identificar:** Selecciona "Respuesta completa del usuario" (User's entire response)
   - **Guardar respuesta como:** Renombra la variable a `VarNombreSolicitante`

8. Añade un segundo nodo **Hacer una pregunta**:
   - **Pregunta:** `¿Podrías describir tu solicitud? Incluye todos los detalles relevantes.`
   - **Identificar:** Selecciona "Respuesta completa del usuario"
   - **Guardar respuesta como:** Renombra la variable a `VarDescripcionSolicitud`

9. Añade un tercer nodo **Hacer una pregunta**:
   - **Pregunta:** `¿Qué prioridad asignarías a esta solicitud?`
   - **Identificar:** Selecciona "Opciones de opción múltiple" (Multiple choice options)
   - **Opciones:** Añade tres opciones: `Alta`, `Media`, `Baja`
   - **Guardar respuesta como:** Renombra la variable a `VarPrioridadSolicitud`

10. Añade un nodo **Mensaje** de confirmación previo a la ejecución:
    ```
    Perfecto. Voy a registrar la siguiente solicitud:
    
    📋 Solicitante: {VarNombreSolicitante}
    📝 Descripción: {VarDescripcionSolicitud}
    ⚡ Prioridad: {VarPrioridadSolicitud}
    
    Procesando tu solicitud...
    ```

    > Para insertar variables, haz clic en el ícono `{x}` y selecciona la variable correspondiente.

11. Añade un nodo **Llamar a una acción** (Call an action):
    - Selecciona **Flujos de Power Automate** en las opciones disponibles.
    - Busca y selecciona `RegistrarSolicitudFlow`.
    - Mapea los parámetros de entrada:
      - `NombreSolicitante` → `VarNombreSolicitante`
      - `DescripcionSolicitud` → `VarDescripcionSolicitud`
      - `PrioridadSolicitud` → `VarPrioridadSolicitud`
    - La salida `MensajeConfirmacion` se almacenará automáticamente en una variable (renómbrala a `VarConfirmacion`).

12. Añade un nodo **Mensaje** final:
    ```
    ✅ {VarConfirmacion}
    
    Se ha enviado un correo de confirmación. ¿Hay algo más en lo que pueda ayudarte?
    ```

13. Añade un nodo **Finalizar con encuesta** (End with survey) o **Finalizar la conversación** según la preferencia del diseño.

14. Haz clic en **Guardar** en la esquina superior derecha del editor de temas.

**Resultado Esperado:**

El flujo del tema en el canvas visual debe mostrar la secuencia:

```
Trigger (frases) → Mensaje bienvenida → Pregunta 1 (nombre) → Pregunta 2 (descripción) → Pregunta 3 (prioridad con opciones) → Mensaje resumen → Llamar acción (RegistrarSolicitudFlow) → Mensaje confirmación → Fin
```

**Verificación:**

- El tema aparece en la lista de temas del agente con estado activo.
- Las tres variables están creadas y mapeadas correctamente al flujo.
- No hay errores de validación (indicadores rojos) en ningún nodo del tema.
- Al pasar el cursor sobre el nodo "Llamar a una acción", se muestra el nombre `RegistrarSolicitudFlow`.

---

### Paso 3: Probar el flujo end-to-end en el panel de pruebas

**Objetivo:** Validar que la conversación completa funciona correctamente, desde la recopilación de datos hasta la creación del registro y el envío del correo.

**Instrucciones:**

1. En Copilot Studio, con el agente `AgenteEmpresarialLab` abierto, haz clic en **Probar** (Test) en la esquina superior derecha para abrir el panel de pruebas.

2. Si aparece un aviso de que hay cambios sin publicar, haz clic en **Probar de todos modos** o publica primero.

3. Escribe en el chat de prueba:
   ```
   Quiero registrar una nueva solicitud
   ```

4. El agente debe responder con el mensaje de bienvenida y preguntar tu nombre. Responde:
   ```
   María García López
   ```

5. El agente preguntará por la descripción. Responde:
   ```
   Solicito acceso al sistema de reportes financieros para el departamento de contabilidad. Necesito permisos de lectura y exportación.
   ```

6. El agente presentará las opciones de prioridad. Selecciona o escribe:
   ```
   Media
   ```

7. El agente debe mostrar el resumen de la solicitud y luego ejecutar el flujo.

8. Espera la respuesta de confirmación del agente que incluye el ID del registro creado.

9. **Verificación en SharePoint:** Abre una nueva pestaña y navega a la lista `SolicitudesLab`. Confirma que existe un nuevo registro con:
   - Title: `María García López`
   - Descripcion: El texto proporcionado
   - Prioridad: `Media`
   - Estado: `Pendiente`

10. **Verificación en Outlook:** Abre https://outlook.office.com y verifica que se recibió el correo de confirmación con el asunto que contiene "María García López".

**Resultado Esperado:**

- La conversación fluye sin interrupciones a través de las tres preguntas.
- El agente muestra un mensaje de confirmación con el ID del registro.
- En SharePoint aparece el nuevo ítem con todos los campos correctamente poblados.
- En Outlook se recibe un correo con el formato definido en el Paso 1.

**Verificación:**

| Punto de verificación | Estado esperado |
|----------------------|-----------------|
| Conversación completa sin errores | ✅ Las tres preguntas se ejecutan en secuencia |
| Flujo ejecutado exitosamente | ✅ Sin mensajes de error en el panel de pruebas |
| Registro en SharePoint | ✅ Nuevo ítem visible con datos correctos |
| Correo en Outlook | ✅ Recibido en menos de 2 minutos |
| Variable de confirmación | ✅ Muestra ID numérico del registro |

---

### Paso 4: Crear el flujo programado `ResumenDiarioFlow`

**Objetivo:** Configurar un flujo Scheduled cloud flow que envíe automáticamente un resumen diario de solicitudes pendientes, demostrando la automatización de tareas repetitivas.

**Instrucciones:**

1. Regresa a https://make.powerautomate.com (entorno `LabPractice-M2`).

2. Haz clic en **+ Crear** > **Flujo de nube programado** (Scheduled cloud flow).

3. Configura el flujo:
   - **Nombre del flujo:** `ResumenDiarioFlow`
   - **Iniciar:** Selecciona la fecha de hoy
   - **Repetir cada:** `1` día
   - **A las:** `08:00` (hora de inicio del día laboral)
   - Haz clic en **Crear**.

4. El trigger `Recurrence` ya estará configurado. Verifica que muestra:
   - Frequency: Day
   - Interval: 1
   - Time zone: (Tu zona horaria local)

5. Añade un **Nuevo paso** > conector **SharePoint** > acción **Obtener elementos** (Get items):
   - **Dirección del sitio:** `https://labagentes[N].sharepoint.com/sites/LabSite`
   - **Nombre de lista:** `SolicitudesLab`
   - **Consulta de filtro (Filter Query):** `Estado eq 'Pendiente'`
   - **Ordenar por:** `Created desc`
   - **Recuento superior:** `50`

6. Añade un **Nuevo paso** > busca **Variables** > selecciona **Inicializar variable**:
   - **Nombre:** `CuerpoResumen`
   - **Tipo:** Cadena (String)
   - **Valor:** 
   ```
   📊 RESUMEN DIARIO DE SOLICITUDES PENDIENTES
   ============================================
   Fecha del reporte: @{utcNow()}
   
   ```

7. Añade un **Nuevo paso** > busca **Control** > selecciona **Aplicar a cada uno** (Apply to each):
   - **Seleccionar una salida del paso anterior:** Contenido dinámico `value` de "Obtener elementos"

8. Dentro del bucle "Aplicar a cada uno", añade la acción **Variables** > **Anexar a variable de cadena** (Append to string variable):
   - **Nombre:** `CuerpoResumen`
   - **Valor:**
   ```
   • Solicitante: @{items('Aplicar_a_cada_uno')?['Title']}
     Descripción: @{items('Aplicar_a_cada_uno')?['Descripcion']}
     Prioridad: @{items('Aplicar_a_cada_uno')?['Prioridad']}
     Fecha creación: @{items('Aplicar_a_cada_uno')?['Created']}
   ---
   ```

   > **Nota:** Selecciona los campos del contenido dinámico del paso "Obtener elementos". Los nombres exactos dependerán de los nombres internos de las columnas de tu lista.

9. Fuera del bucle (después de "Aplicar a cada uno"), añade un **Nuevo paso** > **Office 365 Outlook** > **Enviar un correo electrónico (V2)**:
   - **Para:** Correo del instructor
   - **Asunto:** `[Resumen Diario] Solicitudes Pendientes - @{utcNow()}`
   - **Cuerpo:** Contenido dinámico: variable `CuerpoResumen`
   - **Importancia:** Alta

10. Haz clic en **Guardar**.

11. Para probar inmediatamente sin esperar al día siguiente, haz clic en **Probar** (Test) en la esquina superior derecha:
    - Selecciona **Manualmente** > **Probar**.
    - Espera a que el flujo se ejecute (indicador verde en cada paso).

**Resultado Esperado:**

```
Recurrence (Trigger - Diario 08:00)
    │
├── Get items (SharePoint - SolicitudesLab, filtro: Estado eq 'Pendiente')
    │
├── Initialize variable (CuerpoResumen)
    │
├── Apply to each (por cada solicitud pendiente)
    │   └── Append to string variable (agregar detalle al resumen)
    │
└── Send an email V2 (Outlook - resumen consolidado)
```

**Verificación:**

- El flujo se ejecuta sin errores durante la prueba manual (todos los pasos muestran ✅ verde).
- Se recibe un correo con el resumen que incluye al menos la solicitud creada en el Paso 3.
- El correo muestra correctamente los campos de cada solicitud pendiente.
- El flujo queda programado para ejecución diaria automática.

---

### Paso 5: Publicar el agente actualizado

**Objetivo:** Publicar la versión final del agente con el nuevo tema `NuevaSolicitud` integrado para que esté disponible para el laboratorio 04-00-03.

**Instrucciones:**

1. Regresa a Copilot Studio y abre el agente `AgenteEmpresarialLab`.

2. Verifica que el tema `NuevaSolicitud` aparece en la lista de temas con estado **Activado**.

3. Realiza una prueba rápida adicional en el panel de pruebas con datos diferentes:
   ```
   Necesito crear una solicitud
   ```
   - Nombre: `Carlos Rodríguez`
   - Descripción: `Requiero actualización del software de diseño a la versión más reciente`
   - Prioridad: `Alta`

4. Confirma que la segunda solicitud se registra correctamente en SharePoint y se recibe el correo.

5. Haz clic en **Publicar** (Publish) en la esquina superior derecha.

6. En el diálogo de confirmación, haz clic en **Publicar** nuevamente.

7. Espera a que el estado cambie a "Publicado" (puede tomar 1-2 minutos).

**Resultado Esperado:**

- El agente se publica exitosamente sin errores.
- El estado muestra "Última publicación: [fecha y hora actual]".
- La lista `SolicitudesLab` ahora contiene al menos 7 ítems (5 originales + 2 de prueba).

**Verificación:**

- En la lista de temas, `NuevaSolicitud` muestra un indicador de estado activo.
- El panel de pruebas funciona correctamente después de la publicación.
- Ambos registros de prueba (María García López y Carlos Rodríguez) aparecen en SharePoint con estado `Pendiente`.

---

## Validación y Pruebas

### Prueba Integral Final

Ejecuta la siguiente secuencia de validación para confirmar que todos los componentes funcionan correctamente:

| # | Acción | Resultado esperado | ✅/❌ |
|---|--------|-------------------|-------|
| 1 | Escribir "Registrar solicitud" en el chat del agente | Se activa el tema `NuevaSolicitud` |  |
| 2 | Proporcionar nombre, descripción y prioridad | El agente muestra resumen antes de procesar |  |
| 3 | Verificar SharePoint tras la ejecución | Nuevo ítem con Estado = "Pendiente" |  |
| 4 | Verificar bandeja de Outlook | Correo de confirmación recibido en < 2 min |  |
| 5 | Ejecutar manualmente `ResumenDiarioFlow` | Correo de resumen con todas las solicitudes pendientes |  |
| 6 | Escribir una frase no relacionada al agente | El agente NO activa el tema `NuevaSolicitud` |  |

### Criterios de Éxito

- **Flujo `RegistrarSolicitudFlow`:** Se ejecuta sin errores y completa las dos acciones (SharePoint + Outlook).
- **Tema `NuevaSolicitud`:** Recopila los tres parámetros de forma conversacional y los pasa correctamente al flujo.
- **Flujo `ResumenDiarioFlow`:** Genera un resumen correcto con todas las solicitudes filtradas por estado "Pendiente".
- **Agente publicado:** Versión final disponible para uso en el laboratorio 04-00-03.

---

## Solución de Problemas

### Problema 1: El flujo `RegistrarSolicitudFlow` no aparece en Copilot Studio al intentar añadir la acción

**Síntomas:** Al añadir el nodo "Llamar a una acción" en el tema `NuevaSolicitud`, el flujo `RegistrarSolicitudFlow` no aparece en la lista de flujos disponibles.

**Causa:** El flujo fue creado en un entorno diferente al `LabPractice-M2`, o la solución de Power Automate no se ha compartido correctamente con el agente de Copilot Studio. También puede ocurrir si el trigger del flujo no es `Run a flow from Copilot` sino otro tipo de trigger.

**Solución:**
1. Ve a Power Automate y confirma que el entorno en la esquina superior derecha muestra `LabPractice-M2`.
2. Abre el flujo `RegistrarSolicitudFlow` y verifica que el primer paso (trigger) es exactamente `Run a flow from Copilot`.
3. Si el trigger es incorrecto, elimina el flujo y recréalo seleccionando el trigger correcto.
4. En Copilot Studio, cierra y vuelve a abrir el editor del tema. Haz clic en "Llamar a una acción" > "Flujos de Power Automate" y espera 10-15 segundos para que cargue la lista.
5. Si persiste, haz clic en **Actualizar** (refresh) en la lista de flujos disponibles o intenta buscar por nombre parcial `Registrar`.

---

### Problema 2: El correo de confirmación no se envía aunque el registro en SharePoint se crea correctamente

**Síntomas:** Al probar el flujo, el ítem aparece en la lista de SharePoint pero no se recibe ningún correo en Outlook. En el historial de ejecuciones del flujo, la acción "Send an email V2" muestra estado fallido con error de autorización.

**Causa:** La conexión del conector de Office 365 Outlook ha expirado o no tiene los permisos necesarios. Esto ocurre frecuentemente cuando se usa un tenant de práctica con contraseñas temporales que fueron cambiadas después de crear la conexión inicial.

**Solución:**
1. En Power Automate, ve a **Datos** (Data) > **Conexiones** (Connections) en el menú lateral.
2. Busca la conexión de `Office 365 Outlook` y verifica su estado.
3. Si muestra estado "Error" o "Requiere autenticación", haz clic en los tres puntos (`...`) > **Corregir conexión** (Fix connection).
4. Inicia sesión nuevamente con las credenciales actualizadas `usuario[N]@labagentes[N].onmicrosoft.com`.
5. Regresa al flujo `RegistrarSolicitudFlow`, haz clic en la acción "Send an email V2" y selecciona la conexión reparada.
6. Guarda el flujo y ejecuta una nueva prueba desde el panel de pruebas de Copilot Studio.

---

## Limpieza

Al finalizar el laboratorio, realiza las siguientes acciones de mantenimiento:

1. **NO elimines** el agente `AgenteEmpresarialLab` ni los flujos creados — serán necesarios para el laboratorio 04-00-03.

2. **NO elimines** los registros de prueba en la lista `SolicitudesLab` — servirán como datos de validación en el siguiente laboratorio.

3. **Desactiva temporalmente** el flujo `ResumenDiarioFlow` si no deseas recibir correos diarios hasta el próximo laboratorio:
   - En Power Automate, abre `ResumenDiarioFlow`.
   - Haz clic en **Desactivar** (Turn off) en la barra superior.
   - Podrás reactivarlo cuando sea necesario.

4. **Guarda evidencia** en tu directorio local:
   - Captura de pantalla del tema `NuevaSolicitud` completo → `C:/LabAgentes/capturas/lab04-02-tema.png`
   - Captura del historial de ejecución exitosa del flujo → `C:/LabAgentes/capturas/lab04-02-flujo-exitoso.png`
   - Captura del correo recibido en Outlook → `C:/LabAgentes/capturas/lab04-02-correo-confirmacion.png`

---

## Resumen

En este laboratorio has implementado el patrón completo de **herramientas y acciones** estudiado en la lección 4.1, materializando el ciclo ReAct en un escenario empresarial real:

| Componente creado | Función en el patrón ReAct |
|-------------------|---------------------------|
| Tema `NuevaSolicitud` | **Razonamiento** — El agente guía la conversación y recopila datos |
| Flujo `RegistrarSolicitudFlow` | **Acción** — Herramienta externa que ejecuta operaciones concretas |
| Registro en SharePoint | **Observación** — Resultado tangible de la acción ejecutada |
| Correo de confirmación | **Síntesis** — Comunicación del resultado al usuario |
| Flujo `ResumenDiarioFlow` | **Automatización repetitiva** — Tarea sin intervención humana |

### Conceptos Clave Aplicados

- **Herramientas como extensiones del agente:** El flujo de Power Automate actúa como los "brazos" del agente, ejecutando operaciones que el modelo de lenguaje no puede realizar por sí solo.
- **Parámetros estructurados:** Similar al esquema de `function calling`, el trigger del flujo define parámetros con nombres y tipos que el agente debe proporcionar.
- **Automatización de tareas repetitivas:** El flujo programado demuestra cómo las acciones pueden ejecutarse sin intervención conversacional, complementando la capacidad reactiva del agente.

### Recursos Adicionales

- [Documentación: Llamar a flujos de Power Automate desde Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/advanced-flow-create)
- [Referencia: Conector de SharePoint en Power Automate](https://learn.microsoft.com/es-es/connectors/sharepointonline/)
- [Guía: Flujos programados en Power Automate](https://learn.microsoft.com/es-es/power-automate/run-scheduled-tasks)

---

---

# 7 Laboratorio: Flujo de aprobación con intervención humana

## Metadata

| Campo | Valor |
|-------|-------|
| **Duración** | 50 minutos |
| **Complejidad** | Alta |
| **Nivel Bloom** | Crear |
| **Módulo** | 4 – Uso de Herramientas y Acciones |
| **Entorno** | LabPractice-M2 |

## Descripción General

En este laboratorio implementarás un patrón **human-in-the-loop** completo: diseñarás un flujo de aprobación en Power Automate que intercepta solicitudes de alta prioridad, envía una tarjeta adaptativa de aprobación vía Microsoft Teams y correo electrónico, y actualiza el estado en SharePoint según la decisión del aprobador. Posteriormente, modificarás el agente `AgenteEmpresarialLab` en Copilot Studio para que informe conversacionalmente al usuario sobre el estado de su aprobación. Este patrón materializa el ciclo ReAct estudiado en la lección 4.1, donde la "herramienta" es el flujo de aprobación y la "acción" es la invocación condicional basada en la prioridad.

## Objetivos de Aprendizaje

- [ ] Diseñar e implementar un flujo de aprobación en Power Automate con lógica condicional basada en el campo `PrioridadSolicitud`
- [ ] Configurar el conector de Aprobaciones con envío simultáneo a Teams (canal `AprobacionesLab`) y correo Outlook
- [ ] Implementar la actualización automática del estado en la lista SharePoint según el resultado de la aprobación
- [ ] Modificar el agente en Copilot Studio para comunicar el estado de aprobación (pendiente, aprobada, rechazada) de forma conversacional
- [ ] Validar el flujo completo end-to-end con solicitudes de prioridad Alta y Normal

## Prerrequisitos

### Conocimientos Requeridos

| Concepto | Fuente |
|----------|--------|
| Flujos de Power Automate con triggers y acciones | Laboratorio 04-00-02 |
| Temas y acciones en Copilot Studio | Laboratorios 02-00-01 a 02-00-04 |
| Patrón human-in-the-loop | Material teórico tema 4.6 |
| Ciclo ReAct y herramientas de agentes | Lección 4.1 |
| Lista SharePoint `SolicitudesLab` con datos existentes | Laboratorio 04-00-01 |

### Acceso Requerido

- Credenciales del tenant: `usuario[N]@labagentes[N].onmicrosoft.com`
- Licencia Microsoft 365 E3 + complemento Copilot activa
- Acceso a Power Automate: https://make.powerautomate.com
- Acceso a Copilot Studio: https://copilotstudio.microsoft.com/environments/LabPractice-M2
- Canal de Teams `AprobacionesLab` creado en el equipo del curso
- Al menos 3 registros en la lista SharePoint `SolicitudesLab`
- Flujo `RegistrarSolicitudFlow` activo (Lab 04-00-02)

## Entorno del Laboratorio

### Software

| Herramienta | Versión / Acceso |
|-------------|-----------------|
| Microsoft Power Automate | Web — https://make.powerautomate.com |
| Microsoft Copilot Studio | Web — entorno `LabPractice-M2` |
| Microsoft Teams | Desktop App 24046.2813.2866.2461 o web |
| Microsoft Edge / Chrome | 120+ |
| Visual Studio Code | 1.89.1 (para documentación de prompts) |

### Estructura de Carpetas Local

```
C:/LabAgentes/              (Windows)
~/LabAgentes/               (macOS/Linux)
├── prompts/
├── capturas/
└── docs/
```

### Constantes del Laboratorio

| Variable | Valor |
|----------|-------|
| Correo del aprobador | `lab.approver@labagentes[N].onmicrosoft.com` |
| Nombre del flujo | `RegistrarSolicitudAprobacionFlow` |
| Canal de Teams | `AprobacionesLab` |
| Lista SharePoint | `SolicitudesLab` |
| Umbral de aprobación | Campo `PrioridadSolicitud` = `Alta` |

---

## Paso a Paso

### Paso 1: Duplicar y Renombrar el Flujo Base

**Objetivo:** Crear una versión extendida del flujo existente sin afectar el flujo original funcional.

**Instrucciones:**

1. Abre https://make.powerautomate.com y selecciona el entorno `LabPractice-M2` en el selector de entornos (esquina superior derecha).
2. Navega a **Mis flujos** en el panel izquierdo.
3. Localiza el flujo `RegistrarSolicitudFlow` creado en el laboratorio anterior.
4. Haz clic en los tres puntos (`...`) junto al nombre del flujo y selecciona **Guardar como**.
5. Renombra la copia como `RegistrarSolicitudAprobacionFlow`.
6. Haz clic en **Guardar**.
7. Abre el flujo recién creado y haz clic en **Editar** para entrar al diseñador.

**Resultado Esperado:** Un nuevo flujo `RegistrarSolicitudAprobacionFlow` aparece en la lista con estado **Desactivado** y estructura idéntica al flujo original.

**Verificación:** Confirma que el flujo original `RegistrarSolicitudFlow` permanece en estado **Activado** sin modificaciones.

---

### Paso 2: Añadir la Condición de Prioridad

**Objetivo:** Implementar lógica condicional que bifurque el flujo según el valor del campo `PrioridadSolicitud`.

**Instrucciones:**

1. En el diseñador del flujo `RegistrarSolicitudAprobacionFlow`, localiza la acción que crea el ítem en SharePoint (`Create item` en la lista `SolicitudesLab`).
2. Haz clic en **+ Nuevo paso** inmediatamente después de `Create item`.
3. Busca y selecciona el control **Condición** (*Condition*).
4. Configura la condición de la siguiente manera:
   - **Valor izquierdo:** Selecciona el contenido dinámico `PrioridadSolicitud` (del trigger o de los parámetros de entrada del flujo).
   - **Operador:** `is equal to`
   - **Valor derecho:** `Alta`
5. Observa que se crean automáticamente dos ramas: **Si es verdadero** (*If yes*) y **Si es falso** (*If no*).

**Resultado Esperado:** El diseñador muestra una bifurcación con la condición `PrioridadSolicitud is equal to Alta`.

**Verificación:** Haz clic en la condición y verifica que los tres campos están correctamente configurados. Toma una captura de pantalla y guárdala como `C:/LabAgentes/capturas/lab03-paso02-condicion.png`.

---

### Paso 3: Configurar la Rama de Aprobación (Prioridad Alta)

**Objetivo:** Implementar la solicitud de aprobación formal usando el conector de Aprobaciones de Power Automate.

**Instrucciones:**

1. Dentro de la rama **Si es verdadero** (*If yes*), haz clic en **Agregar una acción**.
2. Busca `Approvals` en el buscador de conectores.
3. Selecciona la acción **Start and wait for an approval**.
4. Configura los campos de la aprobación:

   | Campo | Valor |
   |-------|-------|
   | **Approval type** | `Approve/Reject - First to respond` |
   | **Title** | `Aprobación requerida: Solicitud de prioridad Alta` |
   | **Assigned to** | `lab.approver@labagentes[N].onmicrosoft.com` |
   | **Details** | Usa contenido dinámico para construir el siguiente texto: |

   Para el campo **Details**, escribe el siguiente texto combinando contenido dinámico:

   ```
   Se ha registrado una nueva solicitud que requiere aprobación:
   
   - Solicitante: [NombreSolicitante - contenido dinámico]
   - Descripción: [DescripcionSolicitud - contenido dinámico]
   - Prioridad: Alta
   - Fecha: [utcNow() - expresión]
   
   Por favor, apruebe o rechace esta solicitud.
   ```

5. En el campo **Item link**, ingresa la URL de la lista SharePoint: `https://labagentes[N].sharepoint.com/sites/LabSite/Lists/SolicitudesLab`
6. En **Item link description**, escribe: `Ver lista de solicitudes`

**Resultado Esperado:** La acción `Start and wait for an approval` queda configurada dentro de la rama condicional verdadera con tipo `Approve/Reject - First to respond`.

**Verificación:** Expande la acción y confirma que el campo `Assigned to` contiene el correo del aprobador y que el tipo es `Approve/Reject - First to respond`.

---

### Paso 4: Configurar la Notificación en el Canal de Teams

**Objetivo:** Enviar la tarjeta de aprobación también al canal `AprobacionesLab` de Teams para visibilidad del equipo.

**Instrucciones:**

1. **Antes** de la acción de aprobación (pero dentro de la rama *If yes*), agrega una nueva acción.
2. Busca `Microsoft Teams` y selecciona **Post message in a chat or channel**.
3. Configura:

   | Campo | Valor |
   |-------|-------|
   | **Post as** | `Flow bot` |
   | **Post in** | `Channel` |
   | **Team** | Selecciona el equipo del curso |
   | **Channel** | `AprobacionesLab` |
   | **Message** | (ver abajo) |

4. En el campo **Message**, escribe:

   ```
   ⚠️ **Nueva solicitud de prioridad ALTA pendiente de aprobación**
   
   Solicitante: [NombreSolicitante]
   Descripción: [DescripcionSolicitud]
   
   El aprobador ha sido notificado. Se actualizará el estado una vez resuelta.
   ```

5. Reorganiza las acciones de modo que el mensaje a Teams esté **antes** de `Start and wait for an approval` (arrastra la acción hacia arriba si es necesario).

**Resultado Esperado:** La rama *If yes* contiene primero la notificación a Teams y luego la acción de aprobación que pausa el flujo.

**Verificación:** El orden dentro de la rama debe ser: (1) Post message in Teams → (2) Start and wait for an approval.

---

### Paso 5: Implementar Acciones Post-Aprobación

**Objetivo:** Actualizar el estado en SharePoint y notificar al solicitante según el resultado de la aprobación.

**Instrucciones:**

1. Inmediatamente después de `Start and wait for an approval`, agrega otro control **Condición**.
2. Configura la condición:
   - **Valor izquierdo:** Contenido dinámico → `Outcome` (del paso de aprobación)
   - **Operador:** `is equal to`
   - **Valor derecho:** `Approve`

3. **En la rama "Si es verdadero" (Aprobada):**

   a. Agrega la acción **SharePoint → Update item**:
   
   | Campo | Valor |
   |-------|-------|
   | **Site Address** | `https://labagentes[N].sharepoint.com/sites/LabSite` |
   | **List Name** | `SolicitudesLab` |
   | **Id** | Contenido dinámico: `ID` del paso `Create item` |
   | **Title** | Contenido dinámico: `Title` del paso `Create item` |
   | **Estado** | `Aprobada` |

   b. Agrega la acción **Office 365 Outlook → Send an email (V2)**:
   
   | Campo | Valor |
   |-------|-------|
   | **To** | Contenido dinámico: `CorreoSolicitante` |
   | **Subject** | `✅ Solicitud aprobada` |
   | **Body** | `Su solicitud ha sido aprobada por el equipo de gestión. Puede proceder con la acción solicitada.` |

4. **En la rama "Si es falso" (Rechazada):**

   a. Agrega la acción **SharePoint → Update item**:
   
   | Campo | Valor |
   |-------|-------|
   | **Site Address** | Mismo sitio SharePoint |
   | **List Name** | `SolicitudesLab` |
   | **Id** | Contenido dinámico: `ID` del paso `Create item` |
   | **Title** | Contenido dinámico: `Title` del paso `Create item` |
   | **Estado** | `Rechazada` |

   b. Agrega la acción **Office 365 Outlook → Send an email (V2)**:
   
   | Campo | Valor |
   |-------|-------|
   | **To** | Contenido dinámico: `CorreoSolicitante` |
   | **Subject** | `❌ Solicitud rechazada` |
   | **Body** | `Su solicitud de prioridad alta ha sido rechazada. Contacte a su supervisor para más información. Comentarios del aprobador: [Responses Comments - contenido dinámico]` |

**Resultado Esperado:** La rama de aprobación contiene una condición anidada que actualiza SharePoint y envía correo diferenciado según `Approve` o `Reject`.

**Verificación:** Expande ambas sub-ramas y confirma que cada una tiene exactamente 2 acciones: `Update item` + `Send an email`.

---

### Paso 6: Configurar la Rama Sin Aprobación (Prioridad Normal/Baja)

**Objetivo:** Las solicitudes de prioridad Normal o Baja se registran directamente sin intervención humana.

**Instrucciones:**

1. En la rama **Si es falso** (*If no*) de la condición principal (Paso 2), agrega la acción **SharePoint → Update item**.
2. Configura:

   | Campo | Valor |
   |-------|-------|
   | **Site Address** | `https://labagentes[N].sharepoint.com/sites/LabSite` |
   | **List Name** | `SolicitudesLab` |
   | **Id** | Contenido dinámico: `ID` del paso `Create item` |
   | **Title** | Contenido dinámico: `Title` del paso `Create item` |
   | **Estado** | `Registrada` |

3. Agrega la acción **Office 365 Outlook → Send an email (V2)**:

   | Campo | Valor |
   |-------|-------|
   | **To** | Contenido dinámico: `CorreoSolicitante` |
   | **Subject** | `📋 Solicitud registrada exitosamente` |
   | **Body** | `Su solicitud ha sido registrada correctamente. No requiere aprobación adicional.` |

4. Haz clic en **Guardar** en la esquina superior derecha del diseñador.

**Resultado Esperado:** La rama *If no* contiene 2 acciones simples (actualizar estado a "Registrada" y enviar confirmación por correo).

**Verificación:** Guarda el flujo sin errores. El diseñador no debe mostrar advertencias en rojo.

---

### Paso 7: Activar y Probar el Flujo de Aprobación

**Objetivo:** Validar que el flujo se ejecuta correctamente con una solicitud de prioridad Alta.

**Instrucciones:**

1. Haz clic en **Activar** (*Turn on*) en la barra superior del flujo.
2. Haz clic en **Probar** (*Test*) → selecciona **Manualmente** → **Probar**.
3. Si el flujo tiene un trigger HTTP o se invoca desde Copilot Studio, abre Copilot Studio y ejecuta el tema `NuevaSolicitud` con los siguientes datos de prueba:

   | Campo | Valor de prueba |
   |-------|----------------|
   | NombreSolicitante | `Participante Lab` |
   | DescripcionSolicitud | `Solicitud de acceso a sistema financiero` |
   | PrioridadSolicitud | `Alta` |
   | CorreoSolicitante | Tu correo del tenant |

4. Observa en Power Automate que el flujo se ejecuta y **pausa** en el paso `Start and wait for an approval`.
5. Abre **Microsoft Teams** y navega al canal `AprobacionesLab`. Confirma que aparece el mensaje de notificación.
6. Abre el correo del aprobador (`lab.approver@labagentes[N].onmicrosoft.com`) — si eres el instructor, aprueba la solicitud desde la tarjeta en Teams o desde el correo.
7. Si no tienes acceso al correo del aprobador, solicita al instructor que apruebe la solicitud durante la clase.

**Resultado Esperado:**
- El flujo pausa en el paso de aprobación (estado: `Waiting`).
- El canal de Teams muestra la notificación.
- Tras la aprobación, el flujo continúa y actualiza el estado a `Aprobada` en SharePoint.
- El solicitante recibe un correo de confirmación.

**Verificación:** 
- En Power Automate → historial de ejecuciones: el flujo muestra estado `Succeeded` (verde).
- En la lista SharePoint `SolicitudesLab`: el registro muestra `Estado = Aprobada`.
- Toma captura: `C:/LabAgentes/capturas/lab03-paso07-flujo-exitoso.png`.

---

### Paso 8: Probar con Prioridad Normal (Sin Aprobación)

**Objetivo:** Verificar que las solicitudes de prioridad Normal se procesan sin intervención humana.

**Instrucciones:**

1. Ejecuta nuevamente el tema `NuevaSolicitud` en Copilot Studio con:

   | Campo | Valor de prueba |
   |-------|----------------|
   | NombreSolicitante | `Participante Lab` |
   | DescripcionSolicitud | `Solicitud de papelería de oficina` |
   | PrioridadSolicitud | `Normal` |
   | CorreoSolicitante | Tu correo del tenant |

2. Observa en Power Automate que el flujo se ejecuta completamente **sin pausar**.
3. Verifica en SharePoint que el nuevo registro tiene `Estado = Registrada`.
4. Confirma la recepción del correo de confirmación sin mención de aprobación.

**Resultado Esperado:** El flujo completa en menos de 30 segundos sin intervención humana. El estado es `Registrada`.

**Verificación:** Historial de ejecución muestra todas las acciones en verde, sin paso de aprobación ejecutado.

---

### Paso 9: Crear el Tema "ConsultarEstadoAprobacion" en Copilot Studio

**Objetivo:** Permitir al usuario consultar conversacionalmente el estado de su solicitud pendiente.

**Instrucciones:**

1. Abre https://copilotstudio.microsoft.com/environments/LabPractice-M2.
2. Selecciona el agente `AgenteEmpresarialLab` (o tu agente con la convención `Agente-[TipoCasoUso]-[Inicial]`).
3. Navega a **Temas** → **+ Nuevo tema** → **Desde cero**.
4. Nombra el tema: `ConsultarEstadoAprobacion`.
5. Configura las **frases de activación** (*Trigger phrases*):
   - `¿Cuál es el estado de mi solicitud?`
   - `Consultar estado de aprobación`
   - `¿Mi solicitud fue aprobada?`
   - `Estado de mi solicitud pendiente`
   - `¿Qué pasó con mi solicitud de prioridad alta?`

6. Agrega un nodo **Pregunta** (*Question*):
   - **Mensaje:** `Para consultar el estado de su solicitud, por favor proporcione su nombre o correo electrónico registrado.`
   - **Identificar:** `Email address`
   - **Variable:** `CorreoBusqueda`

7. Agrega un nodo **Acción** que invoque la acción `ObtenerSolicitudes` (creada en el Lab 04-00-01):
   - **Entrada:** Pasa la variable `CorreoBusqueda` como filtro.

8. Agrega un nodo **Condición** que evalúe el campo `Estado` del resultado:
   - **Si** `Estado` es igual a `Aprobada` → Nodo Mensaje: `✅ ¡Buenas noticias! Su solicitud ha sido **aprobada**. Puede proceder con la acción solicitada.`
   - **Si** `Estado` es igual a `Rechazada` → Nodo Mensaje: `❌ Lamentablemente, su solicitud fue **rechazada**. Le recomiendo contactar a su supervisor para más detalles.`
   - **En otro caso** → Nodo Mensaje: `⏳ Su solicitud se encuentra actualmente **pendiente de aprobación**. Le notificaremos por correo una vez que el aprobador tome una decisión.`

9. Finaliza cada rama con un nodo **Fin de conversación** o **Redirigir** al tema de despedida.
10. Haz clic en **Guardar**.

**Resultado Esperado:** El tema `ConsultarEstadoAprobacion` aparece en la lista de temas con 5 frases de activación y lógica condicional de 3 ramas.

**Verificación:** En el panel de prueba (Test bot), escribe `¿Cuál es el estado de mi solicitud?` y confirma que el agente solicita el correo electrónico.

---

### Paso 10: Crear el Tema "SolicitudRechazada" con Mensaje de Seguimiento

**Objetivo:** Proporcionar al usuario una ruta de acción clara cuando su solicitud es rechazada.

**Instrucciones:**

1. En Copilot Studio, crea un nuevo tema: `SolicitudRechazada`.
2. Configura las frases de activación:
   - `Mi solicitud fue rechazada`
   - `¿Qué hago si mi solicitud fue rechazada?`
   - `Solicitud denegada, ¿qué opciones tengo?`

3. Agrega un nodo **Mensaje** con el siguiente contenido:

   ```
   Lamento que su solicitud haya sido rechazada. Aquí tiene algunas opciones:
   
   1️⃣ **Contactar a su supervisor** para entender el motivo del rechazo.
   2️⃣ **Reformular la solicitud** con información adicional que justifique la prioridad.
   3️⃣ **Escalar al área correspondiente** si considera que hubo un error.
   
   ¿Desea que le ayude con alguna de estas opciones?
   ```

4. Agrega un nodo **Pregunta** con opciones múltiples:
   - Opción 1: `Contactar supervisor`
   - Opción 2: `Reformular solicitud`
   - Opción 3: `No, gracias`

5. Para la opción `Reformular solicitud`, redirige al tema `NuevaSolicitud` existente.
6. Para las demás opciones, muestra un mensaje de cierre apropiado.
7. Haz clic en **Guardar**.

**Resultado Esperado:** El tema `SolicitudRechazada` ofrece 3 opciones de seguimiento al usuario, con redirección funcional al tema de nueva solicitud.

**Verificación:** Prueba en el panel de test escribiendo `Mi solicitud fue rechazada` y confirma que aparecen las 3 opciones.

---

### Paso 11: Publicar y Probar el Agente Completo

**Objetivo:** Validar la integración end-to-end entre el agente y el flujo de aprobación.

**Instrucciones:**

1. En Copilot Studio, haz clic en **Publicar** → **Publicar** para desplegar los cambios.
2. Abre el panel de **Prueba** (Test your copilot).
3. Ejecuta la siguiente secuencia conversacional:

   **Conversación de prueba:**
   ```
   Usuario: Quiero registrar una nueva solicitud
   [El agente activa el tema NuevaSolicitud]
   
   Usuario: [Proporciona datos con prioridad Alta]
   [El flujo se activa y pausa en aprobación]
   
   Usuario: ¿Cuál es el estado de mi solicitud?
   [El agente activa ConsultarEstadoAprobacion]
   
   Usuario: [Proporciona su correo]
   [El agente responde con estado "Pendiente"]
   ```

4. Solicita al instructor que apruebe o rechace la solicitud.
5. Repite la consulta de estado para verificar que el agente ahora muestra `Aprobada` o `Rechazada`.

**Resultado Esperado:** El agente comunica correctamente los tres estados posibles (pendiente, aprobada, rechazada) de forma conversacional natural.

**Verificación:** Toma capturas de pantalla de cada interacción y guárdalas en `C:/LabAgentes/capturas/`.

---

## Validación y Pruebas

### Matriz de Validación Completa

| # | Escenario de Prueba | Entrada | Resultado Esperado | ✓ |
|---|---------------------|---------|-------------------|---|
| 1 | Solicitud prioridad Alta | `PrioridadSolicitud = Alta` | Flujo pausa, aprobación enviada a Teams y correo | ☐ |
| 2 | Solicitud prioridad Normal | `PrioridadSolicitud = Normal` | Flujo completa sin pausa, estado = Registrada | ☐ |
| 3 | Aprobación aceptada | Aprobador selecciona `Approve` | Estado actualizado a `Aprobada`, correo enviado | ☐ |
| 4 | Aprobación rechazada | Aprobador selecciona `Reject` | Estado actualizado a `Rechazada`, correo con comentarios | ☐ |
| 5 | Consulta estado pendiente | Usuario pregunta antes de decisión | Agente responde "pendiente de aprobación" | ☐ |
| 6 | Consulta estado aprobada | Usuario pregunta después de aprobación | Agente responde "aprobada" | ☐ |
| 7 | Tema solicitud rechazada | Usuario dice "mi solicitud fue rechazada" | Agente ofrece 3 opciones de seguimiento | ☐ |
| 8 | Notificación en canal Teams | Solicitud de prioridad Alta | Mensaje visible en canal `AprobacionesLab` | ☐ |

### Criterios de Éxito

- **Mínimo 6 de 8** escenarios deben pasar para considerar el laboratorio completado.
- El flujo `RegistrarSolicitudAprobacionFlow` debe mostrar al menos 2 ejecuciones exitosas en el historial.
- Los dos temas nuevos (`ConsultarEstadoAprobacion` y `SolicitudRechazada`) deben estar publicados y funcionales.

---

## Solución de Problemas

### Problema 1: La acción de aprobación falla con error "No Approvals connection found"

**Síntomas:** Al ejecutar el flujo, el paso `Start and wait for an approval` muestra error rojo con mensaje `The connection 'shared_approvals' is not configured` o `No Approvals connection found`.

**Causa:** El conector de Aprobaciones no ha sido autorizado en el entorno. Power Automate requiere que se establezca una conexión al servicio de Aprobaciones la primera vez que se utiliza en un entorno nuevo.

**Solución:**
1. En Power Automate, navega a **Datos** → **Conexiones** en el panel izquierdo.
2. Haz clic en **+ Nueva conexión**.
3. Busca `Approvals` y selecciónalo.
4. Haz clic en **Crear** y autentica con tus credenciales del tenant.
5. Regresa al flujo, haz clic en el paso de aprobación y en el ícono de conexión (`...` → **Fix connection**), selecciona la conexión recién creada.
6. Guarda y vuelve a ejecutar la prueba.

---

### Problema 2: El agente no encuentra la solicitud al consultar el estado

**Síntomas:** Al ejecutar el tema `ConsultarEstadoAprobacion`, el agente responde que no encontró solicitudes o muestra un error de la acción `ObtenerSolicitudes`.

**Causa:** El filtro de la acción en Copilot Studio no está mapeando correctamente la variable `CorreoBusqueda` al parámetro de filtro de la acción de Power Automate, o el campo `CorreoSolicitante` en SharePoint tiene un formato diferente al ingresado por el usuario.

**Solución:**
1. En Copilot Studio, abre el tema `ConsultarEstadoAprobacion` y verifica el nodo de Acción.
2. Confirma que la variable `CorreoBusqueda` está mapeada al parámetro de entrada correcto de la acción.
3. En Power Automate, verifica que el flujo `ObtenerSolicitudes` usa un filtro OData compatible: `CorreoSolicitante eq '[variable]'`.
4. Asegúrate de que el campo en SharePoint se llama exactamente `CorreoSolicitante` (sensible a mayúsculas en el nombre interno).
5. Prueba manualmente en SharePoint que el correo usado existe como valor en la columna.
6. Si el problema persiste, cambia el identificador de búsqueda de correo a nombre del solicitante como alternativa temporal.

---

## Limpieza

1. **No desactives** el flujo `RegistrarSolicitudAprobacionFlow` — será reutilizado en el laboratorio 04-00-04.
2. **No elimines** los temas `ConsultarEstadoAprobacion` ni `SolicitudRechazada` del agente.
3. Limpia solicitudes de prueba duplicadas en la lista SharePoint si tienes más de 10 registros de prueba (conserva al menos 5 para el siguiente laboratorio).
4. Cierra las pestañas de Power Automate que no uses para liberar memoria del navegador.
5. Guarda todas las capturas de pantalla en `C:/LabAgentes/capturas/` con la nomenclatura `lab03-paso[N]-[descripcion].png`.

---

## Resumen

En este laboratorio implementaste un patrón **human-in-the-loop** completo que demuestra cómo las herramientas y acciones (concepto central de la lección 4.1) se materializan en un entorno empresarial real:

- **Flujo de aprobación condicional:** Lógica que bifurca el procesamiento según la prioridad, aplicando el ciclo ReAct donde el "razonamiento" es la condición y la "acción" es la solicitud de aprobación.
- **Intervención humana controlada:** El conector de Aprobaciones pausa el flujo automatizado para permitir juicio humano en decisiones de alto impacto.
- **Comunicación multicanal:** Notificaciones simultáneas en Teams y correo electrónico para maximizar la visibilidad.
- **Agente conversacional informado:** El agente consulta el estado actualizado y comunica resultados de forma natural, cerrando el ciclo de interacción usuario-sistema-aprobador.

### Componentes Producidos para el Siguiente Laboratorio

| Componente | Uso en Lab 04-00-04 |
|------------|-------------------|
| `RegistrarSolicitudAprobacionFlow` | Base para orquestación avanzada |
| Tema `ConsultarEstadoAprobacion` | Integración con flujos de seguimiento |
| Lista SharePoint con campo `Estado` | Fuente de datos para reportes automatizados |

### Recursos Adicionales

- [Documentación de Aprobaciones en Power Automate](https://learn.microsoft.com/es-es/power-automate/get-started-approvals)
- [Acciones personalizadas en Copilot Studio](https://learn.microsoft.com/es-es/microsoft-copilot-studio/advanced-plugin-actions)
- [Patrón ReAct: Synergizing Reasoning and Acting](https://arxiv.org/abs/2210.03629)
- [Conector SharePoint - Update Item](https://learn.microsoft.com/es-es/connectors/sharepointonline/)

---

---

# 9 Laboratorio: Integración de agente en un flujo de procesos empresariales

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 50 minutos |
| **Complejidad** | Alta |
| **Nivel Bloom** | Crear |
| **Módulo** | 4 — Integración de agentes con procesos empresariales |
| **Entorno** | LabPractice-M2 |

## Descripción General

Este laboratorio es el proyecto integrador del Módulo 4. Construirás un flujo maestro orquestador en Power Automate llamado `ProcesoEmpresarialMaestroFlow` que coordina el ciclo de vida completo de una solicitud empresarial: inicio desde el agente, registro en SharePoint, aprobación humana, integración con un sistema ERP simulado mediante conector personalizado, actualización de estado y notificación en Teams. Además, crearás un conector personalizado (`ERPSimuladoConnector`) usando una definición OpenAPI 3.0 editada en Visual Studio Code, añadirás un tema de consulta de historial al agente, ejecutarás pruebas de aceptación con tres escenarios distintos y exportarás la solución completa como archivo `.zip`.

## Objetivos de Aprendizaje

- [ ] Orquestar un proceso empresarial end-to-end que integre el agente con múltiples aplicaciones de Microsoft 365 en un flujo maestro de Power Automate
- [ ] Implementar un conector personalizado basado en OpenAPI 3.0 que conecte el agente con un endpoint REST externo simulando un ERP
- [ ] Configurar el agente como punto de entrada único coordinando registro, aprobación, ejecución y cierre de solicitudes
- [ ] Validar el proceso completo mediante pruebas de aceptación con tres escenarios diferenciados
- [ ] Documentar y exportar la arquitectura completa como solución de Power Platform no administrada

## Prerrequisitos

### Conocimientos Previos

| Requisito | Detalle |
|-----------|---------|
| Laboratorio 04-00-03 completado | Flujo `RegistrarSolicitudAprobacionFlow` activo y temas de consulta funcionales |
| Conectores personalizados | Comprensión de OpenAPI 3.0 (introducida en Lab 01) |
| Power Automate | Experiencia creando flujos con condiciones, acciones HTTP y conectores |
| Copilot Studio | Creación de temas y publicación de agentes |
| API REST | Familiaridad con endpoints JSON y métodos HTTP POST/GET |

### Acceso Requerido

| Recurso | Credenciales/URL |
|---------|-----------------|
| Microsoft 365 Tenant | `usuario[N]@labagentes[N].onmicrosoft.com` |
| Copilot Studio | https://copilotstudio.microsoft.com/environments/LabPractice-M2 |
| Power Automate | https://make.powerautomate.com |
| SharePoint | Lista `SolicitudesLab` con al menos 5 registros previos |
| JSONPlaceholder API | https://jsonplaceholder.typicode.com/posts |
| Visual Studio Code | Versión 1.85.1+ con extensión Power Platform Tools 2.0.19 |

## Entorno del Laboratorio

### Software Necesario

| Herramienta | Versión | Propósito |
|-------------|---------|-----------|
| Microsoft Edge / Chrome | 120+ | Acceso a plataformas web |
| Visual Studio Code | 1.85.1 | Edición de definición OpenAPI |
| Power Platform Tools (extensión VS Code) | 2.0.19 | Soporte Power Platform |
| Postman | 10.22.0 | Verificación de endpoints |
| Microsoft Teams | Desktop 24046+ | Verificación de notificaciones |

### Preparación del Directorio de Trabajo

Verificar que la estructura de carpetas existe antes de comenzar:

```powershell
# Windows - PowerShell
Test-Path "C:\LabAgentes\prompts", "C:\LabAgentes\capturas", "C:\LabAgentes\docs"

# Crear carpeta específica para entregables del Lab 04
New-Item -ItemType Directory -Path "C:\LabAgentes\Lab04" -Force
```

```bash
# macOS/Linux
mkdir -p ~/LabAgentes/Lab04
```

---

## Paso 1: Crear la Definición OpenAPI del Conector Personalizado

### Objetivo
Diseñar y escribir la especificación OpenAPI 3.0 que define el conector `ERPSimuladoConnector`, el cual simula la creación de órdenes en un sistema ERP externo usando el endpoint público JSONPlaceholder.

### Instrucciones

1. Abrir Visual Studio Code 1.85.1.

2. Crear un nuevo archivo en la ruta `C:\LabAgentes\Lab04\erp-simulado-openapi.json` (Windows) o `~/LabAgentes/Lab04/erp-simulado-openapi.json` (macOS).

3. Escribir la siguiente definición OpenAPI 3.0 completa:

```json
{
  "openapi": "3.0.1",
  "info": {
    "title": "ERP Simulado Connector",
    "description": "Conector personalizado que simula la integración con un sistema ERP externo para crear órdenes de compra asociadas a solicitudes empresariales aprobadas.",
    "version": "1.0.0",
    "contact": {
      "name": "Equipo Lab Agentes"
    }
  },
  "servers": [
    {
      "url": "https://jsonplaceholder.typicode.com"
    }
  ],
  "paths": {
    "/posts": {
      "post": {
        "operationId": "CrearOrdenERP",
        "summary": "Crear orden en ERP simulado",
        "description": "Registra una nueva orden en el sistema ERP externo simulado. Retorna el ID de orden generado.",
        "requestBody": {
          "required": true,
          "content": {
            "application/json": {
              "schema": {
                "type": "object",
                "required": ["title", "body", "userId"],
                "properties": {
                  "title": {
                    "type": "string",
                    "description": "Título de la solicitud aprobada"
                  },
                  "body": {
                    "type": "string",
                    "description": "Descripción completa de la solicitud incluyendo prioridad y solicitante"
                  },
                  "userId": {
                    "type": "integer",
                    "description": "ID numérico del solicitante en el sistema"
                  }
                }
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Orden creada exitosamente",
            "content": {
              "application/json": {
                "schema": {
                  "type": "object",
                  "properties": {
                    "id": {
                      "type": "integer",
                      "description": "ID único de la orden generada en el ERP"
                    },
                    "title": {
                      "type": "string"
                    },
                    "body": {
                      "type": "string"
                    },
                    "userId": {
                      "type": "integer"
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  },
  "security": [],
  "tags": []
}
```

4. Guardar el archivo (Ctrl+S).

5. Verificar la validez del JSON usando la terminal integrada de VS Code:

```powershell
# Windows PowerShell
Get-Content "C:\LabAgentes\Lab04\erp-simulado-openapi.json" | ConvertFrom-Json | ConvertTo-Json -Depth 10 | Out-Null
Write-Host "JSON válido" -ForegroundColor Green
```

```bash
# macOS/Linux
python3 -c "import json; json.load(open('$HOME/LabAgentes/Lab04/erp-simulado-openapi.json')); print('JSON válido')"
```

6. **(Opcional)** Probar el endpoint en Postman: enviar un POST a `https://jsonplaceholder.typicode.com/posts` con body JSON:

```json
{
  "title": "Solicitud-TEST-001",
  "body": "Prueba de integración ERP - Prioridad Alta - Solicitante: Lab User",
  "userId": 1
}
```

### Resultado Esperado

- Archivo `erp-simulado-openapi.json` guardado sin errores de sintaxis.
- El endpoint responde con código HTTP 201 y un objeto JSON con `id: 101`.

### Verificación

- El archivo tiene exactamente las propiedades `openapi`, `info`, `servers`, `paths` en el nivel raíz.
- La operación `CrearOrdenERP` está definida bajo `POST /posts`.
- Postman retorna `201 Created` con campo `id` en la respuesta.

---

## Paso 2: Registrar el Conector Personalizado en Power Platform

### Objetivo
Crear el conector personalizado `ERPSimuladoConnector` en el entorno LabPractice-M2 importando la definición OpenAPI creada en el paso anterior.

### Instrucciones

1. Navegar a https://make.powerautomate.com.

2. En la esquina superior derecha, verificar que el entorno seleccionado sea **LabPractice-M2**. Si no lo es, hacer clic en el selector de entorno y cambiarlo.

3. En el menú lateral izquierdo, seleccionar **Más** → **Descubrir todo** → **Conectores personalizados** (bajo la sección "Datos").

4. Hacer clic en **+ Nuevo conector personalizado** → **Importar un archivo OpenAPI**.

5. En el diálogo:
   - **Nombre del conector**: `ERPSimuladoConnector`
   - **Archivo**: seleccionar `C:\LabAgentes\Lab04\erp-simulado-openapi.json`
   - Hacer clic en **Importar** → **Continuar**.

6. En la pestaña **1. General**:
   - Verificar que el host muestra `jsonplaceholder.typicode.com`
   - Esquema: `HTTPS`
   - Hacer clic en **Siguiente** (flecha derecha o pestaña "Seguridad").

7. En la pestaña **2. Seguridad**:
   - Tipo de autenticación: seleccionar **Sin autenticación** (el endpoint es público).
   - Hacer clic en **Siguiente**.

8. En la pestaña **3. Definición**:
   - Verificar que aparece la acción `CrearOrdenERP` con método POST.
   - Verificar que los parámetros `title`, `body` y `userId` están listados en el request body.
   - Hacer clic en **Siguiente**.

9. En la pestaña **4. Código** (si aparece): dejar en blanco, hacer clic en **Siguiente**.

10. En la pestaña **5. Prueba**:
    - Hacer clic en **Crear conector** (botón superior derecho con marca de verificación ✓).
    - Esperar confirmación: "El conector se ha creado correctamente".

11. Crear una conexión de prueba:
    - En la misma pestaña de prueba, hacer clic en **+ Nueva conexión**.
    - Hacer clic en **Crear** (no requiere credenciales).
    - Seleccionar la conexión recién creada.

12. Probar la operación `CrearOrdenERP`:
    - En el campo `title`: `Test-PowerPlatform-001`
    - En el campo `body`: `Validación de conector desde Power Platform`
    - En el campo `userId`: `1`
    - Hacer clic en **Probar operación**.

### Resultado Esperado

- Estado de respuesta: **201**
- Cuerpo de respuesta incluye `"id": 101`
- Mensaje: "La solicitud se ha realizado correctamente"

### Verificación

- El conector `ERPSimuladoConnector` aparece en la lista de conectores personalizados del entorno LabPractice-M2.
- La prueba integrada retorna código 201 con un ID válido.
- Tomar captura de pantalla y guardar en `C:\LabAgentes\capturas\lab04-conector-test.png`.

---

## Paso 3: Construir el Flujo Maestro Orquestador

### Objetivo
Crear el flujo `ProcesoEmpresarialMaestroFlow` en Power Automate que orquesta las cinco etapas del ciclo de vida de una solicitud: registro, aprobación, ejecución ERP, actualización de estado y notificación.

### Instrucciones

1. En Power Automate (https://make.powerautomate.com), seleccionar entorno **LabPractice-M2**.

2. Hacer clic en **+ Crear** → **Flujo de nube automatizado**.

3. Configurar:
   - **Nombre del flujo**: `ProcesoEmpresarialMaestroFlow`
   - **Desencadenador**: seleccionar **Cuando se crea un elemento** (SharePoint).
   - Hacer clic en **Crear**.

4. Configurar el desencadenador **Cuando se crea un elemento**:
   - **Dirección del sitio**: seleccionar el sitio SharePoint del laboratorio.
   - **Nombre de la lista**: `SolicitudesLab`

5. **Acción 1 — Iniciar y esperar una aprobación:**
   - Hacer clic en **+ Nuevo paso** → buscar "Aprobaciones" → seleccionar **Iniciar y esperar una aprobación**.
   - Tipo de aprobación: **Aprobar o rechazar - Primer respondedor**
   - Título: `Aprobación de solicitud: @{triggerOutputs()?['body/Title']}`
   - Asignado a: ingresar el correo del aprobador (usar el propio correo del participante para pruebas).
   - Detalles:
   ```
   Solicitud: @{triggerOutputs()?['body/Title']}
   Prioridad: @{triggerOutputs()?['body/Prioridad']}
   Solicitante: @{triggerOutputs()?['body/Solicitante']}
   Descripción: @{triggerOutputs()?['body/Descripcion']}
   ```

6. **Acción 2 — Condición de aprobación:**
   - Hacer clic en **+ Nuevo paso** → seleccionar **Condición**.
   - Configurar: `Resultado` (del paso de aprobación) **es igual a** `Approve`

7. **Rama "Sí" (Aprobada):**

   7a. Agregar acción: buscar `ERPSimuladoConnector` → seleccionar **CrearOrdenERP**.
   - `title`: `@{triggerOutputs()?['body/Title']}`
   - `body`: `Solicitud aprobada | Prioridad: @{triggerOutputs()?['body/Prioridad']} | Solicitante: @{triggerOutputs()?['body/Solicitante']} | Fecha: @{utcNow()}`
   - `userId`: `1`

   7b. Agregar acción: **Actualizar elemento** (SharePoint).
   - **Dirección del sitio**: mismo sitio SharePoint.
   - **Nombre de la lista**: `SolicitudesLab`
   - **Id**: `@{triggerOutputs()?['body/ID']}`
   - **Title**: `@{triggerOutputs()?['body/Title']}`
   - **Estado**: `Completada`
   - **OrdenERP**: `@{outputs('CrearOrdenERP')?['body/id']}`

   > **Nota:** Si la columna `OrdenERP` no existe en la lista SharePoint, crearla como columna de tipo "Número" antes de continuar.

   7c. Agregar acción: **Publicar mensaje en un chat o canal** (Microsoft Teams).
   - **Publicar como**: Flow bot
   - **Publicar en**: Canal
   - **Equipo**: seleccionar el equipo del laboratorio
   - **Canal**: `General`
   - **Mensaje**:
   ```
   ✅ Solicitud COMPLETADA
   📋 Título: @{triggerOutputs()?['body/Title']}
   👤 Solicitante: @{triggerOutputs()?['body/Solicitante']}
   🏷️ Prioridad: @{triggerOutputs()?['body/Prioridad']}
   🔗 Orden ERP: #@{outputs('CrearOrdenERP')?['body/id']}
   📅 Fecha cierre: @{utcNow()}
   ```

8. **Rama "No" (Rechazada):**

   8a. Agregar acción: **Actualizar elemento** (SharePoint).
   - **Id**: `@{triggerOutputs()?['body/ID']}`
   - **Title**: `@{triggerOutputs()?['body/Title']}`
   - **Estado**: `Rechazada`

   8b. Agregar acción: **Publicar mensaje en un chat o canal** (Microsoft Teams).
   - **Mensaje**:
   ```
   ❌ Solicitud RECHAZADA
   📋 Título: @{triggerOutputs()?['body/Title']}
   👤 Solicitante: @{triggerOutputs()?['body/Solicitante']}
   💬 Motivo: @{outputs('Iniciar_y_esperar_una_aprobación')?['body/responses'][0]['comments']}
   📅 Fecha: @{utcNow()}
   ```

9. Hacer clic en **Guardar** en la esquina superior derecha.

10. Verificar que el flujo se guarda sin errores y muestra estado **Activado**.

### Resultado Esperado

- Flujo `ProcesoEmpresarialMaestroFlow` guardado y activado.
- Estructura: Desencadenador → Aprobación → Condición → (Sí: ERP + Actualizar + Notificar) / (No: Actualizar + Notificar).

### Verificación

- El flujo aparece en la lista "Mis flujos" con estado "Activado".
- Todas las conexiones (SharePoint, Aprobaciones, Teams, ERPSimuladoConnector) muestran icono verde (conectadas).
- Tomar captura de la vista general del flujo y guardar en `C:\LabAgentes\capturas\lab04-flujo-maestro.png`.

---

## Paso 4: Crear el Tema 'ResumenProceso' en el Agente

### Objetivo
Añadir al agente `AgenteEmpresarialLab` un tema que permita al usuario consultar el historial completo de una solicitud, incluyendo su estado actual y número de orden ERP si fue completada.

### Instrucciones

1. Navegar a https://copilotstudio.microsoft.com/environments/LabPractice-M2.

2. Abrir el agente `AgenteEmpresarialLab` (creado según convención: `Agente-[TipoCasoUso]-[Inicial]`).

3. En el panel lateral, seleccionar **Temas** → **+ Nuevo tema** → **Desde cero**.

4. Nombrar el tema: `ResumenProceso`.

5. Configurar las **frases desencadenadoras** (agregar al menos 5):
   - `Quiero ver el resumen de una solicitud`
   - `¿Cuál es el estado completo de mi solicitud?`
   - `Dame el historial de la solicitud`
   - `Consultar proceso de solicitud`
   - `Resumen del proceso empresarial`

6. Agregar un nodo **Preguntar** (Question):
   - Mensaje: `¿Cuál es el título exacto de la solicitud que deseas consultar?`
   - Identificar como: **Respuesta completa del usuario**
   - Guardar en variable: `varTituloSolicitud`

7. Agregar un nodo **Acción** → **Llamar a una acción** → **Crear un flujo**.

8. En Power Automate (se abre en nueva pestaña), crear un flujo auxiliar:
   - **Nombre**: `ConsultarEstadoSolicitudFlow`
   - **Desencadenador**: "Cuando Copilot Studio llama a un flujo" (Power Virtual Agents).
   - Agregar parámetro de entrada tipo texto: `TituloSolicitud`
   - **Acción**: Obtener elementos (SharePoint):
     - Lista: `SolicitudesLab`
     - Consulta de filtro: `Title eq '@{triggerBody()?['text']}'`
     - Máximo de elementos: `1`
   - **Acción**: Componer (Compose):
     - Entrada:
     ```json
     {
       "titulo": "@{first(outputs('Obtener_elementos')?['body/value'])?['Title']}",
       "estado": "@{first(outputs('Obtener_elementos')?['body/value'])?['Estado']}",
       "prioridad": "@{first(outputs('Obtener_elementos')?['body/value'])?['Prioridad']}",
       "solicitante": "@{first(outputs('Obtener_elementos')?['body/value'])?['Solicitante']}",
       "ordenERP": "@{first(outputs('Obtener_elementos')?['body/value'])?['OrdenERP']}"
     }
     ```
   - **Acción de retorno**: "Devolver valores a Copilot Studio":
     - Agregar salida tipo texto: `ResumenJSON` con valor `@{outputs('Componer')}`
   - Guardar el flujo.

9. De vuelta en Copilot Studio, seleccionar el flujo `ConsultarEstadoSolicitudFlow`.
   - Mapear entrada `TituloSolicitud` → `varTituloSolicitud`
   - Salida `ResumenJSON` → guardar en variable `varResumenJSON`

10. Agregar un nodo **Mensaje** después de la acción:
    ```
    📊 **Resumen del Proceso**
    
    Aquí está el historial completo de tu solicitud:
    
    {varResumenJSON}
    
    Si necesitas más detalles o deseas iniciar una nueva solicitud, házmelo saber.
    ```

11. Agregar un nodo **Finalizar conversación** o redirigir al tema del sistema "Fin de conversación".

12. Hacer clic en **Guardar** en el tema.

### Resultado Esperado

- Tema `ResumenProceso` creado con flujo auxiliar conectado.
- El agente puede recibir un título de solicitud y devolver su estado, prioridad, solicitante y número de orden ERP.

### Verificación

- En el panel de prueba de Copilot Studio, escribir "Dame el resumen de una solicitud" y verificar que el tema se activa correctamente.
- Proporcionar el título de una solicitud existente en `SolicitudesLab` y confirmar que retorna datos válidos.
- Tomar captura: `C:\LabAgentes\capturas\lab04-tema-resumen.png`.

---

## Paso 5: Ejecutar Pruebas de Aceptación (3 Escenarios)

### Objetivo
Validar el proceso completo end-to-end ejecutando tres escenarios distintos que cubran los caminos principales del flujo maestro.

### Instrucciones

#### Escenario 1: Solicitud de Prioridad Alta — Aprobada

1. En el panel de prueba de Copilot Studio, activar el tema `NuevaSolicitud` (creado en Lab 02).

2. Proporcionar los datos:
   - Título: `Compra-Urgente-Servidor-E1`
   - Prioridad: `Alta`
   - Descripción: `Adquisición urgente de servidor para migración de producción`

3. Verificar que se crea el elemento en la lista `SolicitudesLab` de SharePoint.

4. Esperar la notificación de aprobación (llegará al correo o a la app de Aprobaciones de Teams).

5. **Aprobar** la solicitud con comentario: `Aprobada por urgencia operativa`.

6. Verificar en SharePoint:
   - Estado cambiado a `Completada`
   - Campo `OrdenERP` contiene el valor `101`

7. Verificar en Teams: mensaje de confirmación con emoji ✅ publicado en el canal General.

8. Registrar resultado en la bitácora.

#### Escenario 2: Solicitud de Prioridad Alta — Rechazada

1. Crear nueva solicitud desde el agente:
   - Título: `Licencia-Premium-Marketing-E2`
   - Prioridad: `Alta`
   - Descripción: `Solicitud de licencia premium de herramienta de marketing digital`

2. Esperar la aprobación.

3. **Rechazar** la solicitud con comentario: `Presupuesto insuficiente para Q3`.

4. Verificar en SharePoint:
   - Estado cambiado a `Rechazada`
   - Campo `OrdenERP` vacío/nulo

5. Verificar en Teams: mensaje con emoji ❌ y motivo de rechazo visible.

#### Escenario 3: Solicitud de Prioridad Normal — Aprobada

1. Crear nueva solicitud:
   - Título: `Suscripcion-Herramienta-Analisis-E3`
   - Prioridad: `Normal`
   - Descripción: `Renovación de suscripción anual de herramienta de análisis de datos`

2. Esperar la aprobación.

3. **Aprobar** con comentario: `Renovación estándar autorizada`.

4. Verificar el ciclo completo (SharePoint actualizado, Teams notificado, Orden ERP generada).

#### Prueba del Tema ResumenProceso

5. En el panel de prueba del agente, escribir: `Quiero ver el resumen de una solicitud`.

6. Cuando el agente pregunte el título, escribir: `Compra-Urgente-Servidor-E1`.

7. Verificar que el agente retorna el estado `Completada` y el número de orden ERP.

### Resultado Esperado

| Escenario | Estado Final | Orden ERP | Notificación Teams |
|-----------|-------------|-----------|-------------------|
| E1 - Alta Aprobada | Completada | 101 | ✅ Mensaje publicado |
| E2 - Alta Rechazada | Rechazada | — | ❌ Mensaje con motivo |
| E3 - Normal Aprobada | Completada | 101 | ✅ Mensaje publicado |

### Verificación

- Los tres flujos ejecutados aparecen en el historial de ejecuciones de `ProcesoEmpresarialMaestroFlow` con estado "Correcto" (o "Exitoso").
- La lista SharePoint `SolicitudesLab` muestra los tres registros con estados correctos.
- El canal de Teams tiene los tres mensajes de notificación.
- El tema `ResumenProceso` retorna datos correctos.
- Tomar capturas de cada escenario: `lab04-escenario1.png`, `lab04-escenario2.png`, `lab04-escenario3.png`.

---

## Paso 6: Exportar la Solución Completa de Power Platform

### Objetivo
Empaquetar todos los componentes creados durante el Módulo 4 (flujos, conector personalizado, agente) en una solución de Power Platform exportable como archivo `.zip` no administrado.

### Instrucciones

1. Navegar a https://make.powerapps.com → seleccionar entorno **LabPractice-M2**.

2. En el menú lateral, seleccionar **Soluciones**.

3. Si no existe una solución para el módulo, crear una nueva:
   - Hacer clic en **+ Nueva solución**.
   - **Nombre para mostrar**: `SolucionModulo4Lab`
   - **Nombre**: `SolucionModulo4Lab`
   - **Editor**: seleccionar el editor predeterminado o crear uno (`LabAgentes`).
   - **Versión**: `1.0.0.0`
   - Hacer clic en **Crear**.

4. Abrir la solución `SolucionModulo4Lab`.

5. Agregar componentes existentes:
   - Hacer clic en **Agregar existente** → **Automatización** → **Flujo de nube**:
     - Seleccionar `ProcesoEmpresarialMaestroFlow`
     - Seleccionar `ConsultarEstadoSolicitudFlow`
     - Seleccionar `RegistrarSolicitudAprobacionFlow` (del Lab 03)
   - Hacer clic en **Agregar existente** → **Conector personalizado**:
     - Seleccionar `ERPSimuladoConnector`
   - Hacer clic en **Agregar existente** → **Chatbot**:
     - Seleccionar `AgenteEmpresarialLab`

6. Verificar que todos los componentes aparecen en la solución (mínimo 5 elementos).

7. Exportar la solución:
   - Hacer clic en **Exportar** (botón en la barra superior).
   - Seleccionar **Siguiente** en el diálogo de comprobaciones.
   - Tipo de exportación: **No administrada**.
   - Hacer clic en **Exportar**.
   - Esperar la descarga del archivo `.zip`.

8. Mover el archivo descargado a la carpeta del laboratorio:

```powershell
# Windows
Move-Item "$env:USERPROFILE\Downloads\SolucionModulo4Lab_1_0_0_0.zip" "C:\LabAgentes\Lab04\SolucionModulo4Lab.zip"
```

```bash
# macOS
mv ~/Downloads/SolucionModulo4Lab_1_0_0_0.zip ~/LabAgentes/Lab04/SolucionModulo4Lab.zip
```

9. Verificar el contenido del archivo:

```powershell
# Windows - Listar contenido del ZIP
Expand-Archive -Path "C:\LabAgentes\Lab04\SolucionModulo4Lab.zip" -DestinationPath "C:\LabAgentes\Lab04\SolucionExtraida" -Force
Get-ChildItem "C:\LabAgentes\Lab04\SolucionExtraida" -Recurse | Select-Object Name, Length
```

### Resultado Esperado

- Archivo `SolucionModulo4Lab.zip` generado (tamaño típico: 500 KB – 5 MB).
- Al extraer, contiene carpetas como `Workflows/`, `Connectors/`, `botcomponents/` y archivo `solution.xml`.

### Verificación

- El archivo `.zip` existe en `C:\LabAgentes\Lab04\`.
- El archivo `solution.xml` dentro del ZIP contiene referencias a todos los componentes agregados.
- Tomar captura de la lista de componentes en la solución: `lab04-solucion-exportada.png`.

---

## Paso 7: Documentar la Arquitectura del Proceso

### Objetivo
Crear un documento de arquitectura que describe todos los componentes, conectores y flujos del proceso empresarial integrado.

### Instrucciones

1. En Visual Studio Code, crear el archivo `C:\LabAgentes\docs\arquitectura-proceso-m4.md`.

2. Documentar la arquitectura usando la siguiente plantilla:

```markdown
# Arquitectura del Proceso Empresarial Integrado - Módulo 4

## Diagrama de Componentes

```
[Usuario] → [Agente Copilot Studio] → [Tema: NuevaSolicitud]
                                            ↓
                                    [SharePoint: SolicitudesLab]
                                            ↓
                              [ProcesoEmpresarialMaestroFlow]
                                            ↓
                                    [Aprobación Humana]
                                      ↙         ↘
               [Aprobada]                        [Rechazada]
                   ↓                                 ↓
        [ERPSimuladoConnector]              [Actualizar: Rechazada]
                   ↓                                 ↓
        [Actualizar: Completada]            [Notificar Teams ❌]
                   ↓
        [Notificar Teams ✅]
```

## Componentes del Sistema

| # | Componente | Tipo | Función |
|---|-----------|------|---------|
| 1 | AgenteEmpresarialLab | Copilot Studio Agent | Punto de entrada conversacional |
| 2 | Tema NuevaSolicitud | Copilot Studio Topic | Captura datos de solicitud |
| 3 | Tema ResumenProceso | Copilot Studio Topic | Consulta historial |
| 4 | SolicitudesLab | SharePoint List | Almacén de datos maestro |
| 5 | ProcesoEmpresarialMaestroFlow | Power Automate Flow | Orquestador principal |
| 6 | ConsultarEstadoSolicitudFlow | Power Automate Flow | Consulta para agente |
| 7 | ERPSimuladoConnector | Custom Connector | Integración ERP simulada |
| 8 | Canal General (Teams) | Microsoft Teams | Notificaciones |

## Conectores Utilizados

| Conector | Tipo | Autenticación |
|----------|------|---------------|
| SharePoint | Estándar | OAuth 2.0 (Microsoft 365) |
| Aprobaciones | Estándar | Contexto de usuario |
| Microsoft Teams | Estándar | OAuth 2.0 (Microsoft 365) |
| ERPSimuladoConnector | Personalizado | Sin autenticación |

## Flujo de Datos

1. Usuario inicia conversación con el agente
2. Agente captura: título, prioridad, descripción, solicitante
3. Flujo auxiliar crea elemento en SharePoint
4. Desencadenador del flujo maestro se activa
5. Flujo maestro solicita aprobación humana
6. Si aprobada: POST a ERP → actualizar SharePoint → notificar Teams
7. Si rechazada: actualizar SharePoint → notificar Teams
8. Usuario puede consultar estado vía tema ResumenProceso

## Métricas de Prueba

| Escenario | Tiempo ejecución | Resultado |
|-----------|-----------------|-----------|
| E1 - Alta Aprobada | ~X min | ✅ Exitoso |
| E2 - Alta Rechazada | ~X min | ✅ Exitoso |
| E3 - Normal Aprobada | ~X min | ✅ Exitoso |

## Autor y Fecha
- Participante: [Nombre]
- Fecha: [Fecha actual]
- Versión: 1.0
```

3. Completar los campos marcados con `[...]` con los datos reales del laboratorio.

4. Guardar el archivo.

### Resultado Esperado

- Documento de arquitectura completo en formato Markdown.
- Todas las secciones rellenadas con datos reales del laboratorio.

### Verificación

- El archivo existe en la ruta correcta y es legible.
- Contiene los 8 componentes documentados.
- Las métricas de prueba reflejan los resultados reales obtenidos en el Paso 5.

---

## Validación y Pruebas Finales

### Lista de Verificación de Entregables

| # | Entregable | Ubicación | Estado |
|---|-----------|-----------|--------|
| 1 | Definición OpenAPI | `C:\LabAgentes\Lab04\erp-simulado-openapi.json` | ☐ |
| 2 | Conector ERPSimuladoConnector | Power Platform - LabPractice-M2 | ☐ |
| 3 | Flujo ProcesoEmpresarialMaestroFlow | Power Automate - LabPractice-M2 | ☐ |
| 4 | Flujo ConsultarEstadoSolicitudFlow | Power Automate - LabPractice-M2 | ☐ |
| 5 | Tema ResumenProceso | Copilot Studio - AgenteEmpresarialLab | ☐ |
| 6 | 3 ejecuciones exitosas documentadas | Historial de flujos + capturas | ☐ |
| 7 | Solución exportada (.zip) | `C:\LabAgentes\Lab04\SolucionModulo4Lab.zip` | ☐ |
| 8 | Documento de arquitectura | `C:\LabAgentes\docs\arquitectura-proceso-m4.md` | ☐ |
| 9 | Capturas de evidencia (mín. 6) | `C:\LabAgentes\capturas\` | ☐ |

### Prueba de Integración Final

1. Ejecutar una solicitud adicional de extremo a extremo sin intervención manual (excepto la aprobación):
   - Iniciar desde el agente → verificar creación en SharePoint → aprobar → verificar ERP + Teams.
   
2. Inmediatamente después, usar el tema `ResumenProceso` para consultar la solicitud recién creada.

3. Confirmar que toda la cadena funciona de forma coherente y sin errores.

---

## Solución de Problemas

### Problema 1: El conector personalizado retorna error 404 o timeout al probar

**Síntomas:** Al ejecutar la operación `CrearOrdenERP` en el flujo maestro o en la prueba del conector, se recibe un error HTTP 404 (Not Found) o la solicitud excede el tiempo de espera.

**Causa:** La URL base del servidor en la definición OpenAPI está mal configurada, o el campo `servers` tiene un formato incorrecto (por ejemplo, incluye una barra diagonal final `/posts` en la URL base en lugar de solo en el path). También puede ocurrir si la red corporativa bloquea solicitudes salientes a dominios externos.

**Solución:**
1. Verificar que el campo `servers` en el JSON contiene exactamente: `"url": "https://jsonplaceholder.typicode.com"` (sin barra final ni path adicional).
2. En Power Platform, editar el conector → pestaña General → verificar que Host = `jsonplaceholder.typicode.com` y Base URL = `/`.
3. Si hay bloqueo de red, probar desde una red diferente o solicitar al administrador que permita tráfico saliente a `jsonplaceholder.typicode.com`.
4. Probar el endpoint directamente en el navegador: `https://jsonplaceholder.typicode.com/posts` debe retornar un array JSON.

---

### Problema 2: El flujo maestro se activa pero la aprobación nunca llega al aprobador

**Síntomas:** El flujo `ProcesoEmpresarialMaestroFlow` inicia correctamente (visible en el historial de ejecuciones con estado "En ejecución"), pero el aprobador designado no recibe la solicitud de aprobación ni en correo electrónico ni en la app de Aprobaciones de Teams.

**Causa:** El campo "Asignado a" en la acción de aprobación contiene un formato de correo incorrecto, el usuario no tiene licencia de Power Automate asignada, o el servicio de Aprobaciones no está habilitado en el entorno. También puede ocurrir si el correo del aprobador tiene un espacio en blanco al inicio o final.

**Solución:**
1. Editar el flujo → acción "Iniciar y esperar una aprobación" → verificar que el campo "Asignado a" contiene el correo exacto sin espacios: `usuario[N]@labagentes[N].onmicrosoft.com`.
2. Verificar en el Centro de Administración de Power Platform que el usuario tiene una licencia válida (E3 o superior con complemento).
3. Navegar a https://flow.microsoft.com/manage/environments → seleccionar LabPractice-M2 → verificar que "Aprobaciones" está en la lista de soluciones del entorno.
4. Como workaround temporal, cancelar la ejecución actual, corregir el correo y crear un nuevo elemento en SharePoint para re-disparar el flujo.

---

## Limpieza

> **Importante:** NO eliminar los componentes creados en este laboratorio. La solución exportada y los flujos activos serán utilizados como referencia en el laboratorio 05-00-01 (análisis de métricas).

Acciones de limpieza parcial permitidas:

1. **Desactivar el flujo maestro** (para evitar ejecuciones no deseadas durante el período entre laboratorios):
   - En Power Automate → Mis flujos → `ProcesoEmpresarialMaestroFlow` → **Desactivar**.

2. **Limpiar elementos de prueba en SharePoint** (opcional):
   - Mantener al menos los 3 registros de los escenarios de prueba (E1, E2, E3).
   - Eliminar solo registros duplicados o fallidos.

3. **Cerrar sesiones activas** en Postman y Visual Studio Code.

4. **Verificar almacenamiento**: confirmar que la carpeta `C:\LabAgentes\Lab04\` no excede 50 MB.

---

## Resumen

En este laboratorio integrador has construido un proceso empresarial end-to-end completo que demuestra cómo un agente de IA puede actuar como punto de entrada único para orquestar operaciones complejas que involucran múltiples sistemas. Los conceptos clave aplicados incluyen:

- **Ciclo ReAct en la práctica**: el agente razona sobre la solicitud del usuario, invoca herramientas (flujos de Power Automate) y sintetiza resultados, implementando el patrón de razonamiento con herramientas estudiado en la lección 4.1.
- **Conectores personalizados**: la creación del `ERPSimuladoConnector` con OpenAPI 3.0 demuestra cómo extender las capacidades del agente hacia sistemas externos, convirtiendo una API REST en una herramienta invocable.
- **Orquestación multi-paso**: el flujo maestro implementa un proceso con ramificaciones condicionales, integrando aprobaciones humanas con automatización, reflejando escenarios empresariales reales.
- **Exportación de soluciones**: el empaquetado como solución no administrada garantiza la portabilidad y reproducibilidad del sistema completo.

### Recursos Adicionales

| Recurso | Enlace |
|---------|--------|
| Documentación de conectores personalizados | https://learn.microsoft.com/es-es/connectors/custom-connectors/ |
| Referencia OpenAPI 3.0 | https://swagger.io/specification/ |
| Aprobaciones en Power Automate | https://learn.microsoft.com/es-es/power-automate/get-started-approvals |
| Exportar soluciones Power Platform | https://learn.microsoft.com/es-es/power-apps/maker/data-platform/export-solutions |
| Patrón ReAct en agentes | https://arxiv.org/abs/2210.03629 |
| JSONPlaceholder API | https://jsonplaceholder.typicode.com/guide/ |

---
