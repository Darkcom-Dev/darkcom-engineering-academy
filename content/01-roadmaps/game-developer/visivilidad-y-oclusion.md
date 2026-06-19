# Visibilidad y Oclusión

## ¿Por qué es importante?

En una escena 3D típica puede haber millones de triángulos. Renderizarlos todos sería imposible. Las técnicas de **visibilidad y oclusión** determinan **qué se ve y qué no**, ahorrando miles de draw calls.

```mermaid
flowchart LR
    ALL["🌍 Escena completa<br/>1.000.000 triángulos"] --> CULL["Técnicas de visibilidad"]
    CULL --> VIS["👁️ Visible<br/>~10.000 triángulos"]
    CULL --> HIDDEN["🚫 Ocluido / Fuera<br/>~990.000 NO se renderizan"]
```

---

## 1. Culling

**Culling** = eliminar objetos que no aportan a la imagen final.

```mermaid
flowchart TD
    CULLING[Técnicas de Culling] --> FRUSTUM[Frustum Culling<br/>Fuera del campo de visión]
    CULLING --> OCC[Occlusion Culling<br/>Tapado por otro objeto]
    CULLING --> BACK[Back-face Culling<br/>De espaldas a la cámara]
    CULLING --> DIST[Distance Culling<br/>Demasiado lejos]
    CULLING --> LOD[LOD Culling<br/>Nivel de detalle]
```

### 1.1. Frustum Culling

El **frustum** es la pirámide truncada que representa lo que ve la cámara. Todo lo que esté fuera se descarta.

```mermaid
flowchart LR
    subgraph "Frustum de visión"
        CAM["📷 Cámara"] --> NEAR["Plano Near"]
        NEAR --> VOL["Volumen visible"]
        VOL --> FAR["Plano Far"]
    end
```

**Los 6 planos del frustum:**

```
 1. Plano Near (cerca)
 2. Plano Far  (lejos)
 3. Plano Left (izquierda)
 4. Plano Right (derecha)
 5. Plano Top (arriba)
 6. Plano Bottom (abajo)
```

**Prueba punto vs frustum:**

```
bool isInsideFrustum(vec3 point, Plane[6] frustum) {
    for (int i = 0; i < 6; i++) {
        // Ecuación del plano: N·P + d >= 0 → dentro
        if (dot(frustum[i].normal, point) + frustum[i].d < 0)
            return false;  // Fuera de este plano → descartar
    }
    return true;
}
```

Para objetos grandes se usa la **bounding sphere** o **AABB** en lugar del punto:

```
bool sphereInFrustum(vec3 center, float radius, Plane[6] frustum) {
    for (int i = 0; i < 6; i++) {
        float dist = dot(frustum[i].normal, center) + frustum[i].d;
        if (dist < -radius) return false;  // Esfera completamente fuera
    }
    return true;
}
```

### 1.2. Light Culling

No todas las luces afectan a todos los objetos. Se determina qué luces iluminan qué partes de la escena.

```mermaid
flowchart TD
    LIGHTS["💡 100 luces en escena"] --> TILE["Tile-based culling<br/>Dividir pantalla en tiles 16×16"]
    TILE --> T1["Tile 1: 3 luces"]
    TILE --> T2["Tile 2: 5 luces"]
    TILE --> T3["Tile 3: 0 luces ❌"]
```

**Técnicas:**
- **Tile-based:** Divide la pantalla en tiles y asigna luces por tile
- **Cluster-based:** Divide en clusters 3D (más preciso)
- **Z-bucket:** Por rango de profundidad

```
// Tile-based light culling (compute shader)
for each tile (16×16 píxeles):
    for each light:
        if light afecta este tile:
            añadir light a lista del tile

// Fragment shader: cada píxel solo itera las luces de su tile
for light in lightList[tileIndex]:
    color += calcularLuz(light, fragment);
```

### 1.3. Shadow Culling

No todos los objetos proyectan sombra, ni todos la reciben. Optimización de shadow maps.

```
Cada objeto tiene flags:
  - CastShadow  : ¿proyecta sombra? (sí/no)
  - ReceiveShadow : ¿recibe sombra? (sí/no)

Objetos que no proyectan sombra → no se renderizan en el shadow map
Objetos que no reciben sombra → no ejecutan el shader de sombras

Ejemplo:
  Suelo:    Cast=No,  Receive=Sí
  Pared:    Cast=Sí,  Receive=Sí
  Césped:   Cast=No,  Receive=Sí
  Techo:    Cast=Sí,  Receive=No
```

---

## 2. Clipping

**Clipping** = recortar partes de geometría que quedan fuera del volumen visible.

### 2.1. Clipping de Polígono (2D)

Recorta un polígono contra un rectángulo (viewport) o contra los planos del frustum.

