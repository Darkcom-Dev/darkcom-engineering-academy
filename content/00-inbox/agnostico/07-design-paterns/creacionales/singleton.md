# Singleton

Singleton es un patrón de diseño creacional que garantiza que tan solo exista un objeto de su tipo y proporciona un 
único punto de acceso a el para cualquier otro código.

- El patrón tiene prácticamente los mismos pros y contras que las variables globales. 
- Aunque son muy útiles, rompen la modularidad de tu código.
- No se puede utilizar una clase que dependa del Singleton en otro contexto. 
- Tendrás que llevar también la clase Singleton. 
- La mayoría de las veces, esta limitación aparece durante la creación de pruebas de unidad.


Complejidad: *--

Popularidad: **-

### Ejemplos de uso: 

Muchos desarrolladores consideran el patrón Singleton un anti-patrón. 
Por este motivo, su uso esta en declive en el código Python.

### Identificación: 

El patrón Singleton se puede reconocer por un método de creación estático, 
que devuelve el mismo objeto guardado en cache.

# Singleton ingenuo
---
Es muy fácil implementar un Singleton descuidado. 
Tan solo necesitas esconder el constructor e implementar un método de creación estático.

La misma clase se comporta de forma incorrecta en un entorno multi-hilo. 
Los múltiples hilos pueden llamar al método de creación de forma simultanea y 
obtener varias instancias de la clase Singleton.


```python
class SingletonMeta(type):
    """
    The Singleton class can be implemented in different ways in Python. Some
    possible methods include: base class, decorator, metaclass. We will use the
    metaclass because it is best suited for this purpose.
    """

    _instances = {}

    def __call__(cls, *args, **kwargs):
        """
        Possible changes to the value of the `__init__` argument do not affect
        the returned instance.
        """
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]


class Singleton(metaclass=SingletonMeta):
    def some_business_logic(self):
        """
        Finally, any singleton should define some business logic, which can be
        executed on its instance.
        """

        # ...


def main(args):
	 # The client code.

    s1 = Singleton()
    s2 = Singleton()

    if id(s1) == id(s2):
        print("Singleton works, both variables contain the same instance.")
    else:
        print("Singleton failed, variables contain different instances.")
        
    return 0

if __name__ == '__main__':
    import sys
    sys.exit(main(sys.argv))

    """ Output.txt: Resultado de la ejecucion
    Singleton works, both variables contain the same instance.
    """
```

# Propósito
---
Singleton es un patrón de diseño creacional que nos permite asegurarnos de que una 
clase tenga una única instancia, a la vez que proporciona un punto de acceso global 
a dicha instancia.

# Problema
---
El patron Singleton resuelve dos problemas al mismo tiempo, vulnerando el Principio de responsabilidad unica:

Garantizar que una clase tenga una unica instancia. 
Por que querria alguien controlar cuantas instancias tiene una clase? 

El motivo mas habitual es controlar el acceso a algun recurso compartido, por ejemplo, una base de datos o un archivo.

## Funciona asi: 

imagina que has creado un objeto y al cabo de un tiempo decides crear otro nuevo. 
En lugar de recibir un objeto nuevo, obtendras el que ya habias creado.

Ten en cuenta que este comportamiento es imposible de implementar con un constructor 
normal, ya que una llamada al constructor siempre debe devolver un nuevo objeto por diseño.

## El acceso global a un objeto

Puede ser que los clientes ni siquiera se den cuenta de que trabajan con el mismo 
objeto todo el tiempo.

Proporcionar un punto de acceso global a dicha instancia. 
Recuerdas esas variables globales que utilizaste (bueno, si, fui yo) 
para almacenar objetos esenciales? Aunque son muy útiles, también son poco seguras, ya que cualquier código podría sobrescribir el contenido de esas variables y descomponer la aplicación.

Al igual que una variable global, el patrón Singleton nos permite acceder a un 
objeto desde cualquier parte del programa. 

No obstante, tambien evita que otro codigo sobreescriba esa instancia.

## Este problema tiene otra cara: 
no queremos que el codigo que resuelve el primer problema se encuentre disperso 
por todo el programa. Es mucho mas conveniente tenerlo dentro de una clase, sobre todo si el resto del código ya depende de ella.

Hoy en día el patrón Singleton se ha popularizado tanto que la gente suele llamar 
singleton a cualquier patrón, incluso si solo resuelve uno de los problemas antes 
mencionados.

# Solución

Todas las implementaciones del patrón Singleton tienen estos dos pasos en común:

Hacer privado el constructor por defecto para evitar que otros objetos utilicen 
el operador new con la clase Singleton.

Crear un metodo de creacion estatico que actue como constructor. 

Tras bambalinas, este metodo invoca al constructor privado para crear un objeto 
y lo guarda en un campo estatico. 
Las siguientes llamadas a este metodo devuelven el objeto almacenado en cache.

Si tu codigo tiene acceso a la clase Singleton, podra invocar su metodo estatico. 
De esta manera, cada vez que se invoque este metodo, siempre se devolvera el mismo objeto.

# Analogía en el mundo real

El gobierno es un ejemplo excelente del patrón Singleton. 
Un país solo puede tener un gobierno oficial. 
Independientemente de las identidades personales de los individuos que forman 
el gobierno, el titulo "Gobierno de X" es un punto de acceso global que identifica 
al grupo de personas a cargo.

# Estructura
## La estructura del patrón Singleton

La clase Singleton declara el método estático `obtenerInstancia` que devuelve 
la misma instancia de su propia clase.

El constructor del Singleton debe ocultarse del código cliente. 
La llamada al método `obtenerInstancia` debe ser la única manera de obtener el 
objeto de Singleton.

