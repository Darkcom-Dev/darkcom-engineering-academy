# Detección de Colisiones

## ¿Por qué es crítica?

La detección de colisiones es el **cuello de botella** de la física en juegos. Cada frame, cientos de objetos deben verificar si chocan entre sí.

```mermaid
flowchart LR
    NO_COLL["❌ Sin detección<br/>Objetos se atraviesan<br/>Mundo irreal"] --> COLL["✅ Con detección<br/>Objetos interactúan<br/>Mundo creíble"]
```

### Estructura general

```mermaid
flowchart TD
    COLL_DET[Detección de Colisiones] --> BROAD["🔭 Fase Amplia (Broad)<br/>Encontrar pares potenciales<br/>Rápido, barato"]
    COLL_DET --> NARROW["🔬 Fase Estrecha (Narrow)<br/>Verificar colisión exacta<br/>Preciso, caro"]
    
    BROAD --> NARROW
    NARROW --> RESOLVE["Resolver colisión<br/>(impulso, penetración)"]
```

---

## 1. Fase Amplia (Broad Phase)

Objetivo: **descartar rápidamente** pares de objetos que **no** pueden colisionar.

```mermaid
flowchart LR
    ALL["🔵🔴🟢🟡<br/>100 objetos<br/>4950 pares posibles"] --> BROAD2["Fase Amplia"]
    BROAD2 --> PAIRS["🔵🔴<br/>🔵🟡<br/>💥 50 pares candidatos"]
    BROAD2 --> DISCARD["🟢 descartado<br/>🟡 descartado"]
```

### 1.1. Bounding Volumes

Envoltura que aproxima la forma del objeto para pruebas rápidas.

```mermaid
flowchart TD
    BV[Bounding Volumes] --> AABB["AABB<br/>Axis-Aligned<br/>Bounding Box"]
    BV --> SPHERE["Esfera<br/>Bounding Sphere"]
    BV --> OBB["OBB<br/>Oriented<br/>Bounding Box"]
    BV --> CONVEX["Convex Hull<br/>Aproximación más precisa"]
```

#### AABB (Axis-Aligned Bounding Box)

Caja alineada a los ejes del mundo. No rota con el objeto.

```mermaid
flowchart LR
    OBJ["🔶 Objeto rotado"] --> AABB2["📦 AABB (verde)<br/>Alineado a ejes X,Y,Z<br/>⚠️ No ajustado al objeto"]
```

```
// Prueba de intersección AABB vs AABB
struct AABB {
    vec3 min;
    vec3 max;
};

bool intersectAABB(AABB a, AABB b) {
    return (a.min.x <= b.max.x && a.max.x >= b.min.x) &&
           (a.min.y <= b.max.y && a.max.y >= b.min.y) &&
           (a.min.z <= b.max.z && a.max.z >= b.min.z);
}
```

#### Bounding Sphere

Esfera que envuelve el objeto. La prueba más rápida.

```
// Prueba esfera vs esfera
bool intersectSphere(vec3 centerA, float radiusA, vec3 centerB, float radiusB) {
    float dist = length(centerA - centerB);
    return dist < radiusA + radiusB;
}
```

**Ventaja:** Solo 1 comparación (distancia). Ideal para broad phase.

#### OBB (Oriented Bounding Box)

Caja que **rota con el objeto**. Más precisa que AABB pero más cara.

```mermaid
flowchart LR
    OBJ2["🔶 Objeto rotado"] --> OBB2["📦 OBB (naranja)<br/>Rota con el objeto<br/>✅ Ajustado"]
```

### 1.2. Ordenamiento y Barrido (Sweep and Prune)

Técnica clásica de broad phase para AABBs.

```
1. Proyectar todos los AABBs en el eje X
2. Ordenar por min.x
3. Barrer la lista: si max.x[i] > min.x[j] → posible colisión
4. Repetir para Y y Z (o solo X si es suficiente)
```

```mermaid
flowchart LR
    subgraph "Sweep and Prune en X"
        A["📦 A"] -->|"min.x=0, max.x=5"| LIST
        B["📦 B"] -->|"min.x=3, max.x=8"| LIST
        C["📦 C"] -->|"min.x=10, max.x=15"| LIST
        LIST["Ordenados por min.x"] --> PAIR["A-B se solapan ✅<br/>A-C no ❌<br/>B-C no ❌"]
    end
```

