# Juramento turiano

Por si no lo sabías, cuando un profesional de una carrera universitaria de medicina se
gradúa, dentro de la ceremonia de graduación debe pronunciar el juramento Hipodrático
(cuyo nombre hace referencia al médico griego Hipócrates), el cual comienza más o
menos así:

Juro por Apolo médico, por Asclepio, Higía y Panacea y pongo por testigos a todos los
dioses y diosas, de que he de observar el siguiente juramento, que me obligo a cumplir
en cuanto ofrezco, poniendo en tal empeño todas mis fuerzas y mi inteligencia.

Tributaré a mi maestro de Medicina el mismo respeto que a los autores de mis días,
partiré con ellos mi fortuna y los socorreré si lo necesitaren; trataré a sus hijos como a
mis hermanos y si quieren aprender la ciencia, se la enseñaré des-interesadamente y
sin ningún género de recompensa.

Bueno, supongamos que a los que se hacen este curso de programación de
computadores se les exigiera algo similar. En este caso el juramento no se hace al
graduarse si no al empezar el curso y no hace referencia a Hipócrates sino a uno de los
progenitores de la computación: Alan Turing.

Obviamente, siendo un curso de programación, en vez de recitarlo, a los estudiantes se
les exige que hagan un programa que los recite por ellos.

Debes entonces hacer un programa que imprima lo siguiente (sin tildes ni saltos de línea):

Juro por Turing que daré mi mayor esfuerzo por completar este curso pero sobre todo
por aprender el grandioso arte de programar. Pongo como testigos a todos los bits y
bytes que revisare todos los materiales y haré a consciencia todos los ejercicios.

Para asegurar que imprimirás el mensaje sin problemas de puntuación, espacios o saltos
de línea, te recomiendo que lo copies y pegues del archivo “Juramento Turiniano.txt”
que aparece en los materiales.


```python
def jurameto_turiano():
    print("Juramento turiano")
```

```python
mensaje = 'Juro por Turing que dare mi mayor esfuerzo por completar este curso pero sobre todo por aprender el grandioso arte de programar. Pongo como testigos a todos los bits y bytes que revisare todos los materiales y hare a consciencia todos los ejercicios.'
```

```python
mensaje = 'Juro por Turing que dare mi mayor esfuerzo por completar este curso pero sobre todo por aprender el grandioso arte de programar. Pongo como testigos a todos los bits y bytes que revisare todos los materiales y hare a consciencia todos los ejercicios.'
```

```python
def juramento_turiano():
    print("Juramento turiano")

def test_juramento_turiano(capsys):
    juramento_turiano()
    captured = capsys.readouterr()
    assert captured.out.strip() == "Juramento turiano"
```

```python
import pytest
pytest.main(["--capture=sys"])
```
