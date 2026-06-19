# Motores de Juegos

## ¿Qué es un motor de juegos?

Un **motor de juegos** (game engine) es un framework de software que proporciona las herramientas y abstracciones necesarias para crear videojuegos. Evita tener que reinventar la rueda cada vez que se hace un juego.

```mermaid
flowchart TD
    subgraph "🎮 Lo que el motor te da"
        REND[Renderizado 2D/3D]
        PHYS[Física]
        AUDIO[Audio]
        INPUT[Input Manager]
        UI[Sistema de UI]
        ANIM[Animación]
        NET[Red / Multiplayer]
        SCR[Scripting / Programación]
    end
    
    subgraph "🛠️ Lo que tú pones"
        GAME[Lógica del juego]
        ART[Arte / Assets]
        LEVEL[Diseño de niveles]
        STORY[Historia / Narrativa]
    end
    
    REND --> GAME
    PHYS --> GAME
    AUDIO --> GAME
    INPUT --> GAME
    UI --> GAME
    ANIM --> GAME
    NET --> GAME
    SCR --> GAME
```

### ¿Motor comercial o nativo?

```mermaid
flowchart LR
    subgraph "Usar motor existente"
        PLUS["✅ +Rápido empezar<br/>✅ +Herramientas visuales<br/>✅ +Comunidad<br/>❌ -Menos control<br/>❌ -Pesado"]
    end
    subgraph "Desarrollo nativo"
        MINUS["❌ -Más trabajo<br/>❌ -Desde cero<br/>✅ +Control total<br/>✅ +Optimizado<br/>✅ +Aprendizaje profundo"]
    end
```

---

## 1. Godot

**Lenguaje principal:** GDScript, C#, C++ (GDExtension)
**Tipo:** 2D y 3D
**Licencia:** MIT (100% gratuito y open-source)

### 1.1. Filosofía

Godot usa un sistema de **escenas y nodos**. Todo es un **nodo** organizado en un árbol.

```mermaid
flowchart TD
    subgraph "Árbol de escena (Scene Tree)"
        ROOT["Nodo Raíz<br/>(Node2D / Node3D)"] --> SPRITE["Sprite2D<br/>Renderiza textura"]
        ROOT --> COLL["CollisionShape2D<br/>Colisión"]
        ROOT --> TIMER["Timer<br/>Temporizador"]
        ROOT --> SCRIPT["Script (GDScript)<br/>Lógica del jugador"]
    end
```

### 1.2. Sistema de señales (Signals)

Godot usa un patrón **observer** desacoplado: los nodos emiten señales y otros nodos (o el mismo) las escuchan.

```mermaid
flowchart LR
    OBJ["Jugador.gd<br/>emit_signal('hit')"] --> SIGNAL["Señal: 'hit'"]
    SIGNAL --> HUD["HUD.gd<br/>_on_player_hit()"]
    SIGNAL --> GAME["GameManager.gd<br/>_on_player_hit()"]
```

```
# Jugador.gd
signal hit(damage)  # Definición de la señal

func recibir_golpe(dmg):
    health -= dmg
    hit.emit(dmg)  # Emitir señal

# HUD.gd
func _ready():
    jugador.hit.connect(_on_player_hit)

func _on_player_hit(dmg):
    label.text = "Vida: %d" % jugador.health
```

### 1.3. Escenas como bloques

Todo es una **escena** (archivo `.tscn`) que puede anidarse dentro de otra.

```
Personaje.tscn   →    Jugador.tscn   →   Nivel.tscn
  - Sprite             - Personaje        - Jugador
  - Collision           - Script          - Enemigo
  - Animación           - Cámara          - Escenario
```

### 1.4. Pros y Contras

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Gratuito y open-source (MIT) | Ecosistema más pequeño |
| Ligero (~50MB) | Menos assets en tienda |
| Excelente para 2D | 3D menos maduro que UE/Unity |
| GDScript fácil de aprender | Pocas ofertas laborales |
| Editor integrado muy completo | Menos tutoriales AAA |

