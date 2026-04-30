# 📄 ECOSISTEMA OMNIVERSAL V20.0‑PRO
**Manual de Marca y Experiencia de Usuario (UX & Accessibility Guide)**

| Metadato | Valor |
| :--- | :--- |
| **Documento** | OMNI‑UX‑GUIDE V20.0‑PRO |
| **Versión** | 1.0.0 |
| **Clasificación** | Confidencial – Uso Interno / Equipo de Diseño y Desarrollo |
| **Autor** | Rubintel Ulloa Francisco (Enterprise Software Architect) |
| **Fecha** | Junio 2026 |

**Propósito:** Documentar la filosofía de diseño, paleta de colores, tipografías, componentes del HUD y reglas de accesibilidad cognitiva (WCAG 2.1 AAA) del ecosistema.

---

## 🎨 1. FILOSOFÍA DE DISEÑO

* **Apple Silicon UX:** Superficies translúcidas, jerarquía tipográfica clara, animaciones sutiles y alto contraste.
* **Invisibilidad Técnica:** Uso de `# @title { display-mode: "form" }` para colapsar código fuente en Google Colab.
* **Zero‑Friction:** Auto-arranque tras la ejecución de la celda. Botones reservados exclusivamente para decisiones humanas críticas (HitL).
* **Glassmorphism:** Tarjetas con efecto de vidrio pulido para profundidad visual.

```css
/* Especificación de estilo para contenedores HUD */
background: rgba(28, 28, 30, 0.6);
backdrop-filter: blur(30px);
border: 1px solid rgba(255, 255, 255, 0.05);
border-radius: 30px;

♿ 2. ACCESIBILIDAD Y PALETA (DEUTAN-FIRST)Se prohíbe estrictamente el uso de rojo/verde como canal único de información para garantizar la compatibilidad con usuarios con Deuteranomalía. Todos los estados se codifican mediante la tríada: Color + Texto + Iconografía. El ratio de contraste se mantiene >12:1 (Cumplimiento AAA).2.1 Colores SemánticosEstadoColorHEXUso🔵 Operativo / ÉxitoAzul Eléctrico#007affEstados SANO, ACTIVO, READY🟠 Riesgo / AlertaNaranja Vibrante#ff9500Estados CAÍDO, advertencias, errores🟣 CriptografíaPúrpura Apple#bf5af2Indicadores de KMS, túneles JIT elevados⚪ Inactivo / NeutroGris Medio#8E8E93Estados IDLE, PENDIENTE, etiquetas secundarias2.2 Theming Engine (3 Modos de Visualización)ModoFondoTextoTarjetasEstadosHigh Contrast (Default)#ffffff#1d1d1f#f5f5f7Azul #007aff / Naranja #ff9500Deutan Safe#000000#ffffffrgba(28, 28, 30, 0.6)Azul #0A84FF / Ámbar #FF9F0AApple Pro#000000#ffffffrgba(28, 28, 30, 0.6)Verde #32D74B / Rojo #FF453A🔤 3. TIPOGRAFÍAEl ecosistema utiliza fuentes sans-serif modernas para optimizar la lectura en pantallas de alta densidad.Inter: Pesos (400, 600, 700, 800). Utilizada para la interfaz principal, títulos y valores de KPI.SF Mono: Utilizada para Trace ID, logs forenses, Dual-Clock y bloques de código.ElementoFamiliaPesoTamañoColorTítulo Celda (H2)Inter80024px#1d1d1fEstado / VersiónInter70014-20pxSemántico (Azul/Naranja)KPI LabelInter8009-11px#555555KPI ValorInter70028-36px#1d1d1fTrace ID / LogsSF Mono40011px#8E8E93🧩 4. COMPONENTES DEL HUD4.1 Bento GridEstructura base de KPIs organizada en una cuadrícula de tarjetas con bordes suavizados.Plaintext┌──────────────┬──────────────┬──────────────┬──────────────┐
│   KPI Card   │   KPI Card   │   KPI Card   │   KPI Card   │
│   Etiqueta   │   Etiqueta   │   Etiqueta   │   Etiqueta   │
│   Valor      │   Valor      │   Valor      │   Valor      │
└──────────────┴──────────────┴──────────────┴──────────────┘
4.2 Elementos TransversalesDual‑Clock: Muestra simultáneamente la hora local (AST 🇩🇴) y la del servidor (UTC ☁️) para correlación de logs.Píldora de Versión (.pill): Badge de identificación con punto de latido animado.Punto de Latido (.dot): Indicador visual de ejecución activa (animation: pulse 1.5s infinite).Auditoría Colapsable: Elemento <details> que contiene el flujo de eventos en formato JSONL.Caja de Error (.error-box): Contenedor de alta visibilidad para reportar causas raíz (RCA) de fallos.🖼️ 5. REFERENCIA VISUAL: GATEWAY HUDPlaintext┌──────────────────────────────────────────────────────────────┐
│  ● ADAPTIVE GATEWAY V20         🇩🇴 19:05:58   ☁️ 23:05:58 │
│  ─────────────────────────────────────────────────────────── │
│   Master Gateway                                            │
│  28 ABRIL, 2026 | SISTEMA EN LÍNEA                           │
│  ██████████████████─────────────────────────── 40%           │
│                                                              │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐         │
│  │   FUSE   │ │ CPU/RAM  │ │ VOLUMEN  │ │  DAMA    │         │
│  │ Latencia │ │ 12%/34%  │ │ 15.2 GB  │ │  100%    │         │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘         │
│                                                              │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐         │
│  │Seguridad │ │Evidencia │ │  Token   │ │Emergency │         │
│  │readonly  │ │   SÍ     │ │  58 min  │ │   NO     │         │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘         │
│                                                              │
│  ︾ Desplegar Auditoría Forense (.jsonl)                      │
│  ─────────────────────────────────────────────────────────── │
│  [23:05:58] G01.2: Evidencia de Readiness persistida.        │
│  [23:05:38] F02.1: Handshake ADC (drive.readonly)...         │
│                                                              │
│  🔗 TRACE ID: b89240cc‑1c32‑4719‑b0a8‑06239e62431f          │
└──────────────────────────────────────────────────────────────┘
"La accesibilidad no es una restricción, sino la máxima expresión de la ingeniería de interfaces. Un diseño que funciona para todos es un diseño que funciona para siempre."