```
// Algoritmo Sutherland-Hodgman
function clipPolygon(vertices, plano):
    salida = []
    for each edge (v_i, v_{i+1}):
        if v_{i+1} está dentro:
            if v_i está fuera:
                salida.push(intersectar(v_i, v_{i+1}, plano))
            salida.push(v_{i+1})
        else if v_i está dentro:
            salida.push(intersectar(v_i, v_{i+1}, plano))
    return salida
```

```mermaid
flowchart LR
    subgraph "Antes de clip"
        POLY1["🔶 Polígono original<br/>parcialmente fuera"]
    end
    subgraph "Después de clip"
        POLY2["🔷 Polígono recortado<br/>nuevos vértices añadidos"]
    end
```

### 2.2. Clipping de Poliedro (3D)

Versión 3D del clipping de polígono. Recorta un volumen 3D contra los planos del frustum.

**Homogeneous clipping (hardware):**
En la pipeline moderna, el clipping se hace en **coordenadas homogéneas** después de la transformación de proyección:

```
// Después de multiplicar por la matriz de proyección:
vértice_clip = projection * view * model * vértice_local

// El hardware descarta automáticamente vértices donde:
//   -w < x < w
//   -w < y < w
//   -w < z < w
// Si algún vértice del triángulo cruza el borde → se recorta
```

---

## 3. Occlusion Culling (Occluder)

Determina qué objetos están **tapados** por otros. Es más avanzado que frustum culling.

### 3.1. Concepto

```mermaid
flowchart LR
    EYE["👁️ Cámara"] --> OCCLUDER["🏠 Edificio (occluder)<br/>Tapa lo que está detrás"]
    OCCLUDER --> HIDDEN["🚫 Enemigo DETRÁS<br/>No visible → no renderizar"]
    EYE --> VISIBLE["👤 Enemigo VISIBLE<br/>Sí renderizar"]
```

### 3.2. Técnicas de occlusion culling

```mermaid
flowchart TD
    OCC[Occlusion Culling] --> HW["Hardware Occlusion Queries<br/>GPU responde: ¿se ve?"]
    OCC --> SW["Software rasterization<br/>CPU rasteriza occluders simples"]
    OCC --> PVS["PVS (Potentially Visible Set)<br/>Precalculado (estático)"]
    OCC --> PORTAL["Portal Culling<br/>Puertas/ventanas (interiores)"]
    OCC --> HIZ["Hi-Z Culling<br/>Depth buffer jerárquico"]
```

#### Hardware Occlusion Queries

```
// 1. Crear query
GLuint query;
glGenQueries(1, &query);

// 2. Renderizar objetos grandes y cercanos primero (occluders)
renderizarEdificios();

// 3. Preguntar por objetos pequeños
glBeginQuery(GL_ANY_SAMPLES_PASSED, query);
renderizarEnemigo();  // Renderiza solo el bounding box
glEndQuery(query);

// 4. Leer resultado
GLuint visible;
glGetQueryObjectuiv(query, GL_QUERY_RESULT, &visible);

if (visible) {
    renderizarEnemigoDetallado();  // Se ve → renderizar completo
}
```

#### Portal Culling

Para escenas de **interiores**: divide en habitaciones conectadas por portales (puertas, ventanas).

```mermaid
flowchart TD
    ROOM1["Habitación 1<br/>(cámara aquí)"] --> PORTAL1["🚪 Puerta abierta"]
    PORTAL1 --> ROOM2["Habitación 2<br/>(visible a través de la puerta)"]
    ROOM2 --> PORTAL2["🚪 Puerta cerrada"]
    PORTAL2 --> ROOM3["Habitación 3<br/>(NO visible)"]
    
    ROOM1 --> WALL["🧱 Pared sólida"]
    WALL --> ROOM4["Habitación 4<br/>(NO visible)"]
```

#### PVS (Potentially Visible Set)

Precalculado en tiempo de compilación del mapa. Divide en celdas y calcula qué es visible desde cada una.

```
// Datos precalculados:
Celda 1 → puede ver: {Celda 1, Celda 2, Celda 5}
Celda 2 → puede ver: {Celda 1, Celda 2, Celda 3}
Celda 3 → puede ver: {Celda 2, Celda 3, Celda 4}

// En runtime:
celdaActual = determinarCelda(camara.position);
objetosRenderizar = PVS[celdaActual];
```

#### Hi-Z Culling

Usa una **pirámide de profundidad** (depth mipmap) para descartar objetos de golpe.

```
// 1. Generar Hi-Z map (mipmaps del depth buffer)
// 2. Por cada objeto:
//    - Proyectar bounding box a pantalla
//    - Obtener profundidad mínima del Hi-Z map en esa región
//    - Si el objeto está detrás de esa profundidad → oculto
```

### 3.3. Implementación práctica (Unreal Engine)

