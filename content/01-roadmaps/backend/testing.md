# Testing

El **testing** es la práctica de verificar que tu código funciona correctamente. No es opcional: es parte fundamental del desarrollo profesional. Un buen suite de tests te da **confianza** para refactorizar y desplegar.

```mermaid
flowchart TB
    Tests["🧪 Testing"] --> Unit["🔬 Unit Tests<br/>Prueban una función/módulo"]
    Tests --> Integration["🔗 Integration Tests<br/>Prueban componentes juntos"]
    Tests --> Functional["🎯 Functional Tests<br/>Prueban desde el usuario"]
    Tests --> E2E["🌐 End-to-End<br/>Flujo completo real"]
    Tests --> Performance["⚡ Performance Tests<br/>Carga, estrés, picos"]

    style Tests fill:#e1f5fe
    style Unit fill:#c8e6c9
    style Integration fill:#fff3e0
    style Functional fill:#e3f2fd
    style E2E fill:#fce4ec
    style Performance fill:#e8eaf6
```

### La pirámide de tests

```mermaid
flowchart TB
    subgraph Piramide ["🔺 Pirámide de Testing"]
        E2E2["🌐 E2E (pocos)<br/>🧪 Lentos y caros<br/>🎯 Prueban flujos críticos"]
        Integration2["🔗 Integración (algunos)<br/>⚡ Rápidos<br/>🔌 Prueban interacción entre componentes"]
        Unit2["🔬 Unitarios (muchos)<br/>⚡ Muy rápidos<br/>📦 Prueban unidades aisladas"]
    end

    style Piramide fill:#f5f5f5
    style E2E2 fill:#fce4ec
    style Integration2 fill:#fff3e0
    style Unit2 fill:#c8e6c9
```

> **Regla:** Muchos tests unitarios rápidos, algunos de integración, pocos E2E. Invertir la pirámide (muchos E2E, pocos unitarios) = tests lentos y frágiles.

---

## Unit Testing

Un **test unitario** prueba la **unidad más pequeña** de código (una función, un método) de forma **aislada**, sin dependencias externas (BD, APIs, archivos).

```mermaid
flowchart LR
    subgraph UnitTest ["🔬 Test Unitario"]
        Input["📥 Input: sumar(2, 3)"] --> Func["⚙️ Función: sumar(a, b)"]
        Func --> Expected["📤 Expected: 5"]
        Expected --> Assert["✅ Assert: resultado === 5"]
    end

    style UnitTest fill:#c8e6c9
```

### Ejemplo

```javascript
// Código a testear
function calcularTotal(precio, cantidad, descuento = 0) {
    const subtotal = precio * cantidad;
    return subtotal - (subtotal * descuento);
}

// Test unitario (Vitest / Jest)
describe('calcularTotal', () => {
    it('calcula el total sin descuento', () => {
        expect(calcularTotal(100, 2)).toBe(200);
    });

    it('aplica descuento porcentual', () => {
        expect(calcularTotal(100, 2, 0.1)).toBe(180);
    });

    it('devuelve 0 si la cantidad es 0', () => {
        expect(calcularTotal(100, 0)).toBe(0);
    });

    it('maneja descuento del 100%', () => {
        expect(calcularTotal(100, 2, 1)).toBe(0);
    });
});
```

```python
# Python con pytest
def calcular_total(precio, cantidad, descuento=0):
    subtotal = precio * cantidad
    return subtotal - (subtotal * descuento)

def test_sin_descuento():
    assert calcular_total(100, 2) == 200

def test_con_descuento():
    assert calcular_total(100, 2, 0.1) == 180

def test_cantidad_cero():
    assert calcular_total(100, 0) == 0
```

### Características de un buen test unitario

- **Aislado** — no depende de BD, red, archivos
- **Rápido** — milisegundos
- **Determinista** — misma entrada → mismo resultado siempre
- **Una cosa** — prueba un solo comportamiento
- **Legible** — nombre describe qué prueba

### Mocks y Stubs

Para aislar el código, reemplazas dependencias externas con **mocks**.

```javascript
// Mock de la base de datos
const mockDb = {
    query: vi.fn().mockResolvedValue([{ id: 1, nombre: 'Ana' }])
};

// Test sin BD real
it('obtiene usuarios', async () => {
    const usuarios = await getUsuarios(mockDb);
    expect(usuarios).toHaveLength(1);
    expect(mockDb.query).toHaveBeenCalledWith('SELECT * FROM usuarios');
});
```

---

## Integration Testing

Un **test de integración** verifica que **varios componentes funcionan juntos** correctamente: función + BD, API + servicio externo, etc.

```mermaid
flowchart LR
    subgraph IntegrationTest ["🔗 Test de Integración"]
        Request["📤 Request: POST /usuarios"] --> API["🌐 API Router"]
        API --> Controller["🎮 Controller"]
        Controller --> DB["🗄️ Base de Datos real<br/>(test DB)"]
        DB --> Response["📥 Response: 201 Created"]
        Response --> Assert2["✅ Assert: usuario creado en BD"]
    end

    style IntegrationTest fill:#fff3e0
```

