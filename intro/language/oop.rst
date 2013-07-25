Programación Orientada a Objetos (POO)
======================================

Python soporta la programación orientada a objetos (POO). Los objetivos de la programación orientada a objetos son:

    * organizar el código, y

    * volver a utilizar el código en contextos similares.

He aquí un pequeño ejemplo: creamos la *clase* Estudiante, es un objeto
que reune varias funciones personalizadas (*métodos*) y variables (*atributos*), que pueden usarse::

    >>> class Estudiante(objecto):
    ...     def __init__(self, nombre):
    ...         self.nombre = nombre
    ...     def establece_edad(self, edad):
    ...         self.edad = edad
    ...     def establece_licenciatura(self, licenciatura): 
    ...         self.licenciatura = licenciatura
    ...  
    >>> anna = Estudiante('anna')
    >>> anna.establece_edad(21)
    >>> anna.establece_licenciatura('fisica')

En el ejemplo anterior, la clase Estudiante tiene los métodos ``__init__``, ``establece_edad`` y ``establece_licenciatura``. Sus atributos son ``nombre``, ``edad`` y ``licenciatura``. Nosotros podemos llamar a estos métodos y atributos con la siguiente notación: ``classinstance.método`` o ``classinstance.atributo``. El constructor ``__init__`` es un método especial que llamamos con: ``MiClase(parámetros de inicio cualquiera)``.

Ahora, supongamos que queremos crear una nueva clase EstudianteMaestria con los mismos métodos y atributos que el anterior, pero con un atributo adicional ``practicas``. No copiaremos la clase anterior, pero si **heredarlos** de ella::

    >>> class EstudianteMaestria(Estudiante):
    ...     practicas = 'obligatorias, de marzo a junio'
    ...   

    >>> james = EstudianteMaestria('james')
    >>> james.practicas
    'obligatorias, de marzo a junio'
    >>> james.establece_edad(23)
    >>> james.edad
    23

La clase EstudianteMaestria hereda los atributos y métodos de la clase Estudiante.

Gracias a las clases y a la programación orientada a objetos, podemos organizar el código en distintas clases correspondientes a diferentes objetos que nos encontramos (clase Experimento, clase Imagen, clase Flujo, etc), con sus propios métodos y atributos. También podemos utilizar la herencia para considerar variaciones en torno a una clase base y **reutilizar** código. Ejemplo: a partir de la clase Flujo, podemos crear sus derivados FlujoStokes, FlujoTurbulento, FlujoPotencial, etc.
