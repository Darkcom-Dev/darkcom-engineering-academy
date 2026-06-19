---
title: Reemplace el Método con el Objeto del Método
tags:
  - poo
  - refactorizacion
  - composicion-de-metodos
course: curso-python/05-refactor
---

# Reemplace el Método con el Objeto del Método

## Problema

tiene un método largo en el que las variables locales están tan entrelazadas que no se puede aplicar Extraer Método.

## Solución

transforme el método en una clase separada para que las variables locales se conviertan en campos de la clase. Luego, puede dividir el método en varios métodos dentro de la misma clase.
