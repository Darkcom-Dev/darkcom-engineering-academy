---
tipo: desafio
dificultad: {{dificultad|default(5)}}
---

# {{emoji}} {{Title}}

## 🎯 Objetivo

{{objetivo}}

---

## 📚 Conocimientos Previos

{{conocimientos_previos}}

---

## 🧩 Funcionalidades

{{funcionalidades}}

{% if endpoints %}
### Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
{% for endpoint in endpoints %}
| {{endpoint.metodo}} | {{endpoint.ruta}} | {{endpoint.descripcion}} |
{% endfor %}
{% endif %}

---

## 🎁 Bonus

{{bonus}}

---

## 💡 Sugerencias de Investigación

Para resolver este desafío, se recomienda investigar sobre:

{% for sugerencia in sugerencias_investigacion %}
- {{sugerencia}}
{% endfor %}

---

## 📖 Documentación

{{documentacion}}

---

## 🧪 Tests (Opcional)

{{tests}}

---

## ✅ Criterios de evaluación

| Criterio | Descripción |
|----------|-------------|
{% for criterio in criterios_evaluacion %}
| {{criterio.nombre}} | {{criterio.descripcion}} |
{% endfor %}

---