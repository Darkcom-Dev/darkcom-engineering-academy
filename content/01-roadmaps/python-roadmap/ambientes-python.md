# Ambientes (Environments)

Los **ambientes virtuales** aíslan las dependencias de cada proyecto, evitando conflictos entre versiones.

```mermaid
flowchart LR
    SISTEMA["🐍 Python sistema\n3.11.0\ndjango==4.2\nflask==2.0"] --> PROY1["📁 Proyecto A\nFlask 2.0\nRequests 2.28"]
    SISTEMA --> PROY2["📁 Proyecto B\nDjango 4.2\nPandas 1.5"]
    SISTEMA --> PROY3["📁 Proyecto C\nFlask 3.0\nRequests 2.31"]

    PROY1 -.-> CONFLICTO["❌ Conflicto:\nFlask 2.0 vs 3.0"]
```

```mermaid
flowchart LR
    PROY1_VENV["📁 Proyecto A\n.venv/\nFlask 2.0"] --> ENV1["🔒 Aislado"]
    PROY2_VENV["📁 Proyecto B\n.venv/\nDjango 4.2"] --> ENV2["🔒 Aislado"]
    PROY3_VENV["📁 Proyecto C\n.venv/\nFlask 3.0"] --> ENV3["🔒 Aislado"]

    ENV1 -.-> OK1["✅ Sin conflictos"]
    ENV2 -.-> OK2["✅ Sin conflictos"]
    ENV3 -.-> OK3["✅ Sin conflictos"]
```

---

## Virtualenv / venv

`venv` es el módulo built-in de Python (v3.3+) para crear ambientes virtuales.

```bash
# Crear ambiente
python -m venv .venv

# Activar (Linux/Mac)
source .venv/bin/activate

# Activar (Windows)
.venv\Scripts\activate

# Desactivar
deactivate

# Eliminar (solo borrar directorio)
rm -rf .venv
```

**Flujo de trabajo:**

```bash
# 1. Crear proyecto
mkdir mi-proyecto && cd mi-proyecto

# 2. Crear ambiente
python -m venv .venv

# 3. Activar
source .venv/bin/activate

# 4. Instalar dependencias
pip install flask requests

# 5. Congelar dependencias
pip freeze > requirements.txt

# 6. En otro equipo / CI
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

```mermaid
flowchart TD
    CREAR["python -m venv .venv"] --> ESTRUCTURA["Estructura .venv/"]
    ESTRUCTURA --> BIN["bin/\nactivate, python, pip"]
    ESTRUCTURA --> LIB["lib/python3.11/site-packages/\n(paquetes instalados)"]
    ESTRUCTURA --> PYZ["pyvenv.cfg\n(configuración)"]

    ACTIVAR["source .venv/bin/activate"] --> PATH["PATH modificado\n.pvenv/bin/python\nusa el del ambiente"]
```

---

## Pipenv

Combina `pip` + `virtualenv` + `Pipfile`. Gestiona dependencias y el ambiente automáticamente.

```bash
# Instalar
pip install pipenv

# Crear proyecto e instalar
pipenv install flask        # instala + crea .venv automático
pipenv install --dev pytest # dependencia de desarrollo

# Activar ambiente
pipenv shell

# Ejecutar comando en el ambiente sin activar
pipenv run python main.py

# Instalar desde Pipfile.lock (reproducible)
pipenv install --deploy

# Gráfico de dependencias
pipenv graph
```

**Archivos generados:**
- `Pipfile` — dependencias (como requirements.txt)
- `Pipfile.lock` — versiones exactas + hashes (reproducible)

```toml
# Pipfile
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true

[packages]
flask = "*"
requests = ">=2.28"

[dev-packages]
pytest = "*"
black = "*"

[requires]
python_version = "3.11"
```

```mermaid
flowchart LR
    PIPFILE["Pipfile\nflask = '*'"] --> LOCK["Pipfile.lock\nflask==3.0.1\nhash=abc123"]
    LOCK --> INSTALL["pipenv install\n(usa lock primero)"]
    INSTALL --> VENV[".venv creado\nautomáticamente"]
```

---

## pyenv

Gestiona **múltiples versiones de Python** en el sistema, permitiendo cambiar entre 3.9, 3.10, 3.11, etc.

```bash
# Instalar pyenv
curl https://pyenv.run | bash

# Listar versiones disponibles
pyenv install --list

# Instalar versiones específicas
pyenv install 3.9.18
pyenv install 3.11.7
pyenv install 3.12.1

# Ver instaladas
pyenv versions

# Global (default)
pyenv global 3.11.7

# Local (proyecto actual → crea .python-version)
cd mi-proyecto
pyenv local 3.9.18   # .python-version contiene "3.9.18"

# Shell (sesión actual)
pyenv shell 3.12.1

# Dónde está el ejecutable
pyenv which python
```

```mermaid
flowchart TD
    PYENV["🐍 pyenv"] --> GLOBAL["global\n~/.pyenv/versions/3.11.7/bin/python"]
    PYENV --> LOCAL["local\n.python-version\nversión por proyecto"]
    PYENV --> SHELL["shell\nvariable PYENV_VERSION\nversión por sesión"]

    ORDEN["Orden de precedencia"] --> SHELL2["1. shell"]
    ORDEN --> LOCAL2["2. local (.python-version)"]
    ORDEN --> GLOBAL2["3. global"]
