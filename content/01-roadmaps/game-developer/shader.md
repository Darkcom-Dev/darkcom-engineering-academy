# Shader

## ¿Qué es un shader?

Un **shader** es un pequeño programa que se ejecuta en la **GPU** (Graphics Processing Unit). Es el corazón de la computación gráfica moderna.

```mermaid
flowchart LR
    CPU["CPU<br/>Lógica del juego<br/>Física, IA"] --> GPU["GPU<br/>Miles de núcleos<br/>Ejecuta shaders"]
    GPU --> SHADERS["Shaders<br/>Programas en GPU"]
    SHADERS --> PIXEL["Píxeles en pantalla"]
```

### Tipos de shaders

```mermaid
flowchart TD
    SHADERS[Shaders] --> VS[Vertex Shader<br/>Procesa vértices]
    SHADERS --> FS[Fragment Shader<br/>Procesa píxeles]
    SHADERS --> GS[Geometry Shader<br/>Genera/elimina geometría]
    SHADERS --> CS[Compute Shader<br/>Cómputo general GPU]
    SHADERS --> TS[Tessellation Shader<br/>Subdivide geometría]
    SHADERS --> MS[Mesh Shader<br/>Moderno, reemplaza VS+GS]
```

### Lenguajes de shaders

```
HLSL   → DirectX (Windows/Xbox)
GLSL   → OpenGL, Vulkan (multiplataforma)
MSL    → Metal (Apple)
SPIR-V → Formato intermedio (Vulkan)
```

---

## 1. Tubería de Gráficos (Graphics Pipeline)

La **pipeline gráfica** es el proceso que sigue un modelo 3D hasta convertirse en píxeles en pantalla.

```mermaid
flowchart LR
    subgraph "✅ Programable por shaders"
        VS2[Vertex Shader] --> TS2[Tessellation]
        TS2 --> GS2[Geometry Shader]
        GS2 --> FS2[Fragment Shader]
    end
    
    subgraph "⚙️ Etapas fijas (hardware)"
        INPUT["Input Assembler<br/>Lectura de vértices"] --> VS2
        FS2 --> RT["Raster Operations<br/>Depth test, stencil, blending"]
        RT --> FB["Frame Buffer<br/>Píxeles finales"]
    end
```

### 1.1. Pipeline clásica (paso a paso)

```mermaid
flowchart TD
    IA["1. Input Assembler<br/>Lee buffers de vértices<br/>e índices de la malla"]
    VS["2. Vertex Shader<br/>Transforma cada vértice:<br/>Local → Mundo → Vista → Proyección"]
    
    subgraph "Tessellation (opcional)"
        TC["Tess Control<br/>Nivel de subdivisión"] --> TE["Tess Eval<br/>Genera nuevos vértices"]
    end
    
    GS3["3. Geometry Shader<br/>Genera/elimina primitivas"]
    RS["4. Rasterizer<br/>Convierte triángulos → fragmentos<br/>(píxeles candidatos)"]
    FS2["5. Fragment Shader<br/>Calcula color de cada píxel"]
    OM["6. Output Merger<br/>Depth test, stencil, blending"]
    
    IA --> VS
    VS --> TC
    TE --> GS3
    GS3 --> RS
    RS --> FS2
    FS2 --> OM
    OM --> FB2[Frame Buffer → Pantalla]
```

### 1.2. Vertex Shader (GLSL)

```
#version 460 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aNormal;
layout (location = 2) in vec2 aTexCoord;

uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

out vec3 FragPos;
out vec3 Normal;
out vec2 TexCoord;

void main() {
    FragPos = vec3(model * vec4(aPos, 1.0));
    Normal = mat3(transpose(inverse(model))) * aNormal;
    TexCoord = aTexCoord;
    
    gl_Position = projection * view * vec4(FragPos, 1.0);
}
```

### 1.3. Fragment Shader (GLSL)