### Ejemplo

```javascript
// Test de integración: API + BD (supertest + vitest)
describe('POST /api/usuarios', () => {
    it('crea un usuario y lo persiste', async () => {
        const res = await request(app)
            .post('/api/usuarios')
            .send({ nombre: 'Ana', email: 'ana@email.com' });

        expect(res.status).toBe(201);
        expect(res.body.nombre).toBe('Ana');

        // Verificar que realmente se guardó en BD
        const usuario = await db.query(
            'SELECT * FROM usuarios WHERE email = $1',
            ['ana@email.com']
        );
        expect(usuario.rows).toHaveLength(1);
    });
});
```

### Integration testing con BD de test

```mermaid
flowchart TB
    subgraph EstrategiaBD ["📐 Estrategias para BD en tests"]
        Memory["💾 En memoria<br/>SQLite :memory:<br/>⚡ Rápido<br/>⚠️ No es igual a prod"]
        Container["🐳 Contenedor Docker<br/>PostgreSQL real<br/>✅ Igual que prod<br/>⚠️ Más lento"]
        MockBD["🧪 Mockear BD<br/>No tocar BD real<br/>⚡ Muy rápido<br/>⚠️ No pruebas SQL real"]
    end

    style EstrategiaBD fill:#ffe0b2
    style Memory fill:#c8e6c9
    style Container fill:#e3f2fd
    style MockBD fill:#fff9c4
```

---

## Functional Testing

El **testing funcional** (o de aceptación) verifica el comportamiento desde la **perspectiva del usuario**: dado un input, espero un output específico. No le importa la implementación interna.

```mermaid
flowchart LR
    UserStory["📖 Historia de usuario:<br/>'Como usuario, quiero<br/>registrarme con email'"] --> TestsFunc["🎯 Tests funcionales"]

    TestsFunc --> TF1["✅ Registro exitoso<br/>→ 201 Created"]
    TestsFunc --> TF2["❌ Email duplicado<br/>→ 409 Conflict"]
    TestsFunc --> TF3["❌ Email inválido<br/>→ 400 Bad Request"]
    TestsFunc --> TF4["❌ Campos faltantes<br/>→ 400 Bad Request"]

    style UserStory fill:#e1f5fe
    style TestsFunc fill:#e3f2fd
```

### Ejemplo

```javascript
describe('Registro de usuarios', () => {
    it('registra un usuario con datos válidos', async () => {
        const res = await request(app)
            .post('/api/auth/register')
            .send({
                nombre: 'Ana',
                email: 'ana@email.com',
                password: 'Pass123!'
            });

        expect(res.status).toBe(201);
        expect(res.body).toHaveProperty('token');
    });

    it('rechaza email duplicado', async () => {
        // Primero registrar
        await request(app).post('/api/auth/register')
            .send({ nombre: 'Ana', email: 'ana@email.com', password: 'Pass123!' });

        // Segundo registro con mismo email
        const res = await request(app).post('/api/auth/register')
            .send({ nombre: 'Bob', email: 'ana@email.com', password: 'Pass456!' });

        expect(res.status).toBe(409);
        expect(res.body.error).toContain('ya existe');
    });
});
```

---

## E2E Testing (End-to-End)

Los tests E2E prueban el **flujo completo** del sistema desde la interfaz de usuario hasta la base de datos, simulando un usuario real.

```mermaid
flowchart LR
    User["👤 Usuario"] --> Browser["🌍 Navegador"]
    Browser --> App["📱 App Web"]
    App --> API["🔌 Backend API"]
    API --> DB["🗄️ Base de Datos"]

    subgraph E2ETest ["🌐 Test E2E"]
        T1["Abre la página de login"]
        T2["Escribe email y password"]
        T3["Hace clic en 'Ingresar'"]
        T4["✅ Ve el dashboard"]
    end

    style E2ETest fill:#fce4ec
```

### Herramientas E2E

| Herramienta | Lenguaje      | Ideal para                     |
| ----------- | ------------- | ------------------------------ |
| **Playwright** | JS/TS/Python | Tests modernos, multi-browser |
| **Cypress**    | JS/TS       | Frontend, debug visual        |
| **Selenium**   | Múltiples    | Legacy, compatibilidad máxima |

```javascript
// Test E2E con Playwright
test('usuario puede iniciar sesión', async ({ page }) => {
    await page.goto('/login');
    await page.fill('[name="email"]', 'ana@email.com');
    await page.fill('[name="password"]', 'Pass123!');
    await page.click('button[type="submit"]');

    await expect(page.locator('.dashboard')).toBeVisible();
    await expect(page.locator('.user-name')).toHaveText('Ana');
});
```

---

## Estrategia de testing

