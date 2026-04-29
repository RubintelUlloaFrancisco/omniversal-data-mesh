# 📄 ECOSISTEMA OMNIVERSAL V20.0‑PRO
**Guía de Troubleshooting y RCA (Root Cause Analysis)**

| Metadato | Valor |
| :--- | :--- |
| **Documento** | OMNI‑TROUBLESHOOT V20.0‑PRO |
| **Versión** | 1.0.0 |
| **Clasificación** | Confidencial – Uso Interno / Equipo de Operaciones y SRE |
| **Autor** | Rubintel Ulloa Francisco (Arquitectura de Gobernanza DAMA‑DMBOK2 · SRE) |
| **Fecha** | Junio 2026 |

**Propósito:** Proporcionar un catálogo de referencia rápida de los síntomas comunes en los HUDs, su causa raíz (RCA) y el procedimiento de corrección validado.

---

## 🔍 1. METODOLOGÍA DE DIAGNÓSTICO

Ante cualquier incidencia, siga este protocolo de 3 pasos:
1. **Leer el HUD:** La caja de error (`.error-box`) contiene el mensaje de RCA.
2. **Desplegar Auditoría Forense:** (︾) en la celda para ver los últimos eventos del ledger.
3. **Consultar esta guía:** Buscar el síntoma y aplicar la solución.
*(Si el síntoma no aparece, abrir `panopticon_evidence.json` para estado general).*

---

## 🟠 2. CATÁLOGO DE SÍNTOMAS POR CELDA

### 2.1 Celda 01 – Master Gateway
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **01‑E1** | "FALLO DE INFRAESTRUCTURA" – barra detenida | Montaje FUSE falló (conflicto previo o sin internet). | 1. Entorno → Desconectar y eliminar. 2. Re-ejecutar Celda 01 (arranque adaptativo purga). |
| **01‑E2** | "FALLO DE INFRAESTRUCTURA" – `ADC_AUTH_FAIL` | Falla ADC. Ventanas emergentes bloqueadas. | 1. Permitir pop-ups para colab. 2. Re-ejecutar y aceptar diálogo. |
| **01‑E3** | "FALLO DE INFRAESTRUCTURA" – `DAMA_INTEGRITY_FAIL`| Falla crear ontología. Permisos insuficientes en Drive. | 1. Verificar permisos en `000_Data_Organizacion`. 2. Eliminar manual si está corrupto y re-ejecutar. |
| **01‑E4** | Watchdog "INACTIVO" pero sistema operativo | Hilo no inició (condición de carrera rara). | Re-ejecutar Celda 01. |
| **01‑E5** | Emergency Ledger muestra "SÍ" | Sesión anterior terminó con FUSE caído. | No requiere acción. Se consolidará automáticamente en el próximo arranque. |

### 2.2 Celda 02 – Capa Bronce
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **02‑E1** | "SISTEMA OFFLINE" – `MISSING_GATEWAY` | Celda 01 no ejecutada. `gateway` no existe. | 1. Ejecutar Celda 01 ("SISTEMA EN LÍNEA"). 2. Re-ejecutar Celda 02. |
| **02‑E2** | "SISTEMA OFFLINE" – `ADC_REVOKED` | Gateway operativo pero túnel revocado. | 1. Re-ejecutar Celda 01. 2. Re-ejecutar Celda 02. |
| **02‑E3** | "FALLO RED" – error en Canary Test | API no responde (corte o cuota). | 1. Verificar red. 2. Esperar 60s, presionar `▶ Reanudar`. 3. Revisar cuotas GCP. |
| **02‑E4** | "PRESUPUESTO AGOTADO" – barra detenida | Costo superó límite de slider. | 1. Aumentar presupuesto, `▶ Reanudar`. 2. O aceptar chunks escritos y seguir a Celda 03. |
| **02‑E5** | Barra estancada <100% sin error | Paginación lenta (red/RAM). | 1. Si RAM >85%, caudal se redujo a 200/lote. 2. Cerrar apps. 3. No requiere intervención (se reanuda solo). |
| **02‑E6** | Contador DLQ aumenta rápido | API devuelve registros fuera de contrato. | 1. Revisar `dlq/schema_drift_records.jsonl`. 2. Actualizar `schema_v1.json` si es legítimo. 3. Re-ejecutar Celda 02. |

### 2.3 Celda 03 – Capa Plata
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **03‑E1** | "BLOQUEO IoC" – Celda 01 no detectada | Falta `gateway_ready.json`. | Ejecutar Celda 01, re-ejecutar Celda 03. |
| **03‑E2** | "BLOQUEO IoC" – Celda 02 sin checkpoint | `state.checkpoint.json` ausente o no COMPLETED. | Regresar a Celda 02, completar al 100%, re-ejecutar Celda 03. |
| **03‑E3** | "BLOQUEO IoC" – `database_raw.parquet` no hallado | Consolidación Bronce falló. | 1. Re-ejecutar Celda 02. 2. Verificar existencia en Drive. |
| **03‑E4** | "FALLO DE TRANSFORMACIÓN" | Fallo tipado/OOM/esquema. | 1. Revisar RCA. 2. Liberar RAM. 3. Verificar contrato Celda 02. |
| **03‑E5** | DQ Score < 85% (naranja) | Nulos en campos críticos. | 1. Revisar `database_raw.parquet`. 2. Si son legacy, seguir. 3. Si no, ajustar contrato y re-ingestar. |