# Pseudocodigo

En este ejemplo, la clase de conexión de la base de datos actúa como Singleton. 
Esta clase no tiene un constructor publico, por lo que la única manera de obtener 
su objeto es invocando el método `obtenerInstancia`. 

Este método almacena en cache el primer objeto creado y lo devuelve en todas las 
llamadas siguientes.

```python
// La clase Base de datos define el metodo `obtenerInstancia`
// que permite a los clientes acceder a la misma instancia de
// una conexion de la base de datos a traves del programa.
class Database is
    // El campo para almacenar la instancia singleton debe
    // declararse estatico.
    private static field instance: Database

    // El constructor del singleton siempre debe ser privado
    // para evitar llamadas de construccion directas con el
    // operador `new`.
    private constructor Database() is
        // Algun codigo de inicializacion, como la propia
        // conexion al servidor de una base de datos.
        // ...

    // El metodo estatico que controla el acceso a la instancia
    // singleton.
    public static method getInstance() is
        if (Database.instance == null) then
            acquireThreadLock() and then
                // Garantiza que la instancia aun no se ha
                // inicializado por otro hilo mientras esta ha
                // estado esperando el desbloqueo.
                if (Database.instance == null) then
                    Database.instance = new Database()
        return Database.instance

    // Por ultimo, cualquier singleton debe definir cierta
    // logica de negocio que pueda ejecutarse en su instancia.
    public method query(sql) is
        // Por ejemplo, todas las consultas a la base de datos
        // de una aplicacion pasan por este metodo. Por lo
        // tanto, aqui puedes colocar logica de regularizacion
        // (throttling) o de envio a la memoria cache.
        // ...

class Application is
    method main() is
        Database foo = Database.getInstance()
        foo.query("SELECT ...")
        // ...
        Database bar = Database.getInstance()
        bar.query("SELECT ...")
        // La variable `bar` contendra el mismo objeto que la
        // variable `foo`.
 

```
# Aplicabilidad

Utiliza el patron Singleton cuando una clase de tu programa tan solo deba 
tener una instancia disponible para todos los clientes; por ejemplo, 
un unico objeto de base de datos compartido por distintas partes del programa.

El patron Singleton deshabilita el resto de las maneras de crear objetos de una clase, 
excepto el metodo especial de creacion. 

Este metodo crea un nuevo objeto, o bien devuelve uno existente si ya ha sido creado.

Utiliza el patron Singleton cuando necesites un control mas estricto de las 
variables globales.

Al contrario que las variables globales, el patrón Singleton garantiza que 
haya una única instancia de una clase. 

Una excepción de la propia clase Singleton, nada puede sustituir la instancia en cache.

Ten en cuenta que siempre podrás ajustar esta limitación y permitir la creación 
de cierto numero de instancias Singleton. 
La única parte del código que requiere cambios es el cuerpo del método `getInstance`.

# Como implementarlo


- Añade un campo estático privado a la clase para almacenar la instancia Singleton.
- Declara un método de creación estático publico para obtener la instancia Singleton.
- Implementa una inicialización diferida dentro del método estático. 

Debe crear un nuevo objeto en su primera llamada y colocarlo dentro del campo estático. 
El método deberá devolver siempre esa instancia en todas las llamadas siguientes.

- Declara el constructor de clase como privado. El método estático de la clase 
seguirá siendo capaz de invocar al constructor, pero no a los otros objetos.

Repasa el código cliente y sustituye todas las llamadas directas al constructor 
de la instancia Singleton por llamadas a su método de creación estático.

# Pros y contras

- Puedes tener la certeza de que una clase tiene una única instancia.
- Obtienes un punto de acceso global a dicha instancia.
- El objeto Singleton solo se inicializa cuando se requiere por primera vez.
- Vulnera el Principio de responsabilidad única. El patrón resuelve dos problemas 
al mismo tiempo.
- El patrón Singleton puede enmascarar un mal diseño, por ejemplo, 
cuando los componentes del programa saben demasiado los unos sobre los otros.
- El patrón requiere de un tratamiento especial en un entorno con múltiples 
hilos de ejecución, para que varios hilos no creen un objeto Singleton varias veces.
- Puede resultar complicado realizar la prueba unitaria del código cliente 
del Singleton porque muchos frameworks de prueba dependen de la herencia 
a la hora de crear objetos simulados (mock objects). Debido a que la clase 
Singleton es privada y en la mayoria de los lenguajes resulta imposible 
sobrescribir métodos estáticos, tendrás que pensar en una manera original 
de simular el Singleton. O, simplemente, no escribas las pruebas. 
O no utilices el patrón Singleton.

# Relaciones con otros patrones


Una clase fachada a menudo puede transformarse en una Singleton, 
ya que un único objeto fachada es suficiente en la mayoría de los casos.

Flyweight podría asemejarse a Singleton si de algún modo pudieras reducir 
todos los estados compartidos de los objetos a un único objeto flyweight. 
Pero existen dos diferencias fundamentales entre estos patrones:

Solo debe haber una instancia Singleton, mientras que una clase Flyweight 
puede tener varias instancias con distintos estados intrínsecos.
El objeto Singleton puede ser mutable. Los objetos flyweight son inmutables.
Los patrones Abstract Factory, Builder y Prototype pueden todos ellos 
implementarse como Singletons.

## Relacionados:

- [[prototype]] #related 
- [[factory-method]] #related 
- [[builder]] #related 
- [[abstract-factory]] #related 
- [[singleton-con-seguridad-de-hilos]] #siguiente 
