# Academia JavaScript

Bienvenido a la Academia JS. Este repositorio contiene una ruta de aprendizaje progresiva desde los fundamentos del lenguaje hasta proyectos avanzados y oportunidades profesionales.

## Ruta de Aprendizaje

```mermaid
graph LR
    subgraph "Fundamentos"
        A["01: Sintaxis y Variables<br/>[[01-Fundamentos/Fundamentos-Sintaxis-Variables]]"]
    end
    
    subgraph "Backend"
        B["02: Node.js CLI<br/>[[02-NodeJS-CLI/CLI-Interaccion-Terminal]]"]
    end
    
    subgraph "Datos"
        C["03: Arrays y Objetos<br/>[[03-Estructuras-Datos/Estructuras-Arrays-Objetos]]"]
    end
    
    subgraph "Frontend"
        D["04: HTML5 Semántico<br/>[[04-Web-Semantica/HTML5-Semantico-Accesibilidad]]"]
        E["05: CSS Layout<br/>[[05-Layouts-CSS/CSS-Tecnicas-Layout]]"]
    end
    
    subgraph "Proyectos"
        F["Calculadora Climática<br/>[[06-Proyectos/Calculadora-Climatica-Psicrometrica]]"]
        G["Mundo 3D Three.js<br/>[[06-Proyectos/Mundo-3D-ThreeJS]]"]
    end
    
    subgraph "Carrera"
        H["Oportunidades JS<br/>[[07-Carrera/Oportunidades-Ecosistema-JS]]"]
    end
    
    A --> B
    A --> C
    A --> D
    D --> E
    B --> F
    C --> F
    D --> F
    E --> F
    C --> G
    D --> G
    E --> G
    F --> H
    G --> H
```

## Módulos

| Módulo | Descripción |
| :--- | :--- |
| [[fundamentos-sintaxis-variables\|Fundamentos]] | Sintaxis, variables (`var`/`let`/`const`), template literals |
| [[CLI-interaccion-terminal\|Node.js CLI]] | Aplicaciones de terminal con stdin/stdout y `process.argv` |
| [[estructuras-arrays-objetos\|Estructuras de Datos]] | Arrays, Objetos, métodos de transformación |
| [[HTML5-semantico-accesibilidad\|Web Semántica]] | HTML5 semántico, SVG, `<picture>`, accesibilidad |
| [[CSS-tecnicas-layout\|Layouts CSS]] | Floats, Flexbox, Grid, Media Queries |
| [[calculadora-climatica-psicrometrica\|Calculadora Climática]] | Proyecto: conversión de unidades, presión, punto de rocío |
| [[mundo-3D-ThreeJS\|Mundo 3D Three.js]] | Proyecto: gráficos 3D, sistema solar, animación |
| [[oportunidades-ecosistema-JS\|Carrera JS]] | Oportunidades laborales, ecosistema, recursos |
