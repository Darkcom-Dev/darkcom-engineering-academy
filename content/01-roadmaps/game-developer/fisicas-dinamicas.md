# Físicas de Juegos: Dinámicas

## ¿Por qué física en juegos?

La física da **credibilidad** al mundo del juego. Una roca que cae, un coche que derrapa, un personaje que salta... sin física, todo parece flotar.

```mermaid
flowchart LR
    NO_PHYS["❌ Sin física<br/>Movimiento rígido<br/>Irreal"] --> PHYS["✅ Con física<br/>Movimiento natural<br/>Creíble"]
```

### Lo que simula una física de juegos

```mermaid
flowchart TD
    PHYS2[Física de juegos] --> DYN["Dinámica<br/>Fuerzas, movimiento,<br/>colisiones"]
    PHYS2 --> KIN["Cinemática<br/>Posición, velocidad,<br/>aceleración (sin masa)"]
    PHYS2 --> STAT["Estática<br/>Equilibrio, reposo"]
    PHYS2 --> FLUID["Fluidos<br/>Flotabilidad,<br/>viscosidad"]
```

---

## 1. Velocidad Lineal

**Velocidad lineal** = tasa de cambio de la posición en el tiempo.

```
v = Δx / Δt

v = velocidad (m/s)
Δx = cambio de posición
Δt = cambio de tiempo
```

### En el game loop

```
// Velocidad constante
posicion += velocidad * deltaTime;

// Con aceleración
velocidad += aceleracion * deltaTime;
posicion += velocidad * deltaTime;
```

```mermaid
flowchart LR
    P0["x = 0m"] -->|"v = 5m/s, t = 2s"| P1["x = 10m"]
```

---

## 2. Aceleración

**Aceleración** = tasa de cambio de la velocidad en el tiempo.

```
a = Δv / Δt = F / m  (Segunda Ley de Newton)

a = aceleración (m/s²)
F = fuerza (N)
m = masa (kg)
```

```mermaid
flowchart LR
    FORCE["Fuerza F"] --> ACCEL["a = F/m<br/>Aceleración"]
    ACCEL --> VEL["v += a·Δt<br/>Velocidad"]
    VEL --> POS["x += v·Δt<br/>Posición"]
```

### Integración de Euler (la más simple)

```
// Euler explícito (usado en motores)
void update(float dt) {
    acceleration = force / mass;
    velocity += acceleration * dt;
    position += velocity * dt;
}
```

### Integración de Verlet (más estable)

```
// Verlet: usa posición anterior en lugar de velocidad
void update(float dt) {
    vec3 temp = position;
    position = 2 * position - prevPosition + acceleration * dt * dt;
    prevPosition = temp;
}
```

| Método | Precisión | Estabilidad | Uso |
|---|---|---|---|
| **Euler explícito** | Baja | Baja | Juegos casuales |
| **Semi-implicito Euler** | Media | Media | Estándar en juegos |
| **Verlet** | Alta | Alta | Simulaciones de partículas |
| **Runge-Kutta 4 (RK4)** | Muy alta | Muy alta | Simulación científica |

---

## 3. Fuerzas

**Segunda Ley de Newton:** `F = m · a`

```mermaid
flowchart TD
    FORCES[Fuerzas comunes en juegos] --> G["Gravedad<br/>F = m·g<br/>hacia abajo"]
    FORCES --> N["Normal<br/>Contra superficie<br/>perpendicular"]
    FORCES --> FRIC["Fricción<br/>Oponerse al movimiento<br/>paralela"]
    FORCES --> DRAG["Drag / Resistencia<br/>Oponerse a la velocidad<br/>F = -k·v"]
    FORCES --> THR["Thrust / Empuje<br/>Motor, salto,<br/>propulsión"]
    FORCES --> SPRING["Spring / Resortes<br/>F = -k·x<br/>(Ley de Hooke)"]
```

### 3.1. Gravedad

```
// La fuerza más común
vec3 gravity = vec3(0, -9.81, 0);  // m/s² en la Tierra
// En juegos: 9.81 se siente realista, 15-20 se siente "arcade"

// Aplicar gravedad
rigidbody.addForce(gravity * rigidbody.mass);
```

```mermaid
flowchart LR
    BALL["🏀 Pelota"] --> GDOWN["⬇️ F = m·g<br/>Gravedad"]
    GDOWN --> GROUND["🌍 Suelo"]
    GROUND --> UP["⬆️ F = -m·g<br/>Normal (equilibrio en reposo)"]
```

