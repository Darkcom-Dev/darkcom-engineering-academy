# Mapping y Reflexión

## 1. Mapping

**Mapping** es el proceso de aplicar datos 2D (texturas, normales, alturas) sobre una superficie 3D para simular detalle sin añadir geometría.

```mermaid
flowchart TD
    MAP[Métodos de Mapping] --> TX["Texturizado UV<br/>Color superficial"]
    MAP --> BM["Bump/Normal Mapping<br/>Simular relieves"]
    MAP --> PX["Parallax Mapping<br/>Desplazamiento UV"]
    MAP --> HM["Horizon Mapping<br/>Auto-sombras"]
    MAP --> DM["Displacement Mapping<br/>Geometría real"]
```

### 1.1. Texturas (UV Mapping)

Cada vértice del modelo 3D tiene coordenadas **UV** que mapean a un punto en la textura 2D.

```mermaid
flowchart LR
    subgraph "Modelo 3D"
        M1["Vértice → UV (0.3, 0.7)"]
        M2["Vértice → UV (0.8, 0.2)"]
    end
    subgraph "Textura 2D"
        TX["📐 Mapa UV<br/>Cada texel tiene un color"]
    end
    TX --> RESULT["Resultado final: modelo texturizado"]
    M1 --> RESULT
    M2 --> RESULT
```

**Problema común: distorsión UV**

```mermaid
flowchart LR
    GOOD["✅ Buena UV<br/>Texeles uniformes"] --> BAD["❌ Mala UV<br/>Texeles estirados<br/>→ textura borrosa"]
```

**Tipos de texturas:**

```
Albedo (diffuse)   → Color base del material
Normal Map         → Perturbación de normales (relieve)
Roughness          → Rugosidad (0=liso, 1=áspero)
Metalness          → Metal (0=dieléctrico, 1=metal)
Ambient Occlusion  → Sombras de contacto precalculadas
Displacement       → Desplazamiento de vértices (altura)
Emissive           → Color auto-iluminado
```

---

### 1.2. Bump Mapping

Simula **relieves** en una superficie plana modificando las normales antes de iluminar. No cambia la geometría.

```mermaid
flowchart TD
    subgraph "Tipos de Bump Mapping"
        BUMP["Bump Map<br/>Imagen en escala de grises<br/>Altura = valor del píxel"]
        NORMAL["Normal Map<br/>Imagen RGB<br/>Normal codificada en color"]
        DISP["Displacement<br/>Mueve vértices reales<br/>(tessellation)"]
    end
```

#### Normal Mapping (el más usado)

Cada texel RGB codifica una **normal**:

```
Normal = (2·R - 1, 2·G - 1, 2·B - 1)

R=128 → normal.x = 0
G=128 → normal.y = 0
B=255 → normal.z = 1 (hacia afuera)
```

```mermaid
flowchart LR
    FLAT["Superficie lisa<br/>Normal = (0,0,1) universal"] --> NM["Normal Map<br/>Cada texel tiene<br/>su propia normal"]
    NM --> DETAIL["Superficie con relieve<br/>Iluminación detallada"]
```

```
// Fragment shader: aplicar normal map
uniform sampler2D normalMap;

vec3 getNormal(vec2 uv, mat3 TBN) {
    vec3 normal = texture(normalMap, uv).rgb;
    normal = normal * 2.0 - 1.0;      // [0,1] → [-1,1]
    normal = normalize(TBN * normal); // Tangent → World space
    return normal;
}

// TBN matrix: transforma de tangent space a world space
vec3 T = normalize(model * vec3(tangent, 0.0));
vec3 B = normalize(model * vec3(bitangent, 0.0));
vec3 N = normalize(model * vec3(normal, 0.0));
mat3 TBN = mat3(T, B, N);
```

#### Bump Map (escala de grises)

Más simple que normal map: calcula la normal a partir de derivadas de la altura.

```
// Calcular normal desde bump map (height map)
float hL = texture(heightMap, uv - vec2(dx, 0)).r;
float hR = texture(heightMap, uv + vec2(dx, 0)).r;
float hD = texture(heightMap, uv - vec2(0, dy)).r;
float hU = texture(heightMap, uv + vec2(0, dy)).r;

vec3 normal = normalize(vec3(hL - hR, hD - hU, 1.0 / strength));
```

| Aspecto | Bump Map | Normal Map | Displacement |
|---|---|---|---|
| **Formato** | Gris (1 canal) | RGB (3 canales) | Gris o RGB |
| **Calidad** | Baja | Alta | Muy alta |
| **Geometría** | No cambia | No cambia | Sí (vértices) |
| **Silueta** | Plana ❌ | Plana ❌ | Real ✅ |
| **Coste** | Muy bajo | Bajo | Alto |

---