```

### pyenv + virtualenv

El plugin `pyenv-virtualenv` permite crear ambientes virtuales nombrados con diferentes versiones de Python.

```bash
# Instalar pyenv-virtualenv (plugin incluido en pyenv-installer)

# Crear ambiente con Python específico
pyenv virtualenv 3.11.7 mi-proyecto-311

# Activar
pyenv activate mi-proyecto-311

# Listar ambientes
pyenv virtualenvs

# Eliminar
pyenv uninstall mi-proyecto-311
```

---

## Tabla comparativa

| Herramienta | Propósito | Archivos | Activación |
|---|---|---|---|
| **venv** | Ambiente aislado (built-in) | `.venv/` | `source .venv/bin/activate` |
| **virtualenv** | Ambiente aislado (terceros) | `venv/` | `source venv/bin/activate` |
| **pipenv** | Ambiente + dependencias | `Pipfile`, `Pipfile.lock` | `pipenv shell` |
| **pyenv** | Múltiples versiones Python | `.python-version` | `pyenv local 3.11` |
| **pyenv-virtualenv** | Ambientes + versiones | `~/.pyenv/versions/` | `pyenv activate nombre` |
| **Poetry** | Ambiente + deps + build | `pyproject.toml`, `poetry.lock` | `poetry shell` |
| **PDM** | Ambiente + deps (PEP 582) | `pyproject.toml`, `__pypackages__/` | Auto (sin activate) |

---

## Flujo de trabajo completo

```mermaid
flowchart TD
    PROY["Nuevo proyecto"] --> PYTHON["¿Qué versión de Python?"]
    PYTHON --> PYENV2["pyenv local 3.11.7\n(especificar versión)"]
    PYENV2 --> VENV2["python -m venv .venv\n(crear ambiente)"]
    VENV2 --> ACTIVATE["source .venv/bin/activate\n(activar)"]
    ACTIVATE --> DEPS["pip install -r requirements.txt\n(instalar deps)"]
    DEPS --> WORK["💻 Trabajar en el proyecto"]
    WORK --> FREEZE["pip freeze > requirements.txt\n(congelar cambios)"]
```

### Script rápido (setup.sh)

```bash
#!/bin/bash
# setup.sh - Configurar proyecto Python

PROYECTO=${1:-.}

cd "$PROYECTO"

# Crear ambiente
python -m venv .venv

# Activar
source .venv/bin/activate

# Actualizar pip
pip install --upgrade pip

# Instalar dependencias si existe requirements.txt
if [ -f requirements.txt ]; then
    pip install -r requirements.txt
fi

# Instalar dev extras si existe requirements-dev.txt
if [ -f requirements-dev.txt ]; then
    pip install -r requirements-dev.txt
fi

echo "✅ Ambiente listo en .venv/"
echo "📌 Activar con: source .venv/bin/activate"
```

---

## Buenas prácticas

```bash
# ✅ .gitignore: ignorar ambientes
echo ".venv/" >> .gitignore
echo ".python-version" >> .gitignore

# ✅ requirements.txt: siempre actualizado
pip freeze > requirements.txt

# ✅ Usar versión explícita de Python
pyenv local 3.11.7
echo "3.11.7" > .python-version

# ✅ Desactivar antes de borrar
deactivate
rm -rf .venv

# ❌ No instalar paquetes globalmente (contamina el sistema)
# pip install flask  ← si no estás en ambiente

# ❌ No commitear el directorio .venv/
```

### requirements.txt vs pyproject.toml

```text
# requirements.txt (simple, plano)
flask==3.0.1
requests>=2.28.0
pytest; extra == "dev"
```

```toml
# pyproject.toml (moderno, estructurado)
[project]
name = "mi-proyecto"
version = "0.1.0"
dependencies = [
    "flask>=3.0",
    "requests>=2.28",
]

[project.optional-dependencies]
dev = ["pytest", "black"]
```

---

## Resumen

```mermaid
flowchart TD
    AMBIENTES["Herramientas de ambientes Python"] --> VENV2["🟢 venv\nBuilt-in, simple\npython -m venv .venv"]
    AMBIENTES --> PYENV2["🟡 pyenv\nMúltiples versiones\npython 3.9 → 3.12"]
    AMBIENTES --> PIPENV2["🔵 pipenv\nAmbiente + deps\nPipfile + Pipfile.lock"]
    AMBIENTES --> POETRY2["🟣 Poetry\nModerna, completa\npyproject.toml"]

    RECOM["Recomendación"] --> RECOM_VENV["Equipos pequeños: venv + pip\n(simple, universal)"]
    RECOM --> RECOM_POETRY["Proyectos profesionales: Poetry\n(reproducible, build, publish)"]
    RECOM --> RECOM_PIPENV["Alternativa: pipenv\n(simple, Pipfile)"]
    RECOM --> RECOM_PYENV["Siempre: pyenv\n(para gestionar versiones de Python)"]
```