**¿Cuándo usarlo?** Juegos 2D, prototipos rápidos, proyectos indie, educación.

---

## 2. Unreal Engine

**Lenguaje principal:** C++, Blueprints (Visual Scripting)
**Tipo:** 3D (también 2D con Paper2D)
**Licencia:** Royalty 5% (gratuito hasta 1M USD)

### 2.1. Filosofía

Unreal está construido para **gráficos AAA**. Su pipeline de renderizado es de los más avanzados del mercado.

```mermaid
flowchart TD
    subgraph "Pipeline de Unreal"
        ACTOR["Actor<br/>Clase base de todo"] --> COMP["Componentes<br/>(Mesh, Collision, Light)"]
        COMP --> BLUEPRINT["Blueprint<br/>Lógica visual"]
        BLUEPRINT --> LEVEL["Level / Mapa"]
        LEVEL --> GAME_MODE["GameMode<br/>Reglas del juego"]
        GAME_MODE --> PC["PlayerController<br/>Input y cámara"]
        PC --> PAWN["Pawn / Character<br/>Avatar del jugador"]
    end
```

### 2.2. Blueprints vs C++

```mermaid
flowchart LR
    subgraph "Blueprints"
        BP["🧩 Visual Scripting<br/>+ Rápido de prototipar<br/>+ Accesible a no-programadores<br/>- Más lento en runtime"]
    end
    subgraph "C++"
        CPP["⚡ Código nativo<br/>+ Máximo rendimiento<br/>+ Control total<br/>- Compilación lenta"]
    end
    
    BP -->|"Se puede llamar"| CPP
    CPP -->|"Se puede exponer a"| BP
```

**Flujo de trabajo típico:** Prototipar en Blueprint → pasar a C++ lo crítico en rendimiento.

### 2.3. Sistema de materiales

Los materiales en Unreal son **node-based** (basados en nodos):

```
Material (shader graph)
  ├── Texture Sample (albedo)
  ├── Texture Sample (normal map)
  ├── Texture Sample (roughness)
  ├── Constant3Vector (color)
  └── → Material Output
```

### 2.4. Pros y Contras

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Renderizado AAA impresionante | Curva de aprendizaje alta |
| Blueprints para no-programadores | Pesado (~100GB con contenido) |
| C++ nativo → máximo control | Compilación lenta |
| Herramientas de cine/animación | Royalty 5% |
| Gran ecosistema (Marketplace, Quixel) | Sobredimensionado para 2D |

**¿Cuándo usarlo?** Juegos 3D AAA, shooters, gráficos realistas, cinemáticas, VR/AR.

---

## 3. Unity 3D

**Lenguaje principal:** C#
**Tipo:** 2D y 3D
**Licencia:** Gratuito (Personal), Pro (pago por asiento)

### 3.1. Filosofía

Unity usa un sistema de **GameObjects y Componentes**. Un GameObject vacío + componentes = cualquier cosa.

```mermaid
flowchart TD
    subgraph "GameObject + Componentes"
        GO["GameObject<br/>Contenedor vacío"]
        GO --> MESH["MeshRenderer<br/>Renderiza el modelo"]
        GO --> COLL["BoxCollider<br/>Colisión"]
        GO --> RB["Rigidbody<br/>Física"]
        GO --> SCRIPT["MonoBehaviour (C#)<br/>Lógica del jugador"]
        GO --> AUD["AudioSource<br/>Sonido"]
    end
```

```
// Ejemplo: mover un GameObject con física
public class PlayerMovement : MonoBehaviour
{
    public float speed = 10f;
    private Rigidbody rb;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void FixedUpdate()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        Vector3 mov = new Vector3(h, 0, v) * speed;
        rb.AddForce(mov);
    }
}
```

### 3.2. Pipeline de renderizado (SRP)

Unity ofrece **pipelines intercambiables**:

```mermaid
flowchart LR
    subgraph "Scriptable Render Pipeline"
        URP["URP<br/>Universal RP<br/>Equilibrio calidad/rendimiento<br/>Móvil y consolas"] --> SRP[Scriptable Render Pipeline]
        HDRP["HDRP<br/>High Definition RP<br/>Calidad AAA, PC/consolas"] --> SRP
        CUSTOM["Custom RP<br/>Hecho a medida"] --> SRP
    end
```