### 3.2. Fuerza de resorte (Hooke)

```
F = -k · (x - x₀)

k = constante del resorte
x = posición actual
x₀ = posición de equilibrio
```

```mermaid
flowchart LR
    WALL["🧱 Pared"] --> SPR["🔵 Resorte<br/>k = 100"]
    SPR --> MASS["🟡 Masa"]
    MASS -->|"Estira"| SPR["F = -k·x<br/>tira hacia atrás"]
```

### 3.3. Fuerza centrípeta (para órbitas/giros)

```
Fc = m · v² / r

v = velocidad tangencial
r = radio de curvatura
```

**Aplicación:** Coches girando, planetas orbitando, loopings.

---

## 4. Centro de Masa

El **centro de masa** (CM) es el punto donde se concentra toda la masa del objeto. Es el punto de aplicación de fuerzas como la gravedad.

```mermaid
flowchart LR
    subgraph "Centro de Masa"
        BOX["🟦 Objeto irregular"] --> CM["⚫ CM<br/>Punto de equilibrio"]
    end
```

```
// Para un sistema de partículas:
CM = Σ(mᵢ · rᵢ) / Σ(mᵢ)

mᵢ = masa de la partícula i
rᵢ = posición de la partícula i

// Objeto uniforme:
CM = centro geométrico
```

**Importancia en juegos:**
- El objeto rota alrededor de su CM
- La gravedad actúa en el CM
- Las colisiones generan torque alrededor del CM

---

## 5. Momento de Inercia

**Momento de inercia (I)** = la "masa rotacional". Mide qué tan difícil es rotar un objeto.

```mermaid
flowchart LR
    MASA["Masa (m)<br/>Resistencia a la traslación"] --> PARALLEL["⬅️⬆️⬇️➡️"]
    INERCIA["Momento de inercia (I)<br/>Resistencia a la rotación"] --> ROT["🔄 Rotación"]
```

```
// Para un objeto puntual:
I = m · r²

// Cilindro sólido:
I = ½ · m · r²

// Esfera sólida:
I = ⅖ · m · r²

// La inercia determina cómo responde un objeto a torques:
τ = I · α    (análogo a F = m·a)
τ = torque
α = aceleración angular
```

```mermaid
flowchart TD
    subgraph "Momento de inercia"
        BARRA["Barra larga<br/>I grande<br/>(difícil de rotar)"] --> SPIN["🔄"]
        ESFERA["Esfera compacta<br/>I pequeña<br/>(fácil de rotar)"] --> SPIN2["🔄"]
    end
```

**En motores de juegos:** La inercia se calcula automáticamente a partir de la forma del collider.

---

## 6. Velocidad Angular

**Velocidad angular (ω)** = qué tan rápido rota un objeto (radianes/segundo o grados/segundo).

```
ω = Δθ / Δt

θ = ángulo de rotación
ω = velocidad angular

Relación con velocidad lineal:
v = ω × r   (producto cruz)
v = velocidad lineal en un punto a distancia r del eje
```

```
// Actualizar rotación
angularVelocity += angularAcceleration * dt;
rotation += angularVelocity * dt;

// Velocidad lineal de un punto en el cuerpo
vec3 pointVelocity = velocity + cross(angularVelocity, point - centerOfMass);
```

**Ejemplo rueda de coche:**

```mermaid
flowchart LR
    WHEEL["🚗 Rueda"] --> WVEL["ω = 30 rad/s<br/>≈ 5 revoluciones/s"]
    WVEL --> CARVEL["v = ω · r<br/>v = 30 · 0.3 = 9 m/s"]
```

---

## 7. Momento Lineal (Momentum)

**Momento lineal (p)** = cantidad de movimiento.

```
p = m · v

Principio de conservación:
En un sistema cerrado, p_total se conserva.
```

**Colisiones:**

```
// Colisión elástica (conserva energía)
m₁·v₁ + m₂·v₂ = m₁·v₁' + m₂·v₂'

// Colisión inelástica (pierde energía, objetos se pegan)
v' = (m₁·v₁ + m₂·v₂) / (m₁ + m₂)
```

---

## 8. Restitución (Rebote)

**Coeficiente de restitución (e)** = qué tanto rebota un objeto.

```
e = velocidad_relativa_después / velocidad_relativa_antes

e = 1.0 → Colisión perfectamente elástica (rebota al 100%)
e = 0.0 → Colisión perfectamente inelástica (no rebota, se pega)
e = 0.5 → Rebota al 50%
```