```
// Sweep and Prune simplificado
vector<Pair> sweepAndPrune(AABB[] boxes) {
    vector<Pair> candidates;
    
    sort(boxes, [](AABB a, AABB b) { return a.min.x < b.min.x; });
    
    for (int i = 0; i < boxes.length; i++) {
        for (int j = i + 1; j < boxes.length; j++) {
            if (boxes[i].max.x < boxes[j].min.x) break;  // No solapan en X
            if (intersectY(boxes[i], boxes[j]) && 
                intersectZ(boxes[i], boxes[j])) {
                candidates.push({i, j});
            }
        }
    }
    
    return candidates;
}
```

### 1.3. BVH (Bounding Volume Hierarchy)

Árbol jerárquico de bounding volumes. Los objetos se agrupan en una estructura de árbol.

```mermaid
flowchart TD
    ROOT2["Root<br/>BV que envuelve todo"] --> L["BVH Left<br/>Grupo izquierdo"]
    ROOT2 --> R["BVH Right<br/>Grupo derecho"]
    
    L --> L1["Obj A"]
    L --> L2["Obj B"]
    L --> L3["Obj C"]
    
    R --> R1["Obj D"]
    R --> R2["Obj E"]
```

**Prueba de colisión con BVH:**

```
function testBVH(node, queryObject):
    if not intersect(node.BV, queryObject.BV):
        return []  // Descartar rama entera
    
    if node.isLeaf():
        return [node.object]  // Posible colisión
    
    // Probar hijos
    return testBVH(node.left, queryObject) + 
           testBVH(node.right, queryObject)
```

### 1.4. DBVT (Dynamic Bounding Volume Tree)

Variante de BVH optimizada para **objetos dinámicos** que se mueven cada frame. Usada en **Bullet Physics** y **Godot**.

```
Características:
- Árbol binario con AABB en cada nodo
- Refit rápido (actualizar AABBs de objetos móviles)
- Rebalanceo periódico
- Inserción/eliminación dinámica de objetos
```

### 1.5. Posicionamiento Espacial (Spatial Hashing)

Divide el mundo en una **cuadrícula** (grid). Solo se prueban objetos en la misma celda o adyacentes.

```mermaid
flowchart TD
    subgraph "Spatial Grid"
        G11["🔵"] --- G12["🔴"] --- G13
        G21 --- G22["🟢"] --- G23
        G31 --- G32 --- G33["🟡"]
    end
    
    G11 -.->|"celda (0,0)"| PAIRS2["🔵 solo colisiona<br/>con objetos en<br/>celdas vecinas"]
```

```
// Spatial hashing
int cellX = floor(position.x / cellSize);
int cellY = floor(position.y / cellSize);
int key = hash(cellX, cellY);

// Solo probar colisiones con objetos en la misma celda
```

---

## 2. Fase Estrecha (Narrow Phase)

Objetivo: determinar **exactamente** si dos formas colisionan y **dónde** (punto de contacto, normal, profundidad).

### 2.1. Convexividad

La convexidad determina qué algoritmos se pueden usar.

```mermaid
flowchart TD
    SHAPE[Formas] --> CONVEX2["Convexo<br/>Toda línea entre<br/>2 puntos está dentro"]
    SHAPE --> CONCAVE["Cóncavo<br/>Hay líneas que<br/>salen de la forma"]
    
    CONVEX2 --> ALGOS["✅ SAT, GJK, EPA<br/>Rápidos"]
    CONCAVE --> DECOMP["❌ Hay que descomponer<br/>en partes convexas"]
```

#### Convex Hull

La **envolvente convexa** más pequeña que contiene un conjunto de puntos.

```mermaid
flowchart LR
    POINTS["🔵🔵🔵<br/>🔵🔵🔵<br/>Puntos dispersos"] --> HULL["⬡ Convex Hull<br/>Polígono que envuelve<br/>todos los puntos"]
```

### 2.2. SAT (Separating Axis Theorem)

**Teorema:** Dos formas convexas NO colisionan si existe un eje en el que sus proyecciones no se solapan.

```mermaid
flowchart LR
    subgraph "SAT - Sin colisión"
        A1["📦 A"] --> PROJ1["Proyección A<br/>▓▓▓▓▓"]
        B1["📦 B"] --> PROJ2["Proyección B<br/>       ▓▓▓▓▓"]
        PROJ1 --> GAP["⚠️ Hueco → no colisión"]
        PROJ2 --> GAP
    end
    
    subgraph "SAT - Colisión"
        A2["📦 A"] --> PROJ3["▓▓▓▓▓▓▓▓▓"]
        B2["📦 B"] --> PROJ4["   ▓▓▓▓▓▓"]
        PROJ3 --> OVERLAP["💥 Solapan → colisión"]
        PROJ4 --> OVERLAP
    end
```