### 3.3. Asset Store y ecosistema

Unity tiene el **Asset Store** más grande del mercado:

```mermaid
flowchart TD
    STORE[Asset Store] --> MODELS[Modelos 3D]
    STORE --> SCRIPTS[Scripts / Herramientas]
    STORE --> FX[Efectos / Shaders]
    STORE --> AUDIO[Audio / Música]
    STORE --> TEMPLATES[Plantillas completas]
```

### 3.4. Pros y Contras

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Versátil (2D y 3D) | Rendimiento 3D < Unreal |
| C# fácil y moderno | Cambios de versión rompen proyectos |
| Asset Store enorme | UI frustrante a veces (aunque mejora) |
| Multiplataforma excelente | Overhead en proyectos grandes |
| Gran comunidad / tutoriales | Política de precios cambió (runtime fee) |

**¿Cuándo usarlo?** Juegos 2D, móviles, prototipos, indie, juegos multiplataforma, AR/VR.

---

## 4. Comparativa general

```mermaid
flowchart TD
    subgraph "Elegir motor según el proyecto"
        Q1["¿Qué tipo de juego?"]
        Q1 -->|"2D"| GODOT["Godot ⭐<br/>o Unity"]
        Q1 -->|"3D realista"| UE["Unreal Engine ⭐"]
        Q1 -->|"3D estilizado/indie"| UNITY["Unity ⭐"]
        Q1 -->|"Móvil"| UNITY2["Unity ⭐<br/>o Godot"]
        Q1 -->|"Prototipo rápido"| GODOT2["Godot ⭐"]
    end
```

| Aspecto | Godot | Unity | Unreal |
|---|---|---|---|
| **Licencia** | MIT (gratis) | Gratis + Pro | Royalty 5% |
| **Lenguaje** | GDScript / C# | C# | C++ / Blueprints |
| **Peso instalación** | ~50 MB | ~5 GB | ~100 GB |
| **2D** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| **3D** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Rendimiento** | Bueno | Bueno | Excelente |
| **Curva aprendizaje** | Baja | Media | Alta |
| **Empleo** | Baja | Alta | Muy alta (AAA) |
| **Comunidad** | Creciendo | Muy grande | Grande |
| **Editor visual** | Completo | Completo | Muy completo |

---

## 5. Desarrollo nativo (sin motor comercial)

También conocido como **"from scratch"** o **"roll your own engine"**. Usas bibliotecas de bajo nivel y construyes todo tú mismo.

### 5.1. Stack típico

```mermaid
flowchart LR
    subgraph "Stack nativo"
        GRA["Graphics API<br/>OpenGL / Vulkan / DirectX"] --> WIN["Window & Input<br/>GLFW / SDL2 / Win32"]
        WIN --> PHYS["Physics<br/>Bullet / PhysX / Jolt"]
        PHYS --> AUDIO["Audio<br/>OpenAL / FMOD / miniaudio"]
        AUDIO --> NET["Network<br/>ENet / RakNet / Steamworks"]
        NET --> GAME["TU CÓDIGO<br/>Game loop, ECS, etc."]
    end
```

### 5.2. Bibliotecas populares por área

| Área | Biblioteca |
|---|---|
| **Gráficos** | OpenGL, Vulkan, DirectX 12, Metal |
| **Ventana/Input** | GLFW, SDL2, SFML |
| **Matemáticas** | GLM (OpenGL Mathematics) |
| **Física** | Bullet Physics, PhysX, Jolt |
| **Audio** | OpenAL, miniaudio, FMOD |
| **Red** | ENet, RakNet, Steamworks SDK |
| **ECS** | EnTT, Flecs |
| **Assets** | Assimp (modelos), stb_image (texturas) |

### 5.3. Arquitectura de motor nativo