```mermaid
flowchart TD
    subgraph "Restitución"
        E1["e = 1.0<br/>🏀 Pelota de goma<br/>Rebota casi igual"] --> BOUNCE1["⬇️⬆️⬇️⬆️"]
        E05["e = 0.3<br/>⚽ Balón de fútbol"] --> BOUNCE2["⬇️⬆️⬇️"]
        E0["e = 0.0<br/>🪨 Roca en barro"] --> BOUNCE3["⬇️❌"]
    end
```

```
// Impulso en colisión
vec3 relativeVelocity = velocityA - velocityB;
float normalVelocity = dot(relativeVelocity, collisionNormal);

// Aplicar restitución
float j = -(1 + restitution) * normalVelocity;
j /= 1/massA + 1/massB;

velocityA += j * collisionNormal / massA;
velocityB -= j * collisionNormal / massB;
```

**Materiales comunes:**

| Material | Restitución |
|---|---|
| Goma dura | 0.95 |
| Acero | 0.90 |
| Madera | 0.50 |
| Plomo | 0.20 |
| Barro | 0.00 |

---

## 9. Fricción

**Fricción** = fuerza que se opone al movimiento entre dos superficies en contacto.

```mermaid
flowchart TD
    FRIC2[Fricción] --> EST["Estática<br/>Objeto quieto<br/>μₛ"]
    FRIC2 --> DIN["Dinámica/Cinética<br/>Objeto en movimiento<br/>μₖ"]
```

### Fricción Estática vs Dinámica

```
Fricción estática: mantiene el objeto quieto (μₛ > μₖ)
Fricción dinámica: se opone al movimiento (μₖ)

μₛ > μₖ  → Cuesta más empezar a mover que mantener el movimiento
```

**Leyes de Coulomb para fricción:**

```
F_fricción ≤ μ · N

μ = coeficiente de fricción
N = fuerza normal (perpendicular a la superficie)

// Fricción dinámica (se aplica cuando hay movimiento relativo)
F_friccion = μₖ · N · -v̂

// Fricción estática (se aplica cuando no hay movimiento)
Si |F_aplicada| ≤ μₛ·N → no se mueve
```

```mermaid
flowchart LR
    BOX2["📦 Caja"] --> PUSH["👉 Fuerza aplicada"]
    BOX2 --> FRIC3["◀️ Fricción opuesta"]
    
    PUSH --> MOVES{"¿F > μₛ·N?"}
    MOVES -->|"No"| STILL["Quieto 😐"]
    MOVES -->|"Sí"| MOVING["Se mueve 😊<br/>Fricción = μₖ·N"]
```

**Coeficientes típicos:**

| Superficies | μₛ | μₖ |
|---|---|---|
| Hielo-hielo | 0.1 | 0.03 |
| Madera-madera | 0.5 | 0.3 |
| Goma-asfalto | 1.0 | 0.8 |
| Metal-metal (seco) | 0.6 | 0.4 |

---

## 10. Juntas (Constraints/Joints)

Las **juntas** conectan dos cuerpos rígidos restringiendo su movimiento relativo.

```mermaid
flowchart TD
    JOINTS[Juntas/Joints] --> HINGE["Bisagra (Hinge)<br/>Rota en un eje<br/>🔄 Puerta, codo"]
    JOINTS --> BALL["Rótula (Ball)<br/>Rota libre en 3 ejes<br/>🔮 Hombro, cámara"]
    JOINTS --> SLIDER["Deslizadera (Slider)<br/>Solo se mueve en un eje<br/>📊 Pistón, ascensor"]
    JOINTS --> FIXED["Fija (Fixed)<br/>Sin movimiento relativo<br/>🔒 Partes soldadas"]
    JOINTS --> SPRING2["Resorte (Spring)<br/>Fuerza elástica<br/>🔵 Suspensión"]
```

### 10.1. Hinge Joint (Bisagra)

```mermaid
flowchart LR
    WALL2["🧱 Pared"] --> HINGE2["🔩 Hinge Joint"]
    HINGE2 --> DOOR["🚪 Puerta<br/>(solo rota en Y)"]
```

**Propiedades:** `anchor, axis, limits (min/max angle), motor (velocity, force)`

### 10.2. Ragdoll (combinación de joints)