### 1.3. Parallax Mapping

Mejora el normal mapping desplazando las coordenadas UV según la **altura** y el **ángulo de vista**. Crea ilusión de profundidad real.

```mermaid
flowchart LR
    subgraph "Parallax Mapping"
        EYE["👁️ Cámara"] --> SURF["Superficie<br/>UV original"]
        SURF --> HEIGHT["Height map<br/>valor en UV"]
        HEIGHT --> SHIFT["Desplazar UV<br/>basado en altura y ángulo"]
        SHIFT --> NEW["Nueva UV<br/>se samplea la textura aquí"]
    end
```

```
// Parallax mapping simple
vec2 parallaxMapping(sampler2D heightMap, vec2 uv, vec3 viewDir) {
    float height = texture(heightMap, uv).r;
    vec2 offset = viewDir.xy * (height * parallaxScale);
    return uv - offset;
}

// Parallax Occlusion Mapping (POM) - mejor calidad
// Samplea la altura en varios pasos y encuentra la intersección
float numLayers = mix(maxLayers, minLayers, abs(dot(viewDir, normal)));
float layerHeight = 1.0 / numLayers;
float currentLayerHeight = 0.0;
vec2 dUV = viewDir.xy * parallaxScale / numLayers;
vec2 currentUV = uv;
float heightFromMap = 1.0 - texture(heightMap, currentUV).r;

for (int i = 0; i < numLayers; i++) {
    if (currentLayerHeight > heightFromMap) break;
    currentLayerHeight += layerHeight;
    currentUV -= dUV;
    heightFromMap = 1.0 - texture(heightMap, currentUV).r;
}

// Interpolar entre capas para suavizar
return currentUV;
```

**Comparativa visual:**

```mermaid
flowchart LR
    NORMAL2["Normal Map<br/>Superficie plana<br/>sombreado detallado"] --> PARALLAX["Parallax<br/>Desplazamiento UV<br/>profundidad aparente"]
    PARALLAX --> POM["POM (Parallax Occlusion)<br/>Múltiples capas<br/>casi 3D"]
```

---

### 1.4. Horizon Mapping

Técnica para calcular **auto-sombras** a nivel de texel. Para cada punto, calcula el ángulo del horizonte en varias direcciones (como horizonte de una montaña).

```mermaid
flowchart TD
    subgraph "Horizon Mapping"
        POINT["Punto en superficie"] --> DIR1["Dirección 1: horizonte a 30°"]
        POINT --> DIR2["Dirección 2: horizonte a 50°"]
        POINT --> DIR3["Dirección 3: horizonte a 10°"]
        DIR1 --> SHADOW["Si luz está bajo horizonte → sombra"]
        DIR2 --> SHADOW
        DIR3 --> SHADOW
    end
```

**Precomputación** (offline):
Para cada texel, almacena el ángulo del horizonte en N direcciones (típicamente 6 u 8).

```
// En runtime: para cada dirección de luz
float ao = 1.0;
for each direction i:
    float horizonAngle = horizonMap[i][uv];
    float lightAngle = calcularAnguloLuz(direccionLuz[i]);
    if (lightAngle < horizonAngle):
        ao -= (horizonAngle - lightAngle) / pi;
```

**Diferencia con AO (Ambient Occlusion):**
- **AO:** sombras precalculadas estáticas (bakeadas)
- **Horizon Mapping:** sombras dinámicas que responden a la dirección de la luz

---

## 2. Reflexión

**Reflexión** = la luz rebota en una superficie y vuelve al ojo. El comportamiento depende del material.

```mermaid
flowchart TD
    REF[Reflexión] --> DIF[Difusa<br/>Lambertiana<br/>Mate]
    REF --> SPEC[Especular<br/>Brillante<br/>Tipo espejo]
    REF --> GLOSSY[Glossy<br/>Entre difusa y especular<br/>Roughness intermedio]
```

### 2.1. Reflexión Difusa

La luz se **dispersa** en todas direcciones al impactar. No importa desde dónde mires, el brillo es el mismo.

```mermaid
flowchart LR
    LIGHT["💡 Luz"] --> SURF1["Superficie rugosa"]
    SURF1 --> R1["↗️ Rebota en todas direcciones"]
    SURF1 --> R2["↘️ Igual intensidad en todas"]
    SURF1 --> R3["↖️ Sin dirección preferida"]
```

```
// Reflexión difusa (Lambert)
float NdotL = max(dot(normal, lightDir), 0.0);
vec3 diffuse = albedo * lightColor * NdotL;
```

**Materiales difusos:** papel, madera sin barnizar, tela, piedra, yeso.

### 2.2. Reflexión Especular

La luz rebota en una **dirección preferente** (como un espejo). Depende del ángulo de vista.

