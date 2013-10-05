Definiendo funciones
====================

Definiendo una función
----------------------

.. sourcecode:: ipython

   In [56]: def test():
      ....:     print('funcion de prueba')
      ....:
      ....:

   In [57]: test()
   funcion de prueba

.. warning:: Después de definir una función deben indentarse los bloques que la componen.

Sentencia return
----------------

Las funciones pueden *opcionalmente* devolver valores.

.. sourcecode:: ipython

   In [6]: def area_circulo(radio):
      ...:     return 3.14 * radio * radio
      ...:

   In [8]: area_circulo(1.5)
   Out[8]: 7.0649999999999995

.. note:: Por defecto, las funciones devuelven ``None``.

.. note:: Tenga en cuenta la sintaxis para definir una función:

   * la palabra clave ``def``;

   * seguido por el **nombre** la función, a continuación

   * los argumentos de la función van entre paréntesis seguido por dos puntos.

   * el cuerpo de la función;

   * y ``return object`` si la función devuelve valores.

Parámetros
----------

Parámetros obligatorios (argumentos con posición)

.. sourcecode:: ipython

   In [81]: def por_dos(x):
      ....:     return x * 2
      ....:

   In [82]: por_dos(3)
   Out[82]: 6

   In [83]: por_dos()
   ---------------------------------------------------------------------------
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: double_it() takes exactly 1 argument (0 given)

Parámetros opcionales (argumentos con nombre o palabra clave)

.. sourcecode:: ipython

   In [84]: def por_dos(x=2):
      ....:     return x * 2
      ....:

   In [85]: por_dos()
   Out[85]: 4

   In [86]: por_dos(3)
   Out[86]: 6

Los argumentos con nombre le permiten especificar los *valores por defecto*.

.. warning:: Los valores por defecto se evalúan cuando se define la función, no cuando se le llama. Esto puede ser problemático cuando se utiliza tipos mutables (por ejemplo, un diccionario o una lista) y su modificación en el cuerpo de la función, ya que el modificaciones no persisten al invocar una función. 

.. sourcecode:: ipython

   In [124]: gran_x = 10

   In [125]: def por_dos(x=gran_x):
      .....:     return x * 2
      .....:

   In [126]: por_dos()
   Out[126]: 20

   In [127]: gran_x = 1e9  # Ahora si es grande

   In [128]: por_dos()
   Out[128]: 20

.. tip:: Un ejemplo que se parece a la segmentación de Python:

.. sourcecode:: ipython

   In [98]: def segmento(secuencia, inicio=None, final=None, paso=None):
      ....:     """Implementacion basica de segmentacion python."""
      ....:     return secuencia[inicio:final:paso]
      ....:

   In [101]: rima = 'pez uno, pez dos, pez rojo, pez azul'.split()

   In [102]: rima
   Out[102]: ['pez', 'uno,', 'pez', 'dos,', 'pez', 'rojo,', 'pez', 'azul']

   In [103]: segmento(rima)
   Out[103]: ['pez', 'uno,', 'pez', 'dos,', 'pez', 'rojo,', 'pez', 'azul']

   In [104]: segmento(rima, paso=2)
   Out[104]: ['pez', 'pez', 'pez', 'pez']

   In [105]: segmento(rima, 1, paso=2)
   Out[105]: ['uno,', 'dos,', 'rojo,', 'azul']

   In [106]: segmento(rima, inicio=1, final=4, paso=2)
   Out[106]: ['uno,', 'dos,']

El orden de los argumentos con nombre no importa:

.. sourcecode:: ipython

   In [107]: segmento(rima, paso=2, inicio=1, final=4)
   Out[107]: ['uno,', 'dos,']

pero es una buena práctica usar el mismo orden que en la definición.

Los *argumentos con nombre* son una característica muy conveniente para la definición de funciones con un número variable de argumentos, sobre todo cuando los valores por defecto se usan en la mayoría de las llamadas a la función.

Paso por valor
--------------

.. tip:: Se puede modificar el valor de una variable dentro una función? La mayoría de los lenguajes (C, Java, ...) distinguen ``el paso por valor`` y ``el paso por referencia``. En Python, tal distinción es un tanto artificial, y es un poco sutil si las variables serán modificadas o no. Afortunadamente, existen reglas claras.

Los parámetros a funciones son referencias a los objetos, que se pasan por valor. Cuando se pasa una variable a una función, Python pasa la referencia al objeto al que hace referencia la variable (el **valor**). No es la propia variable.

Si el **valor** pasado a una función es inmutable, la función no modifica la variable llamada. Si el **valor** es mutable, la función puede modificar la variable

