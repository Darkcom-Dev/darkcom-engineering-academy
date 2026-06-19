# Computación Gráfica: Iluminación y Sombras

## ¿Por qué importa la iluminación?

Sin iluminación, un modelo 3D es solo un montón de triángulos planos. La luz es lo que da **volumen, profundidad y realismo** a la escena.

```mermaid
flowchart LR
    subgraph "Sin luz"
        A["⬜ Modelo plano<br/>Sin volumen<br/>Sin atmósfera"]
    end
    subgraph "Con luz"
        B["✨ Modelo con volumen<br/>Sombras, profundidad<br/>Atmósfera y realismo"]
    end
```

### Componentes de la iluminación

```mermaid
flowchart TD
    LUZ[Iluminación] --> AMB[Ambient<br/>Luz base uniforme]
    LUZ --> DIF[Difusa<br/>Superficie mate]
    LUZ --> SPEC[Especular<br/>Brillos / reflejos]
    LUZ --> EMIT[Emisiva<br/>Objetos que brillan]
```

---

## 1. Modelos de iluminación

### 1.1. Iluminación por vértice (Gouraud)

Se calcula la luz en cada **vértice** y se interpola entre ellos.

```
Color(vértice) = Ambient + Difusa + Especular
                      ↓
         Se interpola linealmente entre vértices
                      ↓
              Color final del píxel
```

**Problema:** Los highlights especulares se ven mal si caen en medio de un triángulo.

### 1.2. Iluminación por píxel (Phong)

Se calcula la luz en **cada píxel** interpolando las normales. Es el estándar moderno.

```mermaid
flowchart LR
    subgraph "Gouraud (vértice)"
        V1["Vértice→luz"] --> T1["Interpolar color"]
        T1 --> P1["Píxel: color interpolado"]
    end
    subgraph "Phong (píxel)"
        V2["Vértice→normal"] --> T2["Interpolar normal"]
        T2 --> P2["Píxel: calcular luz con normal interpolada"]
    end
```

### 1.3. Fórmula de iluminación de Phong

```
Iluminación = Ambient + Difusa + Especular

Ambient = k_a * luz_ambient
Difusa  = k_d * luz * max(0, N·L)
Especular = k_s * luz * max(0, R·V)^α

donde:
N = normal de la superficie
L = dirección a la luz
R = reflexión de L respecto a N (R = 2(N·L)N - L)
V = dirección a la cámara
α = brillo especular (shininess)
```

```mermaid
flowchart TD
    subgraph "Vectores en Phong"
        SURF[Superficie] --> N["N (Normal)"]
        LUZ_F["Fuente de luz"] --> L["L (Dirección luz)"]
        CAM["Cámara"] --> V["V (Dirección vista)"]
        L --> R["R (Reflexión)<br/>= 2(N·L)N - L"]
        N --> R
    end
```

```
// Shader de fragmento (GLSL) - Phong básico
uniform vec3 lightPos;
uniform vec3 viewPos;
uniform vec3 lightColor;
uniform vec3 objectColor;

in vec3 FragPos;   // Posición del fragmento (interpolada)
in vec3 Normal;    // Normal (interpolada)

void main() {
    // Ambient
    float ambientStrength = 0.1;
    vec3 ambient = ambientStrength * lightColor;

    // Difusa
    vec3 norm = normalize(Normal);
    vec3 lightDir = normalize(lightPos - FragPos);
    float diff = max(dot(norm, lightDir), 0.0);
    vec3 diffuse = diff * lightColor;

    // Especular
    float specularStrength = 0.5;
    vec3 viewDir = normalize(viewPos - FragPos);
    vec3 reflectDir = reflect(-lightDir, norm);
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), 32);
    vec3 specular = specularStrength * spec * lightColor;

    vec3 result = (ambient + diffuse + specular) * objectColor;
    FragColor = vec4(result, 1.0);
}
```

---

## 2. Fuentes de luz

```mermaid
flowchart TD
    LUZ[Fuentes de luz] --> DIR[Direccional ☀️]
    LUZ --> PUN[Puntual 💡]
    LUZ --> SPOT[Spot 🔦]
    LUZ --> AREA[Área 🪟]
    LUZ --> INF[Infinito 🌌]
```

### 2.1. Direccional (Sun light)

Simula el sol: **rayos paralelos** desde el infinito. Todos los rayos van en la misma dirección.

```
Posición: (infinito)
Dirección: fija (ej: (-1, -3, -2))
Atenuación: ninguna (no se debilita con distancia)
Sombras: sí (mapa de sombras ortogonal)
```

```mermaid
flowchart LR
    SUN["☀️ Sol (infinito)"] --> R1["→ rayo"]
    SUN --> R2["→ rayo"]
    SUN --> R3["→ rayo"]
    R1 --> OBJ["Objeto<br/>Misma sombra<br/>en toda la escena"]
    R2 --> OBJ
    R3 --> OBJ
```

### 2.2. Puntual (Point light)

Simula una **bombilla**: luz desde un punto en todas direcciones.