```
tu_juego/
├── core/
│   ├── game_loop.cpp      // Fixed step + variable render
│   ├── input_manager.cpp   // Teclado, ratón, mando
│   └── debug.cpp           // Logging, profiling
├── graphics/
│   ├── renderer.cpp        // Pipeline de renderizado
│   ├── shader.cpp          // Compilación de shaders
│   ├── mesh.cpp            // Mallas / VAO / VBO
│   └── camera.cpp          // Frustum, matrices VP
├── physics/
│   └── physics_world.cpp   // Bullet/PhysX wrapper
├── audio/
│   └── audio_engine.cpp    // OpenAL / miniaudio
├── ecs/
│   └── world.cpp           // Sistema ECS (EnTT)
├── assets/
│   └── asset_manager.cpp   // Carga/gestión de recursos
└── main.cpp
```

### 5.4. Pros y Contras

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Control TOTAL del código | Años de desarrollo |
| Sin licencias ni regalías | Sin herramientas visuales |
| Optimizado al máximo | Sin Asset Store |
| Aprendizaje profundo | Tienes que hacer TODO |
| Sin bloatware | Depuración compleja |
| Ideal para estudios técnicos | Equipo grande necesario |

**¿Cuándo usarlo?**
- Cuando quieres **aprender** cómo funcionan los motores por dentro
- Cuando necesitas **rendimiento extremo** (demoscene, juegos retro, experimentos)
- Cuando el motor comercial **no te da el control** que necesitas
- Grandes estudios con equipo de engine dedicado (EA, Riot, Rockstar)

### 5.5. Game Loop nativo mínimo

```
// Mínimo indispensable para un juego nativo
#include <GLFW/glfw3.h>
#include <glm/glm.hpp>

int main() {
    glfwInit();
    GLFWwindow* window = glfwCreateWindow(800, 600, "Mi Juego", NULL, NULL);
    
    double lastTime = glfwGetTime();
    
    while (!glfwWindowShouldClose(window)) {
        double currentTime = glfwGetTime();
        float deltaTime = currentTime - lastTime;
        lastTime = currentTime;
        
        // Input
        if (glfwGetKey(window, GLFW_KEY_W)) moverAdelante(deltaTime);
        if (glfwGetKey(window, GLFW_KEY_ESCAPE)) break;
        
        // Actualizar
        actualizarFisica(deltaTime);
        actualizarIA(deltaTime);
        
        // Renderizar
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        renderizarEscena();
        
        glfwSwapBuffers(window);
        glfwPollEvents();
    }
    
    glfwTerminate();
    return 0;
}
```

---

## 6. Resumen: ¿Qué camino elegir?

```mermaid
flowchart TD
    START["Quiero hacer un juego"] --> Q1{"¿Mi objetivo?"}
    
    Q1 -->|"Hacer un juego RÁPIDO"| COMM["Usa un motor<br/>Godot / Unity / Unreal"]
    Q1 -->|"Aprender programación de juegos"| LEARN["Unity o Godot<br/>+ C# / GDScript"]
    Q1 -->|"Trabajar en AAA"| AAA["Unreal Engine ⭐<br/>Aprende C++ y Blueprints"]
    Q1 -->|"Entender cómo funciona por dentro"| NATIVE["Desarrollo nativo<br/>OpenGL / Vulkan + SDL"]
    Q1 -->|"Hacer dinero (indie)"| INDIE["Unity (más empleo)<br/>o Godot (sin royalties)"]
    
    COMM --> Q2{"¿2D o 3D?"}
    Q2 -->|"2D"| G2[Godot ⭐]
    Q2 -->|"3D realista"| U3[Unreal ⭐]
    Q2 -->|"3D estilizado / móvil"| U2[Unity ⭐]
```

> 💡 **Regla de oro:** No hay motor "mejor". Hay motor **más adecuado** para tu proyecto, tu equipo y tus objetivos. Unreal para AAA gráfico, Unity para versatilidad multiplataforma, Godot para 2D y proyectos libres, nativo para aprendizaje y control extremo.

## Relacionados:
- [[fisicas-dinamicas]] #anterior 
- [[lenguajes-de-programación-para-videojuegos]] #siguiente 