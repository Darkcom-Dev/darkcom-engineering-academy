# Diálogos del agente Smith

Si alguien no ha visto la trilogía de Matrix, ¿Qué está esperando? No es solo la mejor trilogía de ciencia ficción de todos los tiempos, es la mejor trilogía del cine. Bueno es solo mi opinión. Parte de lo que hacen a estas películas tan buenas son los diálogos del agente Smith, casi todos con Neo, pero en algunas ocasiones con otros personajes como Trinity o Morfeo.

>Smith: ¿Por qué, señor Anderson? ¿Por qué lo hace? ¿Por qué? ¿Por qué se levanta?

>¿Por qué sigue luchando? ¿Por qué? ¿De verdad cree que lucha por algo además de por su propia supervivencia? ¿Querría decirme qué es, si es que acaso lo sabe?

>¿Es por la libertad? ¿Por la verdad? ¿Tal vez por la paz? ¿Quizás por el amor?

>Ilusiones, señor Anderson, desvaríos de la percepción, concepciones temporales de un frágil intelecto humano que trata con desesperación el justificar una existencia sin sentido ni objetivo. Todas son tan artificiales como Matrix.

Un sello distintivo de esos diálogos es el uso recurrente de preguntas retóricas, es decir aquellas figuras literarias en las que se formula una pregunta sin esperar respuesta, con la finalidad de reforzar o reafirmar el propio punto de vista, al mismo tiempo que incentiva al oyente a reflexionar sobre un asunto o que adopte un cambio en su conducta.

Dada una serie de diálogos extraídos de los guiones de las películas de Matrix, ¿harías un programa para contar cuántas preguntas diferentes (retóricas o normales) realiza el agente Smith? Para ello debes considerar que:

1. El guion se compone de múltiples líneas (renglones), algunas con diálogos, otras con la descripción de lo que está ocurriendo en el entorno.
2. Todo dialogo comienza con el nombre del personaje seguido de dos puntos.
3. Todo dialogo se encuentra en una única línea. Cuando estos son muy extensos se fraccionan, pero cada vez inician como se indica en el punto anterior.
4. Toda pregunta comienza y termina con el signo ? Debería comenzar con el signo ¿ pero resulta que este signo no hace parte de la codificación que usa por defecto la plataforma
5. Toda pregunta contiene por lo menos dos palabras
6. Las preguntas nunca se fraccionan entre dos diálogos
7. Por simplicidad no se usarán tildes

### Entrada

La entrada consiste (presumiblemente) de apartados de los guiones de las películas de Matrix, los cuales se encuentran en el archivo matrix.txt.

### Salida

La salida debe contener, de a una por línea, sin espacio en blanco al final, y en orden de aparición, las preguntas realizadas por Smith. Si una misma pregunta aparece varias veces, solo se debe mostrar su primera aparición.

**Ejemplo de entrada** (el símbolo ← NO HACE PARTE DE LA ENTRADA, solo sirve para
enfatizar que allí hay un salto de línea)

```
(Neo y Smith se encuentran sobre las vias del subterraneo) ←
Smith: ?Por que, sr. Anderson? ?Por que lo hace? ?Por que? ?Por
que se levanta? ?Por que sigue luchando? ?Por que? ?De verdad cree
que lucha por algo ademas de por su propia supervivencia? ←
Smith: ?Querria decirme que es, si es que acaso lo sabe? ?Es por
la libertad? ?Por la verdad? ?Tal vez por la paz? ?Quizas por el
amor? Ilusiones, sr Anderson, desvarios de la percepcion,
concepciones temporales de un fragil intelecto humano que trata
con desesperacion el justificar una existencia sin sentido ni
objetivo. Todas son tan artificiales como Matrix. ←
Neo: ?Quieres mi respuesta? Porque lo he elegido. ←
(Cambio de escena) ←
(Smith mira con desprecio a Morfeo) ←
Smith: Quisiera compartir una revelacion que he tenido desde que
estoy aqui. Esta me sobrevivo, cuando intente clasificar a su
especie. ←
Smith: Vera me di cuenta de que, en realidad no son mamiferos.
Todos los mamiferos de este planeta desarrollan instintivamente un
logico equilibrio con el habitat natural que les rodea. ←
Smith: Pero los humanos no lo hacen. Se trasladan a una zona y se
multiplican y siguen multiplicandose hasta que todos los recursos
naturales se agotan. Asi que el unico modo de sobrevivir es:
extendiendose hasta otra zona. Existe otro organismo en este
planeta que sigue el mismo patron ?Sabe cual es? ←
Morfeo: Iluminame ←
Smith: Un virus. Los humanos sois una enfermedad sois el cancer de
este planeta, sois, una plaga. Y nosotros somos la cura. ←
```

**Ejemplo de salida**
```
?Por que, sr. Anderson?
?Por que lo hace?
?Por que?
?Por que se levanta?
?Por que sigue luchando?
?De verdad cree que lucha por algo ademas de por su propia
supervivencia?
?Querria decirme que es, si es que acaso lo sabe?
?Es por la libertad?
?Por la verdad?
?Tal vez por la paz?
?Quizas por el amor?
?Sabe cual es?
```

```python

```