```
Posición: (x, y, z)
Dirección: todas
Atenuación: 1 / (d²) (se debilita con distancia)
Sombras: sí (mapa de cubo)
```

```mermaid
flowchart LR
    P["💡 Puntual (x, y, z)"] --> D1["↗️ rayo"]
    P --> D2["↖️ rayo"]
    P --> D3["↙️ rayo"]
    P --> D4["↘️ rayo"]
    P --> D5["⬆️ rayo"]
    P --> D6["⬇️ rayo"]
```

**Fórmula de atenuación:**

```
Atenuación = 1.0 / (Kc + Kl·d + Kq·d²)

Kc = constante (1.0)
Kl = lineal (0.09)
Kq = cuadrática (0.032)
d = distancia a la luz
```

### 2.3. Spot (Spotlight)

Simula un **foco**: luz en forma de cono.

```
Posición: (x, y, z)
Dirección: (dx, dy, dz)
Ángulo interno (θ): zona de luz intensa
Ángulo externo (φ): zona de desvanecimiento
Atenuación: sí (distancia + ángulo)
Sombras: sí (mapa de sombras en perspectiva)
```

```mermaid
flowchart TD
    SP["🔦 Spot"] --> CONE["⛔ Cono interno (θ)<br/>Luz al 100%"]
    CONE --> FALLOFF["⛅ Cono externo (φ)<br/>Atenuación suave"]
    FALLOFF --> OUT["🌑 Fuera del cono<br/>Sin luz"]
```

```
// Atenuación por ángulo del spot
float theta = dot(lightDir, normalize(spotDirection));
float epsilon = innerCone - outerCone;
float intensity = clamp((theta - outerCone) / epsilon, 0.0, 1.0);
```

### 2.4. Infinito (Ambient / Infinite light)

Luz que viene de **todas partes**. No tiene origen ni dirección.

```mermaid
flowchart LR
    SKY["🌌 Bóveda celeste / Skybox"] --> AMB["Luz ambiental uniforme"]
    SKY --> IBL["IBL (Image Based Lighting)<br/>Luz según el entorno"]
```

**Tipos:**
- **Ambient plana:** mismo color en toda la escena (barato, poco realista)
- **IBL (Image Based Lighting):** usa un HDR environment map para iluminar (realista, caro)
- **GI (Global Illumination):** simula rebotes de luz (Lightmass, Enlighten, Voxel GI)

---

## 3. Mapa de Sombras (Shadow Mapping)

### 3.1. Concepto general

El **shadow mapping** es la técnica de sombras más usada en tiempo real. Consiste en dos pasos:

```mermaid
flowchart LR
    subgraph "Paso 1: Render desde la luz"
        R1["Renderizar profundidad<br/>desde perspectiva de la luz"]
        R1 --> SM["Shadow Map<br/>(textura de profundidad)"]
    end
    subgraph "Paso 2: Render desde la cámara"
        CAM["Renderizar escena normal"] --> COMP
        SM --> COMP["Comparar: ¿píxel está<br/>más lejos que el shadow map?"]
        COMP -->|"Sí → en sombra"| DARK["⬛ Oscuro"]
        COMP -->|"No → iluminado"| LIT["☀️ Iluminado"]
    end
```

```
// Fragment shader - prueba de sombra
float shadowCalculation(vec4 fragPosLightSpace) {
    vec3 projCoords = fragPosLightSpace.xyz / fragPosLightSpace.w;
    projCoords = projCoords * 0.5 + 0.5;  // [-1,1] → [0,1]
    
    float closestDepth = texture(shadowMap, projCoords.xy).r;
    float currentDepth = projCoords.z;
    
    float bias = 0.005;  // Evitar shadow acne
    return currentDepth - bias > closestDepth ? 1.0 : 0.0;
}
```

### 3.2. Shadow Acne y Peter Panning

```mermaid
flowchart LR
    subgraph "Problemas comunes"
        SA["Shadow Acne<br/>Rayas/artefactos<br/>→ solución: bias"] 
        PP["Peter Panning<br/>Sombra despegada<br/>→ solución: bias muy alto"]
    end
```

### 3.3. Tipos de Shadow Map

#### Mapa de Sombras 2D (estándar)

Para **luces direccionales**. Un solo shadow map ortogonal.

```
Resolución típica: 1024×1024 a 4096×4096
Proyección: Ortogonal (rayos paralelos)
Problema: Una sola vista → sombras de baja calidad lejos
```

#### Mapa de Cubo (Cube Shadow Map)

Para **luces puntuales**. Se renderiza la escena 6 veces (una por cara del cubo).

```mermaid
flowchart TD
    POINT["💡 Luz puntual"] --> CUBE["📦 Mapa de cubo"]
    CUBE --> XP["+X"]
    CUBE --> XN["-X"]
    CUBE --> YP["+Y"]
    CUBE --> YN["-Y"]
    CUBE --> ZP["+Z"]
    CUBE --> ZN["-Z"]
```

