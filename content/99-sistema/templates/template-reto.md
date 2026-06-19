---
tipo: reto
dificultad: {{dificultad|default(3)}}
---

# {{emoji}} {{title}}

---

## 🎯 Objetivo

{{objetivo}}

---

## 📚 Conocimientos Previos

{{conocimientos_previos}}

---

## 📖 Contexto

{{contexto}}

{% if diagrama_flujos %}
{% for diagrama in diagrama_flujos %}
```mermaid
{{diagrama}}
```
{% endfor %}
{% endif %}

---

## 🧩 ¿Qué debes implementar?

{{que_implementar}}

{% if firmas_funciones %}
```{{lenguaje}}
{{firmas_funciones}}
```
{% endif %}

{% if tabla_parametros %}
### Parámetros de entrada

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
{% for parametro in tabla_parametros %}
| {{parametro.nombre}} | {{parametro.tipo}} | {{parametro.descripcion}} |
{% endfor %}
{% endif %}

{% if tabla_salida %}
### Formato de salida

```{{lenguaje}}
{{tabla_salida}}
```
{% endif %}

---

## 💡 Sugerencias de Investigación

Para resolver este reto, se recomienda investigar sobre:

{% for sugerencia in sugerencias_investigacion %}
- {{sugerencia}}
{% endfor %}

{% if advertencias %}
## ⚠️ Advertencias

{% for advertencia in advertencias %}
- {{advertencia}}
{% endfor %}
{% endif %}

---