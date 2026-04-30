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
* **Invisibilidad Técnica:** Uso de `# @title { display-mode: "form" }` para colapsar código.
* **Zero‑Friction:** Auto-arranque. Botones solo para decisiones humanas (HitL).
* **Glassmorphism:** Tarjetas con efecto vidrio pulido.
```css
background: rgba(28, 28, 30, 0.6);
backdrop-filter: blur(30px);
border: 1px solid rgba(255, 255, 255, 0.05);
border-radius: 30px;

♿ 2. ACCESIBILIDAD Y PALETA (DEUTAN-FIRST)Estrictamente prohibido el uso de rojo/verde como canal único de información (Deuteranomalía). Estados codificados por color + texto + iconografía. Ratio >12:1 (AAA).2.1 Colores SemánticosEstadoColorHEXUso🔵 Operativo / ÉxitoAzul Eléctrico#007affSANO, ACTIVO, READY🟠 Riesgo / AlertaNaranja Vibrante#ff9500CAÍDO, advertencias🟣 CriptografíaPúrpura Apple#bf5af2KMS, túneles JIT⚪ InactivoGris Medio#8E8E93IDLE, etiquetas secundarias2.2 Theming Engine (3 Modos)ModoFondoTextoTarjetasEstadosHigh Contrast (Default)#ffffff#1d1d1f#f5f5f7Azul #007aff / Naranja #ff9500Deutan Safe#000000#ffffffrgba(28..)Azul #0A84FF / Ámbar #FF9F0AApple Pro#000000#ffffffrgba(28..)Verde #32D74B / Rojo #FF453A🔤 3. TIPOGRAFÍAInter: (wght: 400, 600, 700, 800). Para interfaz, títulos, valores.SF Mono: Para Trace ID, logs forenses, Dual-Clock.ElementoFamiliaPesoTamañoColorTítulo Celda (H2)Inter80024px#1d1d1fEstado / VersiónInter70014-20pxSemánticoKPI LabelInter8009-11px#555555KPI ValorInter70028-36px#1d1d1fTrace ID / LogsSF Mono40011px#8E8E93🧩 4. COMPONENTES DEL HUD4.1 Bento GridEstructura base de KPIs en cuadrícula con Glassmorphism y 30px de border-radius.Plaintext┌──────────────┬──────────────┬──────────────┬──────────────┐
│   KPI Card   │   KPI Card   │   KPI Card   │   KPI Card   │
│   Etiqueta   │   Etiqueta   │   Etiqueta   │   Etiqueta   │
│   Valor      │   Valor      │   Valor      │   Valor      │
└──────────────┴──────────────┴──────────────┴──────────────┘
4.2 Elementos TransversalesDual‑Clock: Hora local (AST 🇩🇴) y servidor (UTC ☁️) en esquina superior derecha.Píldora de Versión (.pill): Badge con borde de estado.Punto de Latido (.dot): Círculo de 8px con animation: pulse 1.5s infinite.Auditoría Colapsable: Elemento <details> conteniendo el ledger en SF Mono.Caja de Error (.error-box): Fondo naranja, borde sólido, RCA forense visible.🖼️ 5. REFERENCIA VISUAL: GATEWAY HUDPlaintext┌──────────────────────────────────────────────────────────────┐
│  ● ADAPTIVE GATEWAY V20         🇩🇴 19:05:58   ☁️ 23:05:58 │
│  ─────────────────────────────────────────────────────────── │
│   Master Gateway                                            │
│  28 ABRIL, 2026 | SISTEMA EN LÍNEA                           │
│  ████████████████████████████████████████████ 100%           │
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