```mermaid
flowchart TD
    HIPS["Pelvis (root)"] --> SPINE2["Columna"]
    SPINE2 --> HEAD2["Cabeza (ball)"]
    SPINE2 --> L_SHOULDER["Hombro Izq (ball)"]
    L_SHOULDER --> L_ELBOW["Codo Izq (hinge)"]
    L_ELBOW --> L_HAND2["Mano Izq"]
    SPINE2 --> R_SHOULDER["Hombro Der (ball)"]
    HIPS --> L_HIP["Cadera Izq (ball)"]
    L_HIP --> L_KNEE["Rodilla Izq (hinge)"]
    L_KNEE --> L_FOOT2["Pie Izq"]
```

### 10.3. Ecuación de restricción (Constraint)

```
C(x) = 0   // Ecuación de restricción general

// Para un hinge: el ángulo relativo está limitado
C = (θ₂ - θ₁) - θ_limite = 0

// Para un punto fijo: los puntos deben coincidir
C = (p₁ + r₁) - (p₂ + r₂) = 0
```

Los motores resuelven estas restricciones con **Lagrange multipliers** o **sequential impulses**.

---

## 11. Flotabilidad (Buoyancy)

**Principio de Arquímedes:** Un cuerpo sumergido en un fluido experimenta un empuje hacia arriba igual al peso del fluido desalojado.

```
F_buoyancy = ρ · V · g

ρ = densidad del fluido (kg/m³)
V = volumen sumergido (m³)
g = gravedad (m/s²)

Si ρ_objeto < ρ_fluido → flota
Si ρ_objeto > ρ_fluido → se hunde
Si ρ_objeto = ρ_fluido → suspensión neutral
```

```mermaid
flowchart LR
    WATER["🌊 Agua<br/>ρ = 1000 kg/m³"] --> WOOD["🪵 Madera<br/>ρ = 600 kg/m³<br/>↗️ Flota"]
    WATER --> ROCK["🪨 Roca<br/>ρ = 2500 kg/m³<br/>⬇️ Se hunde"]
    WATER --> BALLOON["🎈 Burbuja<br/>ρ = 1.2 kg/m³<br/>⬆️ Sube rápido"]
```

```
// Flotabilidad simplificada para juegos
vec3 buoyancyForce(vec3 position, float volume, float fluidDensity) {
    float submergedDepth = fluidSurface - position.y;
    if (submergedDepth <= 0) return vec3(0);  // Fuera del agua
    
    float submergedVolume = min(volume, volume * (submergedDepth / objectHeight));
    float buoyancyForce = fluidDensity * submergedVolume * gravity;
    
    return vec3(0, buoyancyForce, 0);
}

// Con amortiguación (drag en agua)
vec3 waterDrag = -velocity * dragCoefficient * submergedVolume;
```

**Parámetros en juegos:**
```
Agua dulce:   ρ = 1000 kg/m³
Agua salada:  ρ = 1025 kg/m³
Aceite:       ρ = 900 kg/m³
Miel:         ρ = 1400 kg/m³
```

---

## 12. Resumen visual del motor de física

```mermaid
flowchart TD
    UPDATE["Update frame"] --> FORCES2["1. Acumular fuerzas<br/>Gravedad, motores,<br/>resortes, flotabilidad"]
    FORCES2 --> INTEG["2. Integrar movimiento<br/>v += a·Δt<br/>x += v·Δt"]
    INTEG --> BROAD["3. Broad phase<br/>Detección rápida<br/>(pares potenciales)"]
    BROAD --> NARROW["4. Narrow phase<br/>Detección precisa<br/>(SAT, GJK)"]
    NARROW --> RESOLVE["5. Resolver colisiones<br/>Impulsos + fricción"]
    RESOLVE --> CONSTRAINTS["6. Resolver constraints<br/>Joints, contactos"]
    CONSTRAINTS --> UPDATE
```

### Pipeline típico por frame

```
1. Aplicar fuerzas externas (gravedad, viento, motores)
2. Actualizar velocidades (a = F/m, v += a·dt)
3. Actualizar posiciones (x += v·dt) 
4. Detectar colisiones (Broad → Narrow)
5. Generar contactos (punto, normal, penetración)
6. Resolver contactos (impulsos secuenciales)
7. Resolver constraints (joints, motores)
8. Actualizar sleeping (objetos en reposo no se procesan)
```

> 💡 **Regla de oro:** Usa un motor de física existente (PhysX, Bullet, Jolt) a menos que quieras aprender. La física es **difícil** de hacer bien (estabilidad, stacking, sleeping, continuos collision). El game loop típico: fuerzas → integrar → detectar → resolver → repetir.

## Relacionados:
- [[fisica-deteccion de colisiones]] #anterior 
- [[motores-de-juegos]] #siguiente 