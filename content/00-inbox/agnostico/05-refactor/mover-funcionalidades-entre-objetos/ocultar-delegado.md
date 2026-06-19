---
title: Ocultar Delegado
tags:
  - poo
  - refactorizacion
  - mover-funcionalidades-entre-objetos
course: curso-python/05-refactor
---

# Ocultar Delegado

## Problema
el cliente obtiene el objeto B de un campo o método del objeto A. Luego, el cliente llama a un método del objeto B.

## Solución
cree un nuevo método en la clase A que delegue la llamada al objeto B. Ahora el cliente no sabe sobre la clase B o depende de ella.