### 2.4 Celda 04 – Capa Oro
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **04‑E1** | "SISTEMA OFFLINE" – `MISSING_GATEWAY` | Celda 01 no ejecutada. | Ejecutar Celda 01, re-ejecutar Celda 04. |
| **04‑E2** | "BLOQUEO IoC" – Plata no detectada | Celda 03 no ejecutada. | 1. Ejecutar Celda 03 (`⚙️ Certificar...`). 2. Re-ejecutar Celda 04. |
| **04‑E3** | "FALLO ANALÍTICO" – error agregaciones | `owners` nulo o mal formado. | 1. Revisar RCA. 2. Verificar Plata. 3. Re-ejecutar Celda 03 con tolerancia si procede. |
| **04‑E4** | Gráfico FinOps no aparece | Data Mart vacío. | 1. Ver auditoría (evento BI.FINAL). 2. Verificar si hubo datos `owners` en Plata. |

### 2.5 Celda 05 – Motor Cognitivo Gemini
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **05‑E1** | "BLOQUEO SEGURIDAD" – Secret missing | API Key no configurada. | 1. Configurar `GEMINI_API_KEY` en Secrets. 2. Activar acceso. 3. Re-ejecutar Celda 05. |
| **05‑E2** | "CUOTA EXCEDIDA (FREE TIER)" | Límite HTTP 429 alcanzado. | 1. Esperar 60s (recuperación auto). 2. Considerar Pay-as-you-go si persiste. |
| **05‑E3** | "PRESUPUESTO AGOTADO – BLOQUEO" | Costo superó slider. | 1. Aumentar slider o activar Free Tier. |
| **05‑E4** | "FALLO SISTÉMICO" – error inferencia | Modelo rechazó prompt. | 1. Revisar RCA. 2. Ajustar prompt a políticas. 3. Re-intentar. |

### 2.6 Celda 06 – Centro de Remediación
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **06‑E1** | "BLOQUEO IoC" – Gateway offline | Falta `gateway_ready.json`. | Ejecutar Celda 01, re-ejecutar Celda 06. |
| **06‑E2** | "BLOQUEO IoC" – Plata no detectada | Celda 03 no ejecutada. | Ejecutar Celda 03, re-ejecutar Celda 06. |
| **06‑E3** | Botón `⚠️ APROBAR` deshabilitado | Conteo = 0. | 1. Probar otro modo. 2. Si ninguno da, Drive está limpio. |
| **06‑E4** | "FALLO EN REMEDIACIÓN" – `API.FAIL` | Permisos insuficientes en archivo. | 1. Revisar IDs en auditoría. 2. Requiere acción manual del propietario. |
| **06‑E5** | Diálogo de elevación JIT no aparece | Pop-ups bloqueados. | Permitir ventanas emergentes, volver a presionar botón. |

### 2.7 Celda 00 – Torre de Control
| ID | Síntoma en HUD | Causa Raíz (RCA) | Procedimiento de Corrección |
| :--- | :--- | :--- | :--- |
| **00‑E1** | Gateway "CAÍDO" pero Celda 01 activa | Torre ejecutada antes que Celda 01. | Presionar `⟳ Sincronizar Panóptico`. |
| **00‑E2** | Celdas en gris ("IDLE") | Celdas no ejecutadas en sesión. | Ejecutar celdas faltantes en orden y Sincronizar. |
| **00‑E3** | Línea de Tiempo vacía | Ledgers sin eventos. | Ejecutar celdas. Se poblará automáticamente. |
| **00‑E4** | Estado "DEGRADADO" | Alguna celda en naranja. | Identificar celda, aplicar solución de esta guía. |
| **00‑E5** | Refresco automático no funciona | Checkbox inactivo o daemon error. | 1. Marcar/desmarcar checkbox. 2. Usar refresco manual. |

---

## 📋 3. ÍNDICE RÁPIDO DE SÍNTOMAS

| Mensaje de Error / Síntoma | ID(s) | Celda |
| :--- | :--- | :--- |
| `Bloqueo Zero‑Trust: MISSING_GATEWAY` | 02‑E1, 04‑E1 | 02, 04 |
| `Bloqueo Zero‑Trust: ADC_REVOKED` | 02‑E2 | 02 |
| `IoC_FAIL: Celda 01 (Gateway) no detectada` | 03‑E1, 06‑E1 | 03, 06 |
| `IoC_FAIL: Celda 02 no ha generado checkpoint`| 03‑E2 | 03 |
| `IoC_FAIL: database_raw.parquet no encontrado`| 03‑E3 | 03 |
| `IoC_FAIL: database_silver.parquet...` | 04‑E2, 06‑E2 | 04, 06 |
| `PRESUPUESTO AGOTADO` | 02‑E4, 05‑E3 | 02, 05 |
| `BLOQUEO DE SEGURIDAD` | 05‑E1 | 05 |
| `CUOTA EXCEDIDA (FREE TIER)` | 05‑E2 | 05 |
| `FALLO DE INFRAESTRUCTURA` | 01‑E1, 01‑E2, 01‑E3| 01 |
| `FALLO DE TRANSFORMACIÓN` | 03‑E4 | 03 |
| `FALLO ANALÍTICO` | 04‑E3 | 04 |
| `FALLO EN REMEDIACIÓN` | 06‑E4 | 06 |
| `FALLO SISTÉMICO` | 05‑E4 | 05 |

---

## 📞 4. ESCALADO

Si una incidencia no puede resolverse con esta guía, se debe recopilar la siguiente información para el equipo de arquitectura:
1. `global_trace_id` de la sesión (visible en el HUD).
2. Archivo de ledger correspondiente (`*_audit_master_ledger.jsonl`).
3. `panopticon_evidence.json` generado por Celda 00.
4. Captura del HUD en el fallo.
5. Acciones realizadas previas.

> *"Un sistema sin un manual de diagnóstico es una caja negra. Un sistema con RCA documentado es un activo empresarial."*
