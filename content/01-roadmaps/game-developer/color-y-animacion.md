# Color y Animación

## 1. Color

### 1.1. Naturaleza del color

El color **no existe** en la naturaleza. Es una interpretación de nuestro cerebro de diferentes longitudes de onda de luz.

```mermaid
flowchart LR
    subgraph "Espectro electromagnético"
        G["Rayos Gamma"] --> X["Rayos X"]
        X --> UV["UV"]
        UV --> VIS["👁️ Visible (380-700nm)"]
        VIS --> IR["Infrarrojo"]
        IR --> M["Microondas"]
        M --> R["Radio"]
    end
```

**El ojo humano tiene 3 tipos de conos:**
- **S** (Short) → azul (~420nm)
- **M** (Medium) → verde (~534nm)
- **L** (Long) → rojo (~564nm)

Todo color que vemos es una combinación de la respuesta de estos 3 conos.

### 1.2. Modelos de color

```mermaid
flowchart TD
    COLOR[Modelos de Color] --> RGB["RGB<br/>Pantallas (aditivo)"]
    COLOR --> CMYK["CMYK<br/>Impresión (sustractivo)"]
    COLOR --> HSV["HSV/HSL<br/>Intuitivo para humanos"]
    COLOR --> YUV["YUV/YCbCr<br/>Video (luma + croma)"]
    COLOR --> CIE["CIE XYZ / Lab<br/>Perceptualmente uniforme"]
```

#### RGB (Aditivo)

Las pantallas **emiten** luz. Rojo + Verde + Azul = Blanco.

```mermaid
flowchart LR
    R["🔴 R (rojo)"] --> W["⚪ Blanco<br/>R+G+B"]
    G["🟢 G (verde)"] --> W
    B["🔵 B (azul)"] --> W
    
    R --> Y["🟡 Amarillo<br/>R+G"]
    G --> C["🟣 Cian<br/>G+B"]
    B --> M["🟠 Magenta<br/>R+B"]
```

#### HSV (Matiz, Saturación, Valor)

Más intuitivo para humanos: "quiero un rojo oscuro y apagado".

```
HSV:
  H (Hue)        = tono puro           (0° - 360°)
  S (Saturation) = intensidad del color (0% = gris, 100% = puro)
  V (Value)      = brillo              (0% = negro, 100% = máximo)
```

```mermaid
flowchart LR
    subgraph "Círculo cromático HSV"
        H0["0° Rojo"] --> H60["60° Amarillo"]
        H60 --> H120["120° Verde"]
        H120 --> H180["180° Cian"]
        H180 --> H240["240° Azul"]
        H240 --> H300["300° Magenta"]
        H300 --> H0
    end
```

```glsl
// RGB ↔ HSV
vec3 rgb2hsv(vec3 c) {
    float max = max(max(c.r, c.g), c.b);
    float min = min(min(c.r, c.g), c.b);
    float h = 0.0;
    
    if (max == c.r) h = 60.0 * (c.g - c.b) / (max - min);
    else if (max == c.g) h = 60.0 * (2.0 + (c.b - c.r) / (max - min));
    else h = 60.0 * (4.0 + (c.r - c.g) / (max - min));
    
    if (h < 0.0) h += 360.0;
    
    float s = (max == 0.0) ? 0.0 : (max - min) / max;
    return vec3(h, s, max);
}
```

#### YUV / YCbCr (Video)

Separa **luminancia** (brillo) de **crominancia** (color). Útil para compresión: el ojo nota más cambios de brillo que de color.

```
Y = 0.299·R + 0.587·G + 0.114·B    (luminancia)
U = -0.147·R - 0.289·G + 0.436·B   (azul - amarillo)
V = 0.615·R - 0.515·G - 0.100·B    (rojo - cian)
```

### 1.3. Espacio de color vs Gamut

```mermaid
flowchart TD
    GAMUT[Gamut = gama de colores representable] --> SRGB["sRGB<br/>Estándar web/gaming<br/>~35% CIE"]
    GAMUT --> DCI["DCI-P3<br/>Cine / HDR<br/>~45% CIE"]
    GAMUT --> REC2020["Rec. 2020<br/>UHDRTV<br/>~75% CIE"]
    GAMUT --> CIE["CIE Lab<br/>Todo el espectro<br/>100%"]
```

---

## 2. Percepción Visual

### 2.1. Cómo procesa el ojo

```mermaid
flowchart LR
    LUZ[Luz] --> CORNEA[Córnea] --> PUPILA[Pupila/Iris] --> CRISTAL[Cristalino] --> RETINA[Retina]
    RETINA --> CONOS["Conos<br/>Color (6M)"] --> BRAIN[Cerebro]
    RETINA --> BASTONES["Bastones<br/>Blanco/negro (120M)"]
```

**Conos** → visión fotópica (color, mucha luz)
**Bastones** → visión escotópica (monocromática, poca luz)

### 2.2. Fenómenos perceptuales clave

