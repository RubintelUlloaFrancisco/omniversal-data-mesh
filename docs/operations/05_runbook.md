# 📄 ECOSISTEMA OMNIVERSAL V20.0‑PRO
**Manual de Operaciones y Despliegue (Runbook)**

| Metadato | Valor |
| :--- | :--- |
| **Documento** | OMNI‑RUNBOOK V20.0‑PRO |
| **Versión** | 1.0.0 |
| **Clasificación** | Confidencial – Uso Interno / Equipo de Operaciones |
| **Autor** | Rubintel Ulloa Francisco (Arquitectura de Gobernanza DAMA‑DMBOK2 · SRE) |
| **Fecha** | Junio 2026 |

**Propósito:** Proporcionar al equipo de operaciones una guía de referencia rápida para la puesta en marcha, monitorización, interpretación de los HUDs y procedimientos de apagado seguro del ecosistema Omniversal.

---

## 📋 1. PRERREQUISITOS

Antes de ejecutar cualquier celda, verifique que el entorno de Google Colab cumple con los siguientes requisitos:

| Requisito | Verificación |
| :--- | :--- |
| **Conexión a Internet** | La VM de Colab debe tener acceso a `googleapis.com`. |
| **Dependencias Python** | Las celdas instalan automáticamente las librerías faltantes. Si una celda falla con `ModuleNotFoundError`, ejecute `!pip install <librería>` en una celda aparte. |
| **API Key de Gemini** | (Solo para Celda 05). Debe estar almacenada en los Secrets de Colab con el nombre exacto `GEMINI_API_KEY`. El interruptor "Acceso al cuaderno" debe estar activado. |
| **Espacio en Drive** | El montaje FUSE requiere al menos 100 MB libres para los artefactos de gobernanza. |

> **Nota:** La Celda 05 (Motor Cognitivo Gemini) es opcional. Si no se dispone de API Key, la celda se iniciará en modo BLOQUEO DE SEGURIDAD y no afectará al resto del ecosistema.

---

## 🚀 2. ORDEN CANÓNICO DE EJECUCIÓN

El ecosistema está diseñado bajo el principio de **Inversión de Control (IoC)**. Cada celda verifica que la celda anterior haya finalizado correctamente antes de operar. El orden de ejecución es estricto e innegociable.

| Paso | Celda | Acción requerida | ¿Automático? | Indicador de éxito |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **01 – Gateway** | Ejecutar la celda. | ✅ Sí (arranque secuencial) | HUD muestra "SISTEMA EN LÍNEA" y barra de progreso al 100 %. |
| **2** | **02 – Bronce** | Ejecutar la celda. Luego, presionar `▶ Iniciar / Reanudar` en el HUD. | ❌ No (requiere interacción) | Barra llega a 100 %, suena ping de audio, HUD muestra "SINCRONIZADO (100%)". |
| **3** | **03 – Plata** | Ejecutar la celda. Presionar `⚙️ Certificar Data Quality`. | ❌ No | Barra llega a 100 %, HUD muestra "ACTIVO DE DATOS CERTIFICADO". |
| **4** | **04 – Oro** | Ejecutar la celda. Presionar `📊 Materializar Data Marts`. | ❌ No | Barra llega a 100 %, HUD muestra "INTELIGENCIA DE NEGOCIO ACTIVA". |
| **5** | **05 – IA** | Ejecutar la celda. | ✅ Sí (si API Key configurada)| HUD muestra "MOTOR COGNITIVO EN LÍNEA". |
| **6** | **06 – Remediación**| Ejecutar celda. Seleccionar modo, presionar `🔍 Generar Manifiesto`. Revisar y presionar `⚠️ APROBAR Y EJECUTAR`. | ❌ No (requiere revisión humana) | HUD muestra "REMEDIACIÓN COMPLETADA CON ÉXITO". |
| **7** | **00 – Torre** | Ejecutar la celda. Presionar `⟳ Sincronizar Panóptico`. | ✅ Sí (refresco automático) | 6 tarjetas en azul. Estado: SALUDABLE. |

> **⚠️ Regla de oro:** Nunca ejecute una celda sin haber verificado que la anterior completó su ciclo. Si una celda muestra BLOQUEO IoC, regrese a la celda anterior y asegúrese de que finalizó correctamente.

---

## 🖥️ 3. INTERPRETACIÓN DE LOS HUDS

### 3.1 Estados comunes en todas las celdas
| Estado del HUD | Significado | Acción recomendada |
| :--- | :--- | :--- |
| **SISTEMA EN LÍNEA / NOMINAL** | La celda ha completado su arranque y está lista para operar. | Proceder con la siguiente celda. |
| **SISTEMA OFFLINE / BLOQUEO IoC** | Dependencia requerida no disponible (Gateway no ejecutado, checkpoint no existe). | Ejecutar la celda anterior y verificar finalización. |
| **FALLO CRÍTICO / TRANSFORMACIÓN** | Ha ocurrido un error durante la ejecución. | Revisar panel de auditoría forense (desplegable ︾) para RCA. |
| **PRESUPUESTO AGOTADO** | El Circuit Breaker FinOps ha detenido la operación. | Ajustar el slider de presupuesto o revisar modo de operación. |