```
#version 460 core
in vec3 FragPos;
in vec3 Normal;
in vec2 TexCoord;

uniform sampler2D albedoMap;
uniform vec3 lightPos;
uniform vec3 viewPos;

out vec4 FragColor;

void main() {
    vec3 color = texture(albedoMap, TexCoord).rgb;
    
    // Iluminación básica
    vec3 norm = normalize(Normal);
    vec3 lightDir = normalize(lightPos - FragPos);
    float diff = max(dot(norm, lightDir), 0.0);
    
    vec3 result = color * (0.1 + diff * 0.9);
    FragColor = vec4(result, 1.0);
}
```

---

## 2. Sampling

### 2.1. ¿Qué es sampling?

**Sampling** es el proceso de tomar valores discretos de una función continua. En gráficos, significa **muestrear texturas** en coordenadas UV.

```mermaid
flowchart LR
    subgraph "Sampling de textura"
        UV["UV (0.34, 0.72)"] --> TEX["Textura<br/>Imagen discreta<br/>de píxeles (texels)"]
        TEX --> SAMPLE["Valor muestreado<br/>(color en esa UV)"]
    end
```

### 2.2. Modos de filtrado

```mermaid
flowchart TD
    FILTER[Filtrado de texturas] --> NEAREST[Nearest Neighbor<br/>Cada texel = píxel más cercano<br/>🧊 Aspecto pixelado]
    FILTER --> LINEAR[Bilinear<br/>Interpola 4 texels cercanos<br/>🔵 Suave pero borroso]
    FILTER --> TRILINEAR[Trilinear<br/>Bilinear + mipmap blending<br/>🟢 Suave sin parpadeo]
    FILTER --> ANISO[Anisotrópico<br/>Muestrea en ángulo<br/>⭐⭐ Calidad máxima]
```

```
// Muestreo en GLSL
vec4 color;

// Nearest neighbor (pixelado)
color = texture(sampler, uv);

// Con filtrado anisotrópico
color = texture(sampler, uv);  // Configurado en la API

// Muestreo manual con derivadas (mipmap level)
float mipLevel = textureQueryLod(sampler, uv).x;
color = textureLod(sampler, uv, mipLevel);
```

### 2.3. Mipmaps

Son versiones **pre-filtradas** y reducidas de la textura para evitar **aliasing** cuando un objeto está lejos.

```mermaid
flowchart LR
    M0["Mip 0<br/>1024×1024"] --> M1["Mip 1<br/>512×512"]
    M1 --> M2["Mip 2<br/>256×256"]
    M2 --> M3["Mip 3<br/>128×128"]
    M3 --> M4["... hasta 1×1"]
```

```
// Determinar mip level automático
float mipLevel = log2(max(dFdx(uv).x, dFdy(uv).y));
// dFdx/dFdy = derivadas de la UV en la pantalla
```

### 2.4. Aliasing y Antialiasing

```mermaid
flowchart TD
    subgraph "Aliasing (dientes de sierra)"
        DIAG["Línea diagonal"] --> PIX["Píxeles escalonados<br/>❌ Aliasing"]
    end
    
    subgraph "Antialiasing (suavizado)"
        DIAG2["Línea diagonal"] --> AA["Píxeles suavizados<br/>✅ Antialiasing"]
    end
```

**Técnicas de antialiasing:**

| Técnica | Cómo funciona | Coste |
|---|---|---|
| **SSAA** | Renderizar a mayor resolución y downscale | Muy alto |
| **MSAA** | Multi-sample solo en bordes de polígonos | Medio |
| **FXAA** | Filtro post-process (detecta bordes) | Bajo |
| **TAA** | Temporal: acumula frames anteriores | Medio |

---

## 3. Rasterización

### 3.1. ¿Qué es la rasterización?

Convertir **triángulos 3D** → **píxeles 2D**. Es la técnica **dominante** en juegos en tiempo real.

```mermaid
flowchart LR
    TRI["🔺 Triángulo 3D<br/>3 vértices"] --> RAST["Rasterizador<br/>(hardware)"]
    RAST --> PIX["Píxeles 2D<br/>Fragmentos"]
    PIX --> FS["Fragment Shader<br/>Calcula color"]
```