| Fenómeno | Descripción | Ejemplo |
|---|---|---|
| **Metamerismo** | 2 estímulos distintos se ven igual | Monitor vs objeto real |
| **Contraste simultáneo** | El mismo color se ve diferente según fondo | Gris sobre blanco vs negro |
| **Persistencia retiniana** | La imagen persiste ~1/24s | Base del cine (24fps) |
| **Ley de Weber-Fechner** | Percepción logarítmica, no lineal | Gamma correction |

### 2.3. Contraste simultáneo

```mermaid
flowchart LR
    subgraph "Contraste simultáneo"
        A["⬜ Fondo blanco<br/>⬛ Cuadrado gris<br/>(se ve más oscuro)"]
        B["⬛ Fondo negro<br/>⬛ Cuadrado gris<br/>(se ve más claro)"]
    end
```

### 2.4. Sensibilidad del ojo

El ojo es más sensible a:
1. **Movimiento** (visión periférica)
2. **Contraste** (diferencias de brillo)
3. **Verde** (más conos M que L o S)

```
Sensibilidad relativa:
Verde  (555nm) → 100%
Rojo   (610nm) → 50%
Azul   (450nm) → 5%
```

---

## 3. Reproducción de Tonos

### 3.1. Gamma

Las pantallas tienen una respuesta **no lineal**:

```
Luminancia_salida = Voltaje_entrada ^ gamma

gamma típica de pantalla: ~2.2
```

```mermaid
flowchart LR
    INPUT["Valor 0.5 (50%)"] --> SCREEN["Pantalla γ=2.2"]
    SCREEN --> OUTPUT["Brillo real: 0.5^2.2 ≈ 22%<br/>❌ No es la mitad de brillo"]
```

Para compensar, se aplica **gamma correction**:

```
Valor_correcto = valor_original ^ (1/2.2)
```

```
// Gamma correction en shader
vec3 color = texture(albedoMap, uv).rgb;
color = pow(color, vec3(1.0/2.2));  // lineal → sRGB
FragColor = vec4(color, 1.0);
```

### 3.2. Pipeline de color (Linear vs sRGB)

```mermaid
flowchart LR
    subgraph "Pipeline correcto"
        TEX["Textura sRGB"] --> LINEAR["Convertir a lineal<br/>(^2.2)"]
        LINEAR --> SHADER["Cálculos en espacio lineal"]
        SHADER --> CORRECT["Gamma correct<br/>(^1/2.2)"]
        CORRECT --> DISPLAY["Pantalla"]
    end
    
    subgraph "Pipeline incorrecto"
        TEX2["Textura sRGB"] --> SHADER2["Cálculos en sRGB ❌"]
        SHADER2 --> DISPLAY2["Pantalla<br/>Colores incorrectos"]
    end
```

**Regla:** Todos los cálculos de iluminación deben hacerse en **espacio lineal**. La corrección gamma se aplica al final.

### 3.3. HDR (High Dynamic Range)

```mermaid
flowchart TD
    subgraph "Rango dinámico"
        SDR["SDR<br/>0-255 (8 bits)<br/>~100 nits"]
        HDR["HDR10<br/>0-1023 (10 bits)<br/>~1000 nits"]
        DOLBY["Dolby Vision<br/>12 bits<br/>~10000 nits"]
    end
```

**HDR en juegos:**
1. Renderizar en espacio HDR (float16)
2. Tone mapping (mapear HDR → SDR para la pantalla)
3. Output

```
// Tone mapping simple (Reinhard)
vec3 tonemap(vec3 hdrColor) {
    return hdrColor / (hdrColor + vec3(1.0));
}

// Tone mapping ACES (estándar cinematográfico)
vec3 tonemap_ACES(vec3 x) {
    float a = 2.51;
    float b = 0.03;
    float c = 2.43;
    float d = 0.59;
    float e = 0.14;
    return clamp((x * (a * x + b)) / (x * (c * x + d) + e), 0.0, 1.0);
}
```

---

## 4. Animación por Computadora

### 4.1. Principios de animación (Disney)

Originalmente 12 principios desarrollados por Ollie Johnston y Frank Thomas (1989). Adaptados a 3D:

```mermaid
flowchart TD
    ANIM[12 Principios] --> S["1. Squash & Stretch<br/>Estirar/comprimir para dar peso"]
    ANIM --> A["2. Anticipation<br/>Preparación antes de la acción"]
    ANIM --> ST["3. Staging<br/>Puesta en escena clara"]
    ANIM --> SA["4. Straight Ahead / Pose to Pose"]
    ANIM --> FO["5. Follow Through & Overlap<br/>Partes que siguen al cuerpo"]
    ANIM --> EA["6. Slow In & Out<br/>Acelerar y frenar suave"]
    ANIM --> AR["7. Arcs<br/>Movimientos curvos, no rectos"]
    ANIM --> SE["8. Secondary Action<br/>Acciones que complementan"]
    ANIM --> TI["9. Timing<br/>Número de frames = peso/emoción"]
    ANIM --> EX["10. Exaggeration<br/>Exagerar para comunicar"]
    ANIM --> SP["11. Solid Drawing<br/>Volumen y peso 3D"]
    ANIM --> AP["12. Appeal<br/>Atractivo visual del personaje"]
```