.. code-block:: python

   >>> def intenta_modificar(x, y, z):
   ...     x = 23
   ...     y.append(42)
   ...     z = [99] # nueva referencia
   ...     print x
   ...     print y
   ...     print z
   ...
   >>> a = 77    # variable immutable 
   >>> b = [99]  # variable mutable
   >>> c = [28]
   >>> intenta_modificar(a, b, c)
   23
   [99, 42]
   [99]
   >>> print a 
   77
   >>> print b
   [99, 42]
   >>> print c
   [28]

Las funciones tienen una tabla de variables locales llamada *local namespace*.

La variable ``x`` sólo existe dentro la función *intenta_modificar*.

Variables globales
------------------

Las variables declaradas fuera de una función pueden referenciarse a una función:

.. sourcecode:: ipython

   In [114]: x = 5

   In [115]: def suma_x(y):
      .....:     return x + y
      .....:

   In [116]: suma_x(10)
   Out[116]: 15

Pero estas variables ``globales`` no se pueden modificar dentro de la función, a menos que se declare como **global** en la función.

Esto no funciona:

.. sourcecode:: ipython

   In [117]: def asigna_x(y):
      .....:     x = y
      .....:     print 'x is %d' % x
      .....:
      .....:

   In [118]: asigna_x(10)
   x is 10

   In [120]: x
   Out[120]: 5

Esto si funciona:

.. sourcecode:: ipython

   In [121]: def asigna_x(y):
      .....:     global x
      .....:     x = y
      .....:     print 'x is %d' % x
      .....:
      .....:

   In [122]: asigna_x(10)
   x is 10

   In [123]: x
   Out[123]: 10

Número variable de parámetros
-----------------------------
Formas especiales de los parámetros:

* ``*args``: cualquier número de argumentos posicionales en una tupla

* ``**kwargs``: cualquier número de argumentos con nombre en un diccionario

.. sourcecode:: ipython

   In [35]: def argumentos_variables(*args, **kwargs):
      ....:     print 'args es', args
      ....:     print 'kwargs es', kwargs
      ....:

   In [36]: argumentos_variables('uno', 'dos', x=1, y=2, z=3)
   args es ('uno', 'dos')
   kwargs es {'y': 2, 'x': 1, 'z': 3}

Docstrings
----------

Documentación sobre lo que hace la función y sus parámetros. Convención general:

.. sourcecode:: ipython

   In [67]: def funcion_ejemplo(parametros):
      ....:     """Frase concisa de una línea que describe la funcion.
      ....:
      ....:     Resumen extendido que puede contener varios parrafos.
      ....:     """
      ....:     # cuerpo de funcion
      ....:     pass
      ....:

   In [68]: funcion_ejemplo?
   Type:       function
   String Form:<function funcion_ejemplo at 0x9893e9c>
   File:       /home/claudio/<ipython-input-4-3c894f027eb2>
   Definition: funcion_ejemplo(parametros)
   Docstring:
   Frase concisa de una línea que describe la funcion.

   Resumen extendido que puede contener varios parrafos.

.. note:: **Guia para docstrings**

   Para estandarizar la documentación, revise la página web `Docstring Conventions <http://www.python.org/dev/peps/pep-0257>`_  contiene documentos de semántica y convenios relacionados con Python docstrings.

   Además, los módulos numpy y scipy han definido un estándar preciso para la documentación de las funciones científicas, es posible que desee seguir para sus propias funciones, con una sección ``Parámetros`` , una sección ``Ejemplos``, etc. Ver http://projects.scipy.org/numpy/wiki/CodingStyleGuidelines#docstring-standard y http://projects.scipy.org/numpy/browser/trunk/doc/example.py#L37

Las funciones son objectos
--------------------------

Las funciones son la primera clase de objetos, lo que significa que puede:

* asignarse a una variable

* ser un elemento de una lista (o cualquier colección)

* ser pasado como argumento a otra función.

.. sourcecode:: ipython

   In [38]: va = argumentos_variables

   In [39]: va('tres', x=1, y=2)
   args es ('tres',)
   kwargs es {'y': 2, 'x': 1}

Métodos
-------

Los métodos son funciones vinculadas a los objetos. Usted ha visto esto en los ejemplos de *listas*, *diccionarios*, *cadenas*, etc ..

Ejercicios
----------

.. topic:: Ejercicio: Serie de Fibonacci
   :class: green

   Escriba una función que muestre los ``n`` primeros términos de la serie de Fibonacci, definido por:

    * ``u_0 = 1; u_1 = 1``
    * ``u_(n+2) = u_(n+1) + u_n``

.. :ref:`fibonacci`

.. topic:: Ejercicio: Quicksort
   :class: green

   Implemente el algoritmo quicksort, definido por wikipedia::

   function quicksort(array)
       var list less, greater
       if length(array) < 2
           return array
       select and remove a pivot value pivot from array
       for each x in array
           if x < pivot + 1 then append x to less
           else append x to greater
       return concatenate(quicksort(less), pivot, quicksort(greater))

.. :ref:`quick_sort`