### 3.2. El proceso

```
Por cada triángulo:
  1. Proyectar vértices a 2D (pantalla)
  2. Calcular bounding box en píxeles
  3. Por cada píxel en el bounding box:
     a. ¿Está dentro del triángulo? (edge function)
     b. Si sí → generar fragmento
     c. Interpolar atributos (UV, normal, depth)
     d. Ejecutar Fragment Shader
```

### 3.3. Edge Function

Determina si un píxel está dentro de un triángulo:

```
// Edge function: producto cruz 2D
float edge(vec2 a, vec2 b, vec2 p) {
    return (b.x - a.x) * (p.y - a.y) - (b.y - a.y) * (p.x - a.x);
}

// Un píxel está dentro si edge(A,B,P), edge(B,C,P), edge(C,A,P) tienen el mismo signo
bool inside(vec2 A, vec2 B, vec2 C, vec2 P) {
    float e1 = edge(A, B, P);
    float e2 = edge(B, C, P);
    float e3 = edge(C, A, P);
    return (e1 >= 0 && e2 >= 0 && e3 >= 0) ||
           (e1 <= 0 && e2 <= 0 && e3 <= 0);
}
```

### 3.4. Interpolación de atributos (Barycentric coordinates)

```mermaid
flowchart TD
    TRI2["🔺 Triángulo<br/>V0, V1, V2"] --> BC["Coordenadas baricéntricas<br/>(α, β, γ)"]
    BC --> UV["UV = α·UV0 + β·UV1 + γ·UV2"]
    BC --> NORM["Normal = α·N0 + β·N1 + γ·N2"]
    BC --> DEPTH["Depth = α·Z0 + β·Z1 + γ·Z2"]
```

### 3.5. Rasterización vs Ray Tracing

| Aspecto | Rasterización | Ray Tracing |
|---|---|---|
| **Velocidad** | ⚡ Muy rápida | 🐢 Lenta (aunque mejora) |
| **Calidad** | Aproximada (sombras, reflejos) | Física (perfecta) |
| **Uso** | Estándar en juegos | Cine, RTX (híbrido) |
| **Hardware** | Cualquier GPU | GPU moderna (RTX, PS5, Xbox) |

---

## 4. Trazado de Rayos (Ray Tracing)

### 4.1. Concepto

Simula el **comportamiento físico de la luz**: traza rayos desde la cámara, rebotando por la escena.

```mermaid
flowchart LR
    CAM["📷 Cámara"] --> RAY["Rayo primario<br/>por cada píxel"]
    RAY --> HIT["¿Impacta en algo?"]
    HIT -->|"Sí"| BOUNCE["Rebotar (reflejo/refracción)"]
    BOUNCE --> HIT2["¿Impacta en algo?"]
    HIT2 --> LIGHT["Calcular luz en punto de impacto"]
    HIT -->|"No"| SKY["Color del cielo"]
```

### 4.2. Pipeline híbrida (estándar moderno)

Los juegos actuales usan **rasterización + ray tracing**:

```mermaid
flowchart TD
    RAST2["Rasterización<br/>G-Buffer (posición, normal, etc.)"] --> RT2["Ray Tracing selectivo"]
    RT2 --> SHADOWS["☀️ Sombras RT"]
    RT2 --> REFL["✨ Reflejos RT"]
    RT2 --> AO["🌑 Oclusión ambiental RT"]
    RT2 --> GI["💡 Iluminación global RT"]
    
    RAST2 --> DEF["Deferred Shading<br/>Iluminación principal"]
    SHADOWS --> COMP
    REFL --> COMP["Composición final"]
    AO --> COMP
    GI --> DEF
    DEF --> COMP
```

### 4.3. Tipos de efectos RT

