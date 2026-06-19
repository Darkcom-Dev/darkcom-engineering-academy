# Hola mundo

Tal como discutimos en el curso, el primer programa por excelencia cuando se está
aprendiendo a programar es uno que simplemente imprima por pantalla el mensaje 'Hola
mundo!'. Como era de esperarse, los libros de texto en inglés, e incluso algunos en
español, tienen su versión 'Hello world!'.

Para no desentonar y rendir un poco de tributo a la historia, hagamos un programa que
imprima ese mensaje en inglés.

Pero cuidado, debes tener en cuenta que la 'H' en 'Hello' es mayúscula y que hay un
signo de admiración al final de la frase.


```python
# Aun en desarrollo los tests
```

```python
import pytest

def hello_world():
    print("Hola mundo!")

def test_hello_world(capsys):
    hello_world()
    captured = capsys.readouterr()
    assert captured.out.strip() == "Hola mundo!", 'La salida debe ser "Hola mundo!"'

pytest.main(['--capture=sys'])
```