```mermaid
flowchart LR
    LIGHT2["💡 Luz"] --> SURF2["Superficie lisa (espejo)"]
    SURF2 --> R4["↗️ Dirección de reflexión<br/>θ_entrada = θ_salida"]
    EYE["👁️ Ojo"] --> R4
```

```
// Reflexión especular (Phong)
vec3 reflectDir = reflect(-lightDir, normal);
float spec = pow(max(dot(viewDir, reflectDir), 0.0), shininess);
vec3 specular = specularColor * lightColor * specStrength;

// Modelo Cook-Torrance (PBR moderno)
// Usa distribución de micro-facetas, Fresnel, y función geométrica
float D = distributionGGX(NdotH, roughness);
float G = geometrySmith(NdotV, NdotL, roughness);
vec3 F = fresnelSchlick(cosTheta, F0);

vec3 specular = (D * G * F) / (4 * NdotV * NdotL + 0.0001);
```

**Materiales especulares:** metal pulido, vidrio, agua, plástico brillante, espejos.

### 2.3. Modelo PBR (Physically Based Rendering)

Estándar moderno. Se basa en: **Roughness + Metalness**.

```mermaid
flowchart LR
    PBR[PBR] --> METAL[Metalness<br/>0 = dieléctrico<br/>1 = metal]
    PBR --> ROUGH[Roughness<br/>0 = liso (especular)<br/>1 = rugoso (difuso)]
    
    METAL --> F0["Fresnel F0<br/>Metal: color del metal<br/>Dieléctrico: 0.04"]
    ROUGH --> MICRO["Micro-facetas<br/>Roughness controla<br/>distribución de normales"]
```

```
// Ecuación de reflectancia PBR (Cook-Torrance)
Lo = (kD * albedo / pi + D * F * G / (4 * NdotV * NdotL)) * LdotV * lightColor;

// kD = 1 - F (difuso complementario)
// D = distribución de normales (GGX/Trowbridge-Reitz)
// F = Fresnel (Schlick approximation)
// G = función geométrica (Smith)
```

### 2.4. Environment Mapping (Reflejos del entorno)

Para reflejar el **entorno** (cielo, edificios) sin geometría real, se usan **environment maps**:

```mermaid
flowchart LR
    SUB["Superficie reflectante"] --> ENV["Environment Map<br/>Cubemap / Equirectangular"]
    ENV --> REFL["Reflejo: samplear<br/>en dirección de reflexión"]
```

```
// Reflejo con cubemap
vec3 viewDir = normalize(viewPos - FragPos);
vec3 reflectDir = reflect(-viewDir, normal);
vec3 reflection = texture(envCubemap, reflectDir).rgb;

// Mezclar reflexión con color superficial
float fresnel = fresnelSchlick(max(dot(viewDir, normal), 0.0), F0);
vec3 finalColor = mix(albedo, reflection, fresnel);
```

### 2.5. Screen Space Reflections (SSR)

Técnica moderna que usa el **frame actual** como fuente de reflejos, sin necesidad de cubemaps.

```mermaid
flowchart LR
    PIXEL["Píxel actual"] --> RAY["Trazar rayo reflectado en screen space"]
    RAY --> MARCH["Ray march por el depth buffer"]
    MARCH --> HIT["Intersección → samplear color de la escena"]
```

**Ventajas:** Reflejos dinámicos, cualquier objeto se refleja.
**Desventajas:** No refleja objetos fuera de pantalla, ruidoso.

---

## 3. Resumen visual completo

```mermaid
flowchart TD
    subgraph "Pipeline de shading por píxel"
        UV["Coordenadas UV"] --> ALBEDO["Samplear Albedo"]
        UV --> NORMAL["Samplear Normal Map"]
        NORMAL --> LIGHT["Calcular iluminación<br/>con normal perturbada"]
        ALBEDO --> LIGHT
        UV --> HEIGHT["Samplear Height Map<br/>(Parallax)"]
        HEIGHT --> SHIFT["Desplazar UV<br/>(Parallax Occlusion)"]
        SHIFT --> ALBEDO
        SHIFT --> NORMAL
        
        LIGHT --> SPEC2["Reflexión especular<br/>(Environment / SSR)"]
        LIGHT --> DIFF2["Reflexión difusa<br/>(albedo × NdotL)"]
        SPEC2 --> FINAL["Color final"]
        DIFF2 --> FINAL
    end
```

> 💡 **Regla de oro:** 
> - **Normal maps** para el 90% del detalle superficial (barato y efectivo)
> - **Parallax Occlusion Mapping** cuando necesitas profundidad realista (suelos, paredes de piedra)
> - **PBR** es el estándar moderno: usa Roughness + Metalness + Environment Map
> - **SSR** para reflejos dinámicos; **cubemaps** para reflejos estáticos de entorno
