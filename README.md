# 🥁 PULSE — Caja de Ritmos Interactiva

Un secuenciador de batería web interactivo con síntesis de audio pura, visualizador de espectro radial y patrones predefinidos. Todo en un único archivo HTML — sin dependencias de compilación, sin backend, sin samples externos.

## 🎧 Demo

Abre `index.html` directamente en tu navegador moderno (Chrome, Firefox, Edge o Safari).

## ✨ Características

| Función | Descripción |
|---|---|
| **Secuenciador 16 pasos** | Grid interactivo de 8 pistas × 16 pasos con activación normal (click) y acento (click derecho). |
| **Síntesis pura** | Todos los sonidos se generan en tiempo real con la **Web Audio API** — osciladores, filtros y ruido blanco. Cero samples. |
| **8 instrumentos** | Kick, Snare, Clap, Hi-Hat cerrado, Hi-Hat abierto, Tom, Cymbal, Percusión. Cada uno con color propio. |
| **8 patrones** | Deep House, Techno, Boom Bap, Trap, Drum & Bass, Funk, Rock y Vacío. Cargables con un click. |
| **Control de tempo** | BPM ajustable (60–200) con slider y teclas ↑/↓. Control de Swing (0–60). |
| **Mezclador por pista** | Volumen individual, botón **Mute** (M) y **Solo** (S) por canal. |
| **Master** | Volumen general y Reverb (convolver con impulse response sintetizado). |
| **Visualizador radial** | Espectro de frecuencia animado en canvas con barras radiales y círculo pulsante central. |
| **Partículas** | Efecto de partículas visuales al disparar cada golpe. |
| **Guardar / Cargar** | Persistencia de patrones personalizados vía `localStorage`. |
| **Atajos de teclado** | Controles rápidos sin tocar el ratón. |
| **Responsive** | Layout adaptable: 3 columnas en escritorio, panel de mezclador oculto en pantallas < 900px. |

## 🎹 Atajos de Teclado

| Tecla | Acción |
|---|---|
| `Space` | Reproducir / Pausar |
| `C` | Limpiar patrón |
| `R` | Generar patrón aleatorio |
| `1`–`8` | Toggle Mute de la pista correspondiente |
| `↑` / `↓` | Subir / Bajar BPM de 1 en 1 |
| Click izquierdo en celda | Activar / Desactivar paso |
| Click derecho en celda | Activar / Desactivar acento del paso |

## 🏗️ Arquitectura

```
index.html   ← Archivo único autocontenido
├── <style>  ← Estilos CSS (custom properties, grid layout, animaciones)
├── <body>   ← Estructura HTML semántica
│   ├── Header      (Logo, BPM, Swing, Transport)
│   ├── Mixer       (Pane izquierdo — controles por pista)
│   ├── Sequencer   (Centro — grid 8×16 + beam + partículas)
│   ├── Visual      (Panel derecho — visualizador, patrones, master)
│   └── Footer      (Atajos de teclado)
└── <script> ← Lógica JavaScript
    ├── Configuración (TRACKS_CONFIG, PATTERNS, estado global)
    ├── Audio Engine  (Web Audio API routing, convolver, compressor)
    ├── Síntesis      (8 funciones de síntesis de instrumentos)
    ├── Secuenciador  (Scheduler con lookahead, swing)
    ├── Visualización (Canvas: espectro radial + partículas)
    └── UI            (Grid, mixer, patrones, event handlers)
```

### Cadena de Audio

```
Sintetizadores → masterGain → [dryGain ──────────────┐
                                  → reverbNode → reverbGain ──┤
                                                          ├→ masterCompressor → analyser → destination (altavoces)
```

### Instrumentos — Método de Síntesis

| Instrumento | Técnica |
|---|---|
| **Kick** | Oscilador sine con pitch sweep 160→40 Hz + click triangle a 1200 Hz |
| **Snare** | Ruido filtrado (highpass 1500 Hz) + oscilador triangle 220→110 Hz |
| **Clap** | 3 ráfagas de ruido espaciadas 12 ms, filtradas por bandpass 1200 Hz |
| **Hi-Hat Cerrado** | Ruido highpass 7000 + bandpass 10000, decay 60 ms |
| **Hi-Hat Abierto** | Igual que cerrado, decay 300 ms |
| **Tom** | Oscilador sine con pitch sweep 220→90 Hz |
| **Cymbal** | Ruido highpass 5000 + bandpass 8000, decay 600 ms |
| **Perc** | Oscilador square 880→220 Hz, bandpass 1500 Hz, Q=2 |

## 🛠️ Tecnologías

| Tecnología | Uso |
|---|---|
| **HTML5 / CSS3** | Estructura y estilos (CSS custom properties, CSS Grid, animaciones) |
| **JavaScript (ES6+)** | Toda la lógica de audio, UI e interactividad |
| **Web Audio API** | Síntesis de sonido, efectos (reverb, compresión), análisis de espectro |
| **Canvas 2D** | Visualizador radial y sistema de partículas |
| **Tailwind CSS** | CDN — utilidades auxiliares (no crítico, el layout es CSS custom) |
| **Google Fonts** | Bricolage Grotesque, JetBrains Mono, Space Grotesk |

## 📂 Estructura del Proyecto

```
BEAT-BOX/
├── index.html    ← Aplicación completa (HTML + CSS + JS)
└── README.md     ← Esta documentación
```

## 🚀 Uso

No requiere instalación. Simplemente:

```bash
# Clonar el repositorio
git clone https://github.com/jorsenc/BEAT-BOX.git
cd BEAT-BOX

# Abrir en el navegador
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

O servirlo con cualquier servidor estático:

```bash
npx serve .
python -m http.server 8000
```

## 📋 Patrones Incluidos

| Patrón | BPM | Estilo |
|---|---|---|
| Vacío | 120 | Grid limpio para composición libre |
| Deep House | 124 | Kick en cuatro, clap en off-beat, hi-hats sincopados |
| Techno | 130 | Kick constante, hi-hats rápidos, percusión rítmica |
| Boom Bap | 90 | Breakbeat clásico hip-hop con swing natural |
| Trap | 140 | Kick con silencios, hi-hats en semicorchea, clap en 2 y 4 |
| Drum & Bass | 174 | Ritmo rápido, kick en 1 y 11, snare en 5 y 13 |
| Funk | 110 | Síncopa en el kick, hi-hats regulares, percusión variada |
| Rock | 120 | Patrón de rock estándar con crash en el 1 |

## 📄 Licencia

Este proyecto es de uso libre. Créalo, modídicalo, compártelo.