### 4.2. Técnicas de animación

```mermaid
flowchart LR
    subgraph "Animación en juegos"
        SK["Esqueletal (Skinning)<br/>Huesos + Piel"] --> ANIM
        VERT["Por vértice<br/>Morph targets / Blend shapes"] --> ANIM
        PROC["Procedural<br/>IK, física, ragdoll"] --> ANIM
        ANIM["Animación final"]
    end
```

#### Animación Esqueletal

```mermaid
flowchart LR
    ROOT["Hueso Raíz<br/>(pelvis)"] --> SPINE["Columna"]
    SPINE --> HEAD["Cabeza"]
    SPINE --> L_ARM["Brazo Izq"]
    L_ARM --> L_FORE["Antebrazo Izq"]
    L_FORE --> L_HAND["Mano Izq"]
    SPINE --> R_ARM["Brazo Der"]
    SPINE --> L_LEG["Pierna Izq"]
    L_LEG --> L_FOOT["Pie Izq"]
    SPINE --> R_LEG["Pierna Der"]
```

**Matriz final de cada vértice:**

```
Matriz_Vertex = Σ (peso_i × Matriz_Hueso_i × BindPose_i⁻¹)
```

Donde `BindPose` es la pose de referencia (T-pose).

### 4.3. Blending de animaciones

```mermaid
flowchart LR
    subgraph "Blend Tree"
        IDLE["Idle"] --> BLEND["Blend Tree"]
        WALK["Walk"] --> BLEND
        RUN["Run"] --> BLEND
        BLEND --> OUT["Output<br/>(según velocidad)"]
    end
```

```
// Blend linear entre idle y walk
float speed = player.velocity.length();
float t = clamp(speed / maxSpeed, 0.0, 1.0);

Vec3 finalPos = lerp(idlePos, walkPos, t);
Quat finalRot = slerp(idleRot, walkRot, t);
```

### 4.4. Inverse Kinematics (IK)

**FK (Forward Kinematics):** Rotas el hombro → se mueve la mano.
**IK (Inverse Kinematics):** Pones la mano donde quieres → se calculan las rotaciones.

```mermaid
flowchart LR
    subgraph "FK: Hombro→Codo→Mano"
        HOM["Hombro (roto)"] --> CODO["Codo (roto)"]
        CODO --> MANO["Mano (resultado)"]
    end
    subgraph "IK: Mano→Hombro"
        MANO2["Mano (posición deseada)"] --> CODO2["Codo (calculado)"]
        CODO2 --> HOM2["Hombro (calculado)"]
    end
```

**Usos:** Pies en el suelo, manos en objetos, cabezas mirando al objetivo.

### 4.5. Curvas de animación (F-curves)

Cada hueso tiene curvas para cada eje de transformación:

```
Hueso: Hombro_Izq
  ├── translate.x : [curva 1]
  ├── translate.y : [curva 2]
  ├── translate.z : [curva 3]
  ├── rotate.x    : [curva 4]
  ├── rotate.y    : [curva 5]
  ├── rotate.z    : [curva 6]
  └── rotate.w    : [curva 7] (cuaternión)
```

```mermaid
flowchart LR
    subgraph "F-curve (posición Y de un salto)"
        T0["t=0<br/>suelo"] --> T1["t=0.2<br/>sube"]
        T1 --> T2["t=0.5<br/>pico"] --> T3["t=0.8<br/>baja"]
        T3 --> T4["t=1.0<br/>suelo"]
    end
```

### 4.6. State Machine de Animación

```mermaid
flowchart TD
    IDLE["Idle"] -->|"velocidad > 0.1"| WALK["Walk"]
    WALK -->|"velocidad < 0.1"| IDLE
    WALK -->|"shift presionado"| RUN["Run"]
    RUN -->|"shift suelto"| WALK
    IDLE -->|"espacio"| JUMP["Jump"]
    JUMP -->|"en el suelo"| IDLE
    IDLE -->|"golpe"| HIT["Hit"]
    HIT -->|"termina"| IDLE
```

### 4.7. Compresión de animaciones

Las animaciones en disco pueden ser enormes. Técnicas de compresión:

| Técnica | Ratio | Calidad |
|---|---|---|
| **Wavelet compresión** | 10:1 | Alta |
| **Cuaterniones de 16 bits** | 2:1 | Alta |
| **Keyframe reduction** | Variable | Ajustable |
| **Animation retargeting** | N/A | Reutiliza animaciones |

> 💡 **Reglas de oro:**
> - **Color:** Siempre cálculos en espacio lineal; gamma correction al final
> - **Percepción:** El ojo no es lineal → aprovecha gamma, usa dithering en bandas
> - **Animación:** Los 12 principios de Disney funcionan TAMBIÉN en 3D. No los ignores
> - **IK vs FK:** FK para animaciones predefinidas, IK para adaptación al entorno

## Relacionados:
- [[iluminación-y-sombras]] #anterior 
- [[visivilidad-y-oclusion]] #extra 
- [[mapping-reflexion]] #extra 
- [[shader]] #siguiente 