**Algoritmo:**

```
1. Obtener todos los ejes posibles de prueba:
   - Para AABB: ejes X, Y, Z (3 ejes)
   - Para OBB: las normales de las caras + productos cruz (15 ejes)
   
2. Para cada eje:
   - Proyectar ambas formas
   - Si las proyecciones NO se solapan → no hay colisión (salir)

3. Si todos los ejes muestran solapamiento → hay colisión
```

```
// SAT 2D simplificado (polígonos convexos)
bool SAT(Polygon a, Polygon b) {
    vector<vec2> axes = getAxes(a) + getAxes(b);
    
    for (vec2 axis : axes) {
        Projection pa = project(a, axis);
        Projection pb = project(b, axis);
        
        if (pa.max < pb.min || pb.max < pa.min)
            return false;  // Eje separador encontrado
    }
    
    return true;  // Colisión
}
```

| Formas | Ejes a probar | Complejidad |
|---|---|---|
| AABB vs AABB | 3 (X, Y, Z) | Muy rápida |
| OBB vs OBB | 15 | Rápida |
| Convexo 3D vs Convexo 3D | Muchos (caras + aristas) | Media |

### 2.3. GJK (Gilbert-Johnson-Keerthi)

Algoritmo más eficiente que SAT para formas convexas 3D. Usa **diferencias de Minkowski**.

**Diferencia de Minkowski:**

```
A ⊖ B = {a - b : a ∈ A, b ∈ B}

Si A y B colisionan → el origen (0,0,0) está dentro de A ⊖ B
```

```mermaid
flowchart LR
    A3["🟦 Forma A"] --> MINK["Diferencia de Minkowski"]
    B3["🟥 Forma B"] --> MINK
    MINK --> RESULT["⬡ A ⊖ B"]
    RESULT --> CHECK{"¿Origen dentro?"}
    CHECK -->|"Sí"| COLL2["💥 Colisión"]
    CHECK -->|"No"| NOCOLL["✅ No colisión"]
```

**Algoritmo GJK:**

```
1. Inicializar simplex (punto, línea, triángulo, tetraedro)
2. Elegir punto inicial en la diferencia de Minkowski
3. Buscar punto en dirección al origen
4. Si no se puede encontrar punto más cercano → no colisión
5. Si el simplex contiene el origen → colisión
```

```mermaid
flowchart TD
    GJK2["GJK Loop"] --> SUPPORT["Support function<br/>Encontrar punto más lejano<br/>en dirección d"]
    SUPPORT --> SIMPLEX["Actualizar simplex"]
    SIMPLEX --> CONTAINS{"¿Origen en simplex?"}
    CONTAINS -->|"Sí"| COLL3["💥 Colisión"]
    CONTAINS -->|"No"| NEXT_DIR{"¿Nueva dirección?"}
    NEXT_DIR -->|"Sí"| SUPPORT
    NEXT_DIR -->|"No"| NOCOLL2["✅ No colisión"]
```

```
// Support function: punto más lejano en dirección d
vec3 support(Shape a, Shape b, vec3 d) {
    return a.getFarthestPoint(d) - b.getFarthestPoint(-d);
}

// GJK simplificado
bool GJK(Shape a, Shape b) {
    vec3 d = a.center - b.center;  // Dirección inicial
    Simplex simplex;
    simplex.add(support(a, b, d));
    d = -d;
    
    for (int i = 0; i < maxIterations; i++) {
        vec3 p = support(a, b, d);
        if (dot(p, d) < 0) return false;  // No colisión
        simplex.add(p);
        if (simplex.containsOrigin()) return true;  // Colisión
        d = simplex.getClosestEdge();  // Nueva dirección
    }
    return false;
}
```

**Ventajas de GJK sobre SAT:**
- Más eficiente para formas complejas
- Fácil de generalizar a cualquier forma convexa
- Solo necesita la **support function** (no necesita conocer las caras)

### 2.4. EPA (Expanding Polytope Algorithm)

**Extiende GJK** para obtener la **profundidad de penetración** y la **normal de contacto**.

```
1. Después de que GJK encuentra colisión, EPA expande el simplex
2. Busca el punto más cercano al origen en la frontera de A ⊖ B
3. La distancia del origen a la frontera = profundidad de penetración
4. La dirección desde el origen a ese punto = normal de contacto
```