```
// Cube shadow map en el fragment shader
float depth = texture(shadowCubeMap, fragToLight).r;
// fragToLight = vector desde el fragmento a la luz
// depth = distancia al primer obstáculo en esa dirección
```

#### Mapas en Cascada (CSM - Cascaded Shadow Maps)

Para **luces direccionales en escenas grandes**. Divide el frustum en cascadas, cada una con su propio shadow map.

```mermaid
flowchart TD
    CAM[Cámara] --> FRUSTUM[Frustum de visión]
    FRUSTUM --> C1["Cascada 1 (cerca)<br/>Shadow Map 2048×2048"]
    FRUSTUM --> C2["Cascada 2 (media)<br/>Shadow Map 1024×1024"]
    FRUSTUM --> C3["Cascada 3 (lejos)<br/>Shadow Map 512×512"]
```

**Ventaja:** Alta calidad cerca de la cámara, menor calidad donde no se nota.

```
// CSM - seleccionar cascada según profundidad
int getCascadeIndex(float depth) {
    if (depth < cascadePlanes[0]) return 0;
    if (depth < cascadePlanes[1]) return 1;
    return 2;
}

float shadow = 0.0;
int cascade = getCascadeIndex(depth);
shadow = calculateShadow(cascade, fragPosLightSpace[cascade]);
```

---

## 4. Stencil Shadow (Volumes)

Técnica anterior al shadow mapping (Doom 3, 2004). Usa el **stencil buffer** para recortar las áreas en sombra.

```mermaid
flowchart LR
    subgraph "Stencil Shadow Volume"
        OBJ["Objeto"] --> SV["Volumen de sombra<br/>(extrusión del modelo<br/>desde la luz)"]
        SV --> STENCIL["Stencil buffer<br/>cuenta pasos frontera"]
        STENCIL --> MASK["Máscara: píxeles<br/>dentro del volumen = sombra"]
    end
```

**Proceso:**
1. Renderizar escena sin luz (solo ambiente)
2. Renderizar volúmenes de sombra al stencil buffer
3. Renderizar escena iluminada donde **stencil = par**

| Técnica | Shadow Mapping | Stencil Shadows |
|---|---|---|
| Rendimiento | Escala con resolución | Escala con geometría |
| Calidad | Depende de resolución | Perfecta (píxel exacto) |
| Transparencia | Fácil | Difícil |
| Uso moderno | Estándar actual | Obsoleto |

---

## 5. Comparativa de técnicas de sombra

```mermaid
flowchart TD
    SOMBRA[Técnicas de sombra] --> RT["Ray Tracing<br/>(hardware RTX)"]
    SOMBRA --> SM["Shadow Mapping<br/>(estándar)"]
    SOMBRA --> SV["Stencil Volumes<br/>(obsoleto)"]
    SOMBRA --> CONTACT["Contact Shadows<br/>(SSAO-like)"]
    
    SM --> CASCADE["CSM<br/>Grandes escenas"]
    SM --> CUBE["Cube Map<br/>Luces puntuales"]
    SM --> PCF["PCF<br/>Suavizado<br/>Percentage Closer Filtering"]
```

| Técnica | Calidad | Rendimiento | Complejidad |
|---|---|---|---|
| Shadow Map 2D | ⭐⭐⭐ | ⭐⭐⭐⭐ | Baja |
| CSM | ⭐⭐⭐⭐ | ⭐⭐⭐ | Media |
| Cube Shadow | ⭐⭐⭐ | ⭐⭐⭐ | Media |
| PCF (suavizado) | ⭐⭐⭐⭐ | ⭐⭐⭐ | Media |
| Ray Tracing | ⭐⭐⭐⭐⭐ | ⭐ (caro) | Alta |
| Stencil Volume | ⭐⭐⭐⭐ | ⭐⭐ (geometría) | Alta |

---

## 6. Pipeline completo de iluminación

```mermaid
flowchart TD
    subgraph "Forward Rendering"
        GEOM["Geometría"] --> SHADE["Sombrear cada objeto<br/>con cada luz"]
        SHADE --> OUT["Frame final"]
    end
    
    subgraph "Deferred Rendering (estándar moderno)"
        G1["G-Buffer Pass"] --> GBUF["G-Buffer<br/>Posición, Normal, Color,<br/>Roughness, Metalness"]
        GBUF --> LIGHT["Lighting Pass<br/>Calcular luz para<br/>cada píxel"]
        LIGHT --> OUT2["Frame final"]
    end
```

**Deferred Rendering** permite tener **muchas luces** sin penalización.

```
Forward:  Luces × Objetos visibles
Deferred: Luces × Píxeles en pantalla (independiente de objetos)
```

> 💡 **Regla de oro:** Shadow mapping es el estándar, CSM para mundos abiertos, cube maps para luces puntuales, y Ray Tracing para el futuro (cuando el hardware lo permita).

## Relacionados:
- [[lenguajes-de-programación-para-videojuegos]] #anterior 
- [[color-y-animacion]] #siguiente 