```mermaid
flowchart TD
    UE["Unreal Engine Occlusion Culling"] --> PRE["Precomputed Visibility<br/>Volumes (escenas estáticas)"]
    UE --> HWQ["Hardware Occlusion Queries<br/>(escenas dinámicas)"]
    UE --> HIZ["Hi-Z Culling (GPU)"]
    UE --> DIST["Distance Culling<br/>Global / por actor"]
```

### 3.4. Comparativa

| Técnica | Estático/Dinámico | Coste CPU | Coste GPU | Precisión |
|---|---|---|---|---|
| Frustum Culling | Ambos | Bajo | Ninguno | Baja (solo fuera de vista) |
| HW Occlusion Queries | Dinámico | Medio | Medio | Alta |
| Portal Culling | Estático | Bajo | Ninguno | Muy alta (interiores) |
| PVS | Estático | Muy bajo | Ninguno | Muy alta (precalculado) |
| Hi-Z Culling | Ambos | Bajo | Medio | Alta |
| Software Occlusion | Dinámico | Alto | Bajo | Alta |

---

## 4. Niebla (Fog)

La niebla no solo es atmosférica: es una **herramienta de optimización** que permite ocultar el pop-in de objetos lejanos.

### 4.1. Tipos de niebla

```mermaid
flowchart TD
    FOG[Niebla] --> LINEAR["Lineal<br/>d = distancia<br/>factor = (d - near) / (far - near)"]
    FOG --> EXP["Exponencial<br/>factor = 1 - e^(-d·densidad)"]
    FOG --> EXP2["Exponencial²<br/>factor = 1 - e^(-(d·densidad)²)"]
    FOG --> HEIGHT["Altura (Height Fog)<br/>Niebla a nivel del suelo"]
    FOG --> VOL["Volumétrica<br/>Ray marching en niebla 3D"]
```

### 4.2. Fórmulas

```
// Niebla lineal
float fogFactor = (viewDistance - fogStart) / (fogEnd - fogStart);
fogFactor = clamp(fogFactor, 0.0, 1.0);
finalColor = mix(sceneColor, fogColor, fogFactor);

// Niebla exponencial
float fogFactor = 1.0 - exp(-density * viewDistance);
finalColor = mix(sceneColor, fogColor, fogFactor);

// Niebla exponencial² (más realista)
float fogFactor = 1.0 - exp(-pow(density * viewDistance, 2));
finalColor = mix(sceneColor, fogColor, fogFactor);
```

### 4.3. Height Fog (Niebla por altura)

```mermaid
flowchart LR
    SKY["☁️ Cielo despejado<br/>Sin niebla"] --> FOG_L["🌫️ Capa de niebla"]
    FOG_L --> GROUND["🌍 Suelo<br/>Máxima niebla"]
```

```
// Height fog: niebla que aumenta cerca del suelo
float heightFactor = exp(-falloff * max(0.0, fogHeight - worldPos.y));
float fogAmount = fogDensity * viewDistance * heightFactor;
```

### 4.4. Niebla como optimización

```mermaid
flowchart LR
    NOFOG["Sin niebla<br/>Se ven objetos hasta el far plane<br/>🔄 Muchos draw calls"] --> FOG["Con niebla<br/>Far plane puede ser más cercano<br/>🔄 Menos draw calls"]
    FOG --> OPT["Far plane = 100m<br/>Cerca del jugador<br/>La niebla oculta el corte"]
```

---

## 5. Jerarquía completa de visibilidad

```mermaid
flowchart TD
    ALL["🌍 Todos los objetos"] --> FF["Frustum Culling"]
    FF -->|"Fuera de cámara"| OUT["🚫 Descartado"]
    FF -->|"Dentro"| DIST2["Distancia / LOD"]
    DIST2 -->|"Muy lejos"| CULLED["🚫 Descartado"]
    DIST2 -->|"Cerca"| OCC2["Occlusion Culling"]
    OCC2 -->|"Tapado"| OCCL["🚫 Descartado"]
    OCC2 -->|"Visible"| LODC["LOD Selection"]
    LODC --> LOD0["LOD0 (máximo detalle)"]
    LODC --> LOD1["LOD1"]
    LODC --> LOD2["LOD2 (mínimo detalle)"]
    LOD0 --> RENDER["✅ Renderizar"]
    LOD1 --> RENDER
    LOD2 --> RENDER
```

### Orden de ejecución (recomendado)

```
Por cada frame:
  1. Frustum Culling      → descarta 50-70% (rápido, CPU)
  2. Distance Culling     → descarta objetos lejanos
  3. Occlusion Culling    → descorta objetos tapados (más caro)
  4. LOD Selection        → elegir nivel de detalle
  5. Shadow Culling       → qué objetos al shadow map
  6. Light Culling        → qué luces afectan a qué
  7. Enviar draw calls a GPU
```

> 💡 **Regla de oro:** No renderices lo que no se ve. Empieza con Frustum Culling (es gratis y descarta la mitad), añade occlusion culling para interiores/urbanas, y usa niebla para disimular el pop-in de objetos lejanos.