### 3.2 Interpretación del panopticon_evidence.json (Celda 00)
El archivo contiene un snapshot del estado de todo el ecosistema:
* 🔵 **Azul (`#007aff`):** Celda operativa y saludable (SANO, ACTIVO, READY, GOLD, BRAIN, ACTIVE).
* 🟠 **Naranja (`#ff9500`):** Celda en estado de alerta (CAÍDO, DEGRADADO).
* ⚪ **Gris (`#8E8E93`):** Celda no ejecutada (IDLE, PENDIENTE).

---

## 🔄 4. REANUDACIÓN DE INGESTA DESDE CHECKPOINT (CELDA 02)

La Celda 02 es la única que puede requerir reanudación tras una interrupción.

**4.1 Escenario: La ingesta se detuvo antes de completarse**
1. Vuelva a ejecutar la Celda 02.
2. El HUD mostrará el estado del checkpoint existente (ej. "Reanudando desde lote 48...").
3. Presione `▶ Reanudar Extracción`.
4. La ingesta continuará desde el último `pageToken`. Los chunks ya escritos se conservan.

**4.2 Escenario: Desea reiniciar la ingesta desde cero**
1. Elimine manualmente `state.checkpoint.json` y el directorio `data/chunks/`.
2. Vuelva a ejecutar la Celda 02.
3. Presione `▶ Iniciar / Reanudar`. Comenzará una extracción limpia.

---

## 🛑 5. PROCEDIMIENTO DE APAGADO SEGURO (ZERO TRUST)

El protocolo de Ciclo de Vida Zero Trust garantiza que ningún activo (túnel ADC, FUSE, KMS) quede expuesto.

**5.1 Apagado formal desde la Celda 01**
1. En el HUD de la Celda 01, presione `⚡ Apagar (Zero Trust)`.
2. El sistema ejecutará automáticamente: Revocación ADC, Desmontaje FUSE, Destrucción KMS, Eliminación `gateway_ready.json`, recolección de basura (`gc.collect()`).
3. El HUD mostrará "APAGADO SEGURO" y cambiará a ENTORNO ESTÉRIL.

**5.2 Apagado de emergencia (reinicio de la VM)**
1. Al re-ejecutar la Celda 01, el arranque adaptativo purgará los residuos.
2. Si existía `01_emergency_ledger.jsonl`, se consolidará automáticamente.
3. Celdas subsiguientes deberán reanudarse desde sus checkpoints.

---

## ⚙️ 6. CONFIGURACIÓN DEL REFRESCO AUTOMÁTICO (CELDA 00)

1. Ejecute la Celda 00.
2. Marque el checkbox "Refresco Auto (60s)".
3. Un hilo daemon leerá las evidencias de todas las celdas cada 60 segundos y actualizará el HUD.
4. Para detener el refresco, desmarque el checkbox. Se puede forzar manual con `⟳ Sincronizar Panóptico`.

---

## 🔧 7. RESOLUCIÓN DE INCIDENCIAS COMUNES

| Síntoma | Causa probable | Solución |
| :--- | :--- | :--- |
| **Celda 02 "SISTEMA OFFLINE"** | Celda 01 no se ha ejecutado. | Ejecutar Celda 01 y verificar "SISTEMA EN LÍNEA". |
| **Celda 03 "IoC_FAIL: Celda 02 no ha generado checkpoint"** | Ingesta de Celda 02 no ha finalizado. | Regresar a Celda 02, presionar `▶ Iniciar` y esperar al 100 %. |
| **Celda 05 "BLOQUEO DE SEGURIDAD: Secret GEMINI... missing"** | API Key no configurada en Secrets. | Ir a menú Secrets (🔑), añadir `GEMINI_API_KEY` y activar acceso. |
| **Celda 00 muestra Gateway CAÍDO, pero Celda 01 está en línea** | Celda 00 ejecutada antes que el Gateway. | Presionar `⟳ Sincronizar Panóptico` en la Celda 00. |

---

## 📞 8. CANALES DE SOPORTE

Para incidencias complejas, el ecosistema genera trazas forenses en `/log/`:
* `01_audit_master_ledger.jsonl` – eventos del Gateway.
* `02_audit_master_ledger.jsonl` – eventos de ingesta.
* `03_audit_dq_ledger.jsonl` – eventos de calidad.
* `04_business_audit.jsonl` – eventos de negocio.
* `05_audit_ai_ledger.jsonl` – eventos de inferencia IA.
* `06_audit_remediation_ledger.jsonl` – eventos de remediación.
* `00_audit_master_ledger.jsonl` – eventos de Torre de Control.
* `panopticon_evidence.json` – snapshot de estado general.

Cada entrada contiene `global_trace_id`, timestamp UTC, nivel (OK, WARN, CRÍTICO) y RCA de causa raíz.

> *"Un sistema bien diseñado es aquel que puede ser operado por cualquier miembro del equipo, no solo por su creador."*