```mermaid
flowchart TD
    RT[Trazado de rayos] --> RTS["Ray Traced Shadows<br/>Sombras suaves y precisas"]
    RT --> RTR["Ray Traced Reflections<br/>Reflejos perfectos"]
    RT --> RTAO["Ray Traced AO<br/>Oclusión ambiental precisa"]
    RT --> RTGI["Ray Traced GI<br/>Rebotes de luz global"]
    RT --> RTDEN["Denoising<br/>Eliminar ruido (imprescindible)"]
```

### 4.4. Ejemplo: Ray Tracing en GLSL (Compute Shader)

```
#version 460 core
#extension GL_EXT_ray_tracing : require

layout(set = 0, binding = 0) uniform accelerationStructureEXT topLevelAS;

layout(location = 0) rayPayloadEXT vec3 hitColor;

void main() {
    vec3 origin = cameraPos;
    vec3 direction = calculateRayDirection(gl_GlobalInvocationID.xy);
    
    // Trazar rayo
    traceRayEXT(topLevelAS,        // Acceleration structure
                gl_RayFlagsNoneEXT, // Flags
                0xFF,               // Cull mask
                0,                  // Ray offset
                0,                  // Ray offset
                0,                  // Ray offset
                origin,             // Origen
                0.001,              // Tmin
                direction,          // Dirección
                1000.0,             // Tmax
                0                   // Payload location
    );
    
    imageStore(outputImage, ivec2(gl_GlobalInvocationID.xy), vec4(hitColor, 1.0));
}
```

### 4.5. Denoising

El RT puro con pocos rayos por píxel (1-4) produce **mucho ruido**. El **denoising** es obligatorio:

```mermaid
flowchart LR
    NOISY["🌫️ Imagen ruidosa<br/>1 rayo/píxel"] --> DENOISE["Denoiser<br/>(SVGF, NRD, OptiX)"]
    DENOISE --> CLEAN["✨ Imagen limpia<br/>Spacio-temporal"]
```

**Denoisers comunes:**
- **SVGF** (Spatiotemporal Variance-Guided Filtering)
- **NRD** (NVIDIA Real-Time Denoiser)
- **OptiX Denoiser** (IA, muy rápido)

---

## 5. Resumen visual del pipeline completo

```mermaid
flowchart TD
    subgraph "🔄 Pipeline completo"
        APP["Aplicación<br/>Envía draw calls"] --> VS["Vertex Shader<br/>Transforma vértices"]
        VS --> RAST["Rasterizer<br/>Triángulos → Fragmentos"]
        RAST --> FS["Fragment Shader<br/>Color de cada píxel"]
        FS --> OM["Output Merger<br/>Depth + Blending"]
        OM --> FB["Frame Buffer<br/>Imagen final"]
    end
    
    subgraph "📦 Datos que fluyen"
        APP -->|"Vértices, matrices,<br/>texturas, uniforms"| VS
        VS -->|"Vértices transformados"| RAST
        RAST -->|"Fragmentos con<br/>atributos interpolados"| FS
        FS -->|"Color RGBA"| OM
    end
```

### Lenguajes por plataforma

```
Plataforma     │ API gráfica    │ Lenguaje shader
───────────────┼────────────────┼────────────────
PC Windows     │ DirectX 12     │ HLSL
PC multiplataf │ Vulkan         │ GLSL / SPIR-V
PC / Linux     │ OpenGL         │ GLSL
Mac / iOS      │ Metal          │ MSL
PS5            │ Proprietary    │ PSSL (basado en HLSL)
Xbox Series    │ DirectX 12 HW  │ HLSL
Nintendo Switch│ NVN            │ GLSL modificado
Web            │ WebGL / WebGPU │ GLSL / WGSL
```

> 💡 **Regla de oro:** El Vertex Shader mueve geometría, el Fragment Shader pinta píxeles, y el Ray Tracing (cuando se pueda) hace que todo luzca real. En la práctica, los juegos modernos usan **rasterización para el 90%** y **RT para los efectos clave** (sombras, reflejos, AO).

## Relacionados:
- [[color-y-animacion]] #anterior 
- [[apis-graficas]] #siguiente 