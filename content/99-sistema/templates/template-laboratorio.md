---
tipo: laboratorio
dificultad: {{dificultad|default(1)}}
---

# {{emoji}} Laboratorio: {{title}}

**Elaborado por:** {{autor}} — {{proyecto}}

---

## 🎯 Objetivo

{{objetivo}}

---

## 📚 Conocimientos Previos

{{conocimientos_previos}}

---

## 📖 Contexto

{{contexto}}

---

## 🧪 Actividad: {{nombre_actividad}}

{{descripcion_actividad}}

{% if diagrama_flujo %}
```mermaid
{{diagrama_flujo}}
```
{% endif %}

{% if tabla_errores %}
| Error | Código erroneo | ¿Qué mensaje da Python? |
|-------|----------------|------------------------|
{% for fila in tabla_errores %}
| {{fila.error}} | {{fila.codigo}} | {{fila.mensaje}} |
{% endfor %}
{% endif %}

---

## 🌎 Ejercicio: {{nombre_ejercicio}}

{{descripcion_ejercicio}}

```{{lenguaje}}
{{codigo_inicial}}
```
```

---

## 🤖 Ejercicio: {{nombre_ejercicio2}}

{{descripcion_ejercicio2}}

```{{lenguaje}}
{{codigo_inicial2}}
```
```

---

## 💡 Sugerencias de Investigación

Para comprender mejor los conceptos de este laboratorio, se recomienda investigar sobre:

{% for sugerencia in sugerencias_investigacion %}
- {{sugerencia}}
{% endfor %}

---

## 📝 Entregables

{% for entregable in entregables %}
{{loop.index}}. {{entregable}}
{% endfor %}

---