# Ecosistema de Automatización IA para Gestión de Leads

**Proyecto Final – Javier Sibar**

Este proyecto implementa un ecosistema de automatización para la gestión de leads, integrando una base de datos centralizada, procesamiento mediante inteligencia artificial, aprobación humana, manejo de errores y monitoreo del proceso.

## Arquitectura

El ecosistema utiliza:

- **n8n** como orquestador de la automatización.
- **Airtable** como base de datos central.
- **Google Gemini** como modelo de inteligencia artificial.
- **Gmail** para el proceso de Human in the Loop (HITL).
- **Airtable Interface** como dashboard de control.

El flujo general es:

**Airtable → Validación → Agente IA → Actualización de datos → Gmail HITL → Aprobación/Rechazo → Actualización de Airtable**

Además, el agente de IA dispone de una ruta independiente para el registro de errores.

## Funcionamiento

1. Un nuevo lead ingresa en Airtable.
2. n8n verifica que su estado sea `Pendiente` y que todavía no haya sido procesado.
3. El agente de IA analiza el mensaje.
4. Se asigna una prioridad y se genera una propuesta de respuesta.
5. n8n actualiza el registro en Airtable.
6. Gmail envía la propuesta para revisión humana.
7. El workflow espera la decisión del usuario.
8. Si se aprueba, el lead pasa a `Aprobado por Humano`.
9. Si se rechaza, el lead pasa a `Rechazado`.
10. Los errores del procesamiento de IA se registran en una tabla específica de Airtable.

## Human in the Loop

Antes de completar el proceso se requiere una decisión humana mediante Gmail.

La persona responsable puede aceptar o rechazar la respuesta propuesta por la inteligencia artificial. El workflow permanece en espera hasta recibir esta decisión.

Esto permite automatizar el análisis y la generación de respuestas sin eliminar el control humano sobre las decisiones críticas.

## Manejo de errores

El agente de IA cuenta con una salida específica de error.

Ante una incidencia, el workflow continúa por una ruta alternativa y registra el error y su fecha en la tabla `Registro de Errores` de Airtable.

También se implementó una validación previa para evitar reprocesamientos y posibles bucles.

## Base de datos

La base de Airtable está compuesta por:

- **Leads**
- **Contactos**
- **Registro de Errores**

Los leads utilizan diferentes estados para mantener trazabilidad durante todo el proceso:

`Pendiente` → `Procesado por IA` → `Aprobado por Humano` / `Rechazado`

## Dashboard de control

El proyecto incluye un dashboard en Airtable para monitorear:

- Total de leads.
- Leads aprobados por humano.
- Leads rechazados.
- Distribución por estado.
- Errores registrados.

### Enlaces de Airtable

**Dashboard / Interface:**  
https://airtable.com/appc1tVfrfVDp1u1g/pagT7dwtbovAYPRU4

**Vista pública de la base de datos:**  
https://airtable.com/appc1tVfrfVDp1u1g/shrElrLdXH1BilT2k

> La vista pública permite consultar los registros sin necesidad de iniciar sesión en Airtable.

## Video demostrativo

Demostración completa del funcionamiento y arquitectura del proyecto:

https://youtu.be/5dF_WkuQWMo

## Documentación incluida

Este repositorio contiene:

- `Javier Sibar - Ecosistema IA.json` – Backup/exportación del workflow de n8n.
- `Diagrama_Arquitectura_Ecosistema_IA_Javier_Sibar.pdf` – Diagrama de arquitectura.
- `Manual_de_Datos_y_Esquemas_JSON_Javier_Sibar.docx` – Manual de datos y esquemas JSON.
- `Matriz_de_Costos_Ecosistema_IA_Javier_Sibar.docx` – Matriz de costos y ahorro estimado.
- `Seguridad_y_Resiliencia_Ecosistema_IA_Javier_Sibar.docx` – Seguridad, resiliencia, manejo de errores y HITL.

## Pruebas

El workflow fue probado mediante múltiples ejecuciones, incluyendo:

- Procesamiento exitoso de leads.
- Validación de leads pendientes.
- Prevención de reprocesamiento.
- Clasificación mediante IA.
- Generación de respuestas.
- Aprobación humana mediante Gmail.
- Rechazo mediante HITL.
- Caminos alternativos.
- Registro de incidencias.

## Seguridad

Las credenciales y claves de API no están incluidas en este repositorio.

Las conexiones con Airtable, Gmail y el modelo de inteligencia artificial se administran mediante el sistema de credenciales de n8n.

---

**Autor:** Javier Sibar  
**Proyecto Final – Ecosistema de Automatización IA para Negocios**