```mermaid
flowchart LR
    GJK4["GJK: colisión detectada"] --> EPA2["EPA: Expandir polítopo"]
    EPA2 --> EDGE["Encontrar cara más cercana al origen"]
    EDGE --> SUPPORT2["Support en dirección de esa cara"]
    SUPPORT2 --> CLOSER{"¿Punto más cercano?"}
    CLOSER -->|"No"| EDGE
    CLOSER -->|"Sí"| RESULT2["Profundidad: d<br/>Normal: n<br/>Punto: p"]
```

**Salida de Narrow Phase (para resolver colisión):**

```
Contact {
    point:    vec3    // Punto de contacto en el mundo
    normal:   vec3    // Normal de la colisión
    depth:    float   // Profundidad de penetración
}
```

---

## 3. CCD (Continuous Collision Detection)

### 3.1. El problema de la "tunelización"

Cuando un objeto se mueve rápido, en un frame puede estar **antes de una pared** y en el siguiente **después**, atravesándola.

```mermaid
flowchart LR
    F1["Frame 1<br/>🔵 Pelota antes"] --> F2["Frame 2<br/>🔵 Pelota después<br/>❌ Atravesó la pared"]
    F1 --- WALL["🧱 Pared"]
    F2 --- WALL
```

### 3.2. Solución CCD

En lugar de solo probar la posición final, CCD prueba el **volumen barrido** entre la posición anterior y la actual.

```mermaid
flowchart LR
    PREV["Posición anterior"] --> SWEPT["Volumen barrido"]
    SWEPT --> CURRENT["Posición actual"]
    SWEPT --> HIT["💥 Encuentra la pared<br/>en el camino"]
```

**Tipos de CCD:**

| Técnica | Cómo funciona |
|---|---|
| **Swept AABB** | Barrer AABB desde posición anterior a actual |
| **Ray Cast + expansion** | Disparar rayo desde centro + radio |
| **Speculative CCD** | Calcular TOI (Time of Impact) |
| **Subdivisión temporal** | Múltiples sub-pasos de física |

```
// CCD con ray cast
bool CCD(Sphere sphere, vec3 from, vec3 to, World world) {
    vec3 direction = to - from;
    float distance = length(direction);
    direction /= distance;
    
    Ray ray = {from, direction};
    Hit hit = world.raycast(ray);
    
    if (hit && hit.distance < distance + sphere.radius) {
        // Colisión en hit.distance
        return true;
    }
    return false;
}
```

---

## 4. Pipeline completo de detección

```mermaid
flowchart TD
    UPDATE["Frame update"] --> BROAD3["FASE AMPLIA"]
    BROAD3 --> SP["Sweep & Prune<br/>o BVH / DBVT"]
    SP --> PAIRS3["Pares potenciales"]
    
    PAIRS3 --> NARROW3["FASE ESTRECHA"]
    NARROW3 --> CONV{"¿Formas convexas?"}
    CONV -->|"Sí"| GJK3["GJK → ¿Colisión?"]
    CONV -->|"No"| DECOMP2["Descomponer en<br/>convexos o SAT-2D"]
    GJK3 -->|"Sí"| EPA3["EPA: profundidad"]
    GJK3 -->|"No"| NEXT["Siguiente par"]
    
    EPA3 --> CONTACT3["Contacto: punto, normal, profundidad"]
    CONTACT3 --> CCD2{"¿Objeto rápido?"}
    CCD2 -->|"Sí"| CCD3["CCD: verificar<br/>tunelización"]
    CCD2 -->|"No"| RESOLVE2["Resolver física"]
    CCD3 --> RESOLVE2
```

### Complejidad computacional

| Fase | Algoritmo | Complejidad |
|---|---|---|
| Broad | Sweep & Prune | O(n log n) |
| Broad | BVH/DBVT | O(log n) por objeto |
| Broad | Spatial Hashing | O(n) promedio |
| Narrow | SAT (AABB) | O(1) por par |
| Narrow | SAT (OBB) | O(1) por par |
| Narrow | GJK | O(log n) iteraciones |
| Narrow | EPA | O(n) iteraciones |
| CCD | Swept | O(n) por objeto |

> 💡 **Regla de oro:** Nunca pruebes todos los pares (O(n²)). Usa **Broad Phase** primero (Sweep & Prune o BVH). Para Narrow Phase, **GJK + EPA** es el estándar moderno para formas convexas. Activa **CCD** solo en objetos que se mueven rápido (balas, coches) para evitar tunelización.

## Relacionados:
- [[matemáticas-para-juegos]] #anterior 
- [[fisicas-dinamicas]] #siguiente 