```mermaid
flowchart TB
    Proyecto["📁 Proyecto"] --> Estrategia["📐 Definir estrategia"]

    Estrategia --> Critico["🔴 Funcionalidad crítica<br/>Pagos, auth, datos sensibles<br/>→ Unit + Integration + E2E"]
    Estrategia --> Medio["🟡 Funcionalidad media<br/>CRUD, perfiles<br/>→ Unit + Integration"]
    Estrategia --> Bajo["🟢 Funcionalidad baja<br/>UI estática, textos<br/>→ Unit básicos"]

    Proyecto --> Herramientas2["🛠️ Herramientas según lenguaje"]
    Herramientas2 --> JS["JS/TS: Vitest, Playwright, Supertest"]
    Herramientas2 --> Python["Python: pytest, requests, Selenium"]
    Herramientas2 --> Ruby["Ruby: RSpec, Capybara"]
    Herramientas2 --> Java["Java: JUnit, TestNG, Mockito"]

    style Proyecto fill:#e1f5fe
    style Estrategia fill:#fff3e0
    style Critico fill:#ffcdd2
    style Medio fill:#fff9c4
    style Bajo fill:#c8e6c9
    style Herramientas2 fill:#f5f5f5
```

---

## Buenas prácticas

```mermaid
flowchart LR
    subgraph Do ["✅ SÍ hacer"]
        D1["✔️ Escribir tests antes<br/>del código (TDD)"]
        D2["✔️ Tests independientes<br/>(pueden correr en paralelo)"]
        D3["✔️ Nombres descriptivos<br/>'devuelve 404 si<br/>el usuario no existe'"]
        D4["✔️ AAA Pattern<br/>Arrange + Act + Assert"]
        D5["✔️ CI ejecuta tests<br/>en cada push"]
    end

    subgraph Dont ["❌ NO hacer"]
        Dt1["✖️ Tests que dependen<br/>de otros tests"]
        Dt2["✖️ Tests lentos<br/>(>1s por test unitario)"]
        Dt3["✖️ Tests que prueban<br/>cosas obvias (getters)"]
        Dt4["✖️ Ignorar tests rojos"]
        Dt5["✖️ Probar implementación<br/>en vez de comportamiento"]
    end

    style Do fill:#c8e6c9
    style Dont fill:#ffcdd2
```

### El patrón AAA

```javascript
describe('obtenerUsuario', () => {
    it('devuelve el usuario si existe', async () => {
        // ARRANGE: preparar datos
        const usuario = await crearUsuario({ nombre: 'Ana' });

        // ACT: ejecutar la acción
        const resultado = await obtenerUsuario(usuario.id);

        // ASSERT: verificar resultado
        expect(resultado.nombre).toBe('Ana');
    });
});
```

---

## Cobertura de código

La **cobertura** mide qué porcentaje del código es ejecutado por los tests. Útil para encontrar código no probado, pero **no es una meta en sí misma**.

```mermaid
flowchart TB
    Cobertura["📊 Cobertura de código"]

    Cobertura --> Lines["📏 Líneas<br/>% de líneas ejecutadas"]
    Cobertura --> Branches["🔀 Ramas<br/>% de if/else ejecutados"]
    Cobertura --> Funcs["🔤 Funciones<br/>% de funciones llamadas"]

    Cobertura --> Meta["🎯 Meta: 80%+ en lógica crítica<br/>No obsesionarse con 100%"]
    Cobertura --> Danger["⚠️ 100% de cobertura ≠<br/>100% de calidad"]

    style Cobertura fill:#e8eaf6
    style Meta fill:#c8e6c9
    style Danger fill:#ffcdd2
```

---

## Resumen visual

```mermaid
graph TB
    Testing2["🧪 Testing"] --> Unit2["🔬 Unitarios<br/>Función aislada<br/>⚡ Muy rápido"]
    Testing2 --> Integration2["🔗 Integración<br/>Componentes juntos<br/>⚡ Rápido"]
    Testing2 --> Functional2["🎯 Funcional<br/>Comportamiento usuario<br/>⏱️ Medio"]
    Testing2 --> E2E2["🌐 End-to-End<br/>Flujo completo<br/>🐌 Lento"]
    Testing2 --> Performance2["⚡ Performance<br/>Carga y estrés"]

    Testing2 --> CI["⚙️ CI ejecuta tests<br/>en cada push"]
    Testing2 --> Coverage["📊 Cobertura (+80%)"]

    style Testing2 fill:#e1f5fe
    style Unit2 fill:#c8e6c9
    style Integration2 fill:#fff3e0
    style Functional2 fill:#e3f2fd
    style E2E2 fill:#fce4ec
    style Performance2 fill:#e8eaf6
```

---

> **Siguiente paso:** Agrega tests unitarios a una función existente de tu proyecto. Luego un test de integración para un endpoint. Haz que el CI los ejecute automáticamente. La práctica constante es la clave.

## Relacionados:
- [[mas-acerca-de-bases-de-datos]] #anterior 
- [[containerización]]