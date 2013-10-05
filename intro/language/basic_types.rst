Tipos básicos
=============

Tipos numéricos
---------------

.. tip:: Python soporta los siguientes tipos escalares:

:Integer:

 .. code-block:: python

    >>> 1 + 1
    2
    >>> a = 4
    >>> type(a)
    <type 'int'>

:Floats:

 .. code-block:: python

    >>> c = 2.1
    >>> type(c)
    <type 'float'>

:Complex:

 .. code-block:: python

    >>> a = 1.5 + 0.5j
    >>> a.real
    1.5
    >>> a.imag
    0.5
    >>> type(1. + 0j )
    <type 'complex'>

:Booleans:

 .. code-block:: python

    >>> 3 > 4
    False
    >>> test = (3 > 4)
    >>> test
    False
    >>> type(test)
    <type 'bool'>

.. tip:: La consola Python puede reemplazar a una calculadora, las operaciones arítmeticas básicas ``+``, ``-``, ``*``, ``/``, ``%`` (módulo) estan implementadas nativamente.

.. code-block:: python

   >>> 7 * 3.
   21.0
   >>> 2**10
   1024
   >>> 8 % 3
   2

Conversión de tipos (casting)

.. code-block:: python

   >>> float(1)
   1.0

.. warning:: División de enteros

   .. code-block:: python

      >>> 3 / 2
      1

   **Truco**: Use floats

   .. code-block:: python

      >>> 3 / 2.
      1.5
      >>> a = 3
      >>> b = 2
      >>> a / b
      1
      >>> a / float(b)
      1.5

   .. tip:: Si requiere la parte entera de una division use ``//``
       
      .. code-block:: python

         >>> 3.0 // 2
         1.0

   .. note:: El comportamiento del operador división fue cambiado en Python 3. Por favor visite `python3porting <http://python3porting.com/preparing.html#use-instead-of-when-dividing-integers>`_ para más detalles.

Contenedores
------------

.. tip:: Python proporciona muchos tipos eficazes de contenedores, en la que colecciones de objetos pueden ser almacenados.

Listas
~~~~~~

.. tip:: Una lista es una colección ordenada de objetos, que puede contener diferentes tipos. Por ejemplo:

.. code-block:: python

   >>> L = ['rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> type(L)
   <type 'list'>

Indexado: acceso individual a objetos contenidos en la lista

.. code-block:: python
   
   >>> L[2]
   'verde'

Conteo de elementos desde el final con indices negativos

.. code-block:: python

   >>> L[-1]
   'blanco'
   >>> L[-2]
   'negro'

.. warning:: **El indexado empieza en 0** (como en C), no en 1 (como en Fortran o Matlab)!

Segmentación(slicing): Obtener sublistas con elementos regularmente espaciados

.. code-block:: python

   >>> L
   ['rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> L[2:4]
   ['verde', 'negro']

.. warning:: Note que ``L[inicio:final]`` contiene los elementos con indices ``i`` que pertenecen al intervalo ``inicio<= i < final`` (``i`` es el rango de valores enteros desde ``inicio`` a ``final-1``). Por tanto, ``L[inicio:final]`` tiene elementos ``(final-inicio)``.

**Sintaxis de segmentación**: ``L[inicio:final:paso]``

.. tip:: Los parámetros de segmentación son opcionales

.. code-block:: python

   >>> L
   ['rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> L[3:]
   ['negro', 'blanco']
   >>> L[:3]
   ['rojo', 'azul', 'verde']
   >>> L[::2]
   ['rojo', 'verde', 'blanco']

Las listas son objectos *mutables* y pueden modificarse

.. code-block:: python

   >>> L[0] = 'amarillo'
   >>> L
   ['amarillo', 'azul', 'verde', 'negro', 'blanco']
   >>> L[2:4] = ['gris', 'púrpura']
   >>> L
   ['amarillo', 'azul', 'gris', 'púrpura', 'blanco']

.. note:: Los elementos de una lista pueden ser de tipos diferentes

   .. code-block:: python

      >>> L = [3, -200, 'hola']
      >>> L
      [3, -200, 'hola']
      >>> L[1], L[2]
      (-200, 'hola')

   .. tip:: Para las colecciones de datos numéricos que tienen el mismo tipo, a menudo es **más eficiente** utilizar el tipo ``arreglo (array)`` proporcionado por el módulo ``numpy``. Un arreglo NumPy es un trozo de memoria que contiene elementos de tamaño fijo. Con arreglos NumPy, las operaciones con elementos son más rápidos porque los elementos están espaciados regularmente en memoria y otras operaciones se realizan a través funciones C especializadas en lugar de bucles Python.

.. tip:: Python ofrece un gran panel de funciones para modificar las listas, o la consulta a ellos. Éstos son algunos ejemplos; para obtener más detalles, consulte http://docs.python.org/tutorial/datastructures.html#more-on-lists

Agregar y remover elementos

.. code-block:: python

   >>> L = ['rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> L.append('rosado')
   >>> L
   ['rojo', 'azul', 'verde', 'negro', 'blanco', 'rosado']
   >>> L.pop() # remover y devolver el último item
   'rosado'
   >>> L
   ['rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> L.extend(['rosado', 'violeta']) # extender L, sobre la marcha
   >>> L
   ['rojo', 'azul', 'verde', 'negro', 'blanco', 'rosado', 'violeta']
   >>> L = L[:-2]
   >>> L
   ['rojo', 'azul', 'verde', 'negro', 'blanco']

Invertir el orden de los elementos (reverse)

.. code-block:: python

   >>> r = L[::-1]
   >>> r
   ['blanco', 'negro', 'verde', 'azul', 'rojo']
   >>> r2 = list(L)
   >>> r2
   ['rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> r2.reverse() # reasignando valores
   >>> r2
   ['blanco', 'negro', 'verde', 'azul', 'rojo']

Concatenar y repetir listas

.. code-block:: python

   >>> r + L
   ['blanco', 'negro', 'verde', 'azul', 'rojo', 'rojo', 'azul', 'verde', 'negro', 'blanco']
   >>> r * 2
   ['blanco', 'negro', 'verde', 'azul', 'rojo', 'blanco', 'negro', 'verde', 'azul', 'rojo']

.. tip:: Ordenar en forma ascendente

.. code-block:: python

   >>> sorted(r) # nuevo objecto
   ['azul', 'blanco', 'negro', 'rojo', 'verde']
   >>> r
   ['blanco', 'negro', 'verde', 'azul', 'rojo']
   >>> r.sort()  # reasignando elementos
   >>> r
   ['azul', 'blanco', 'negro', 'rojo', 'verde']

.. note:: **Métodos y Programación Orientada a Objectos**

   La notación ``r.method()`` (``r.append(3)``, ``L.pop()``) es el primer ejemplo de Programación Orientada a Objectos (POO). Para una ``lista``, el objecto es `r` posee el *método* `función` que es llamado usando la notación **.**. Sin profundizar en el conocimiento de la Programación Orientada a Objetos la comprensión de la notación **.** es necesaria para recorrer este tutorial.

.. note:: **Descubriendo métodos:**

   Recuerde: en IPython: autocompletado con tabular (presione tab)

   .. sourcecode:: ipython

       In [28]: r.<TAB>
       r.__add__           r.__iadd__          r.__setattr__
       r.__class__         r.__imul__          r.__setitem__
       r.__contains__      r.__init__          r.__setslice__
       r.__delattr__       r.__iter__          r.__sizeof__
       r.__delitem__       r.__le__            r.__str__
       r.__delslice__      r.__len__           r.__subclasshook__
       r.__doc__           r.__lt__            r.append
       r.__eq__            r.__mul__           r.count
       r.__format__        r.__ne__            r.extend
       r.__ge__            r.__new__           r.index
       r.__getattribute__  r.__reduce__        r.insert
       r.__getitem__       r.__reduce_ex__     r.pop
       r.__getslice__      r.__repr__          r.remove
       r.__gt__            r.__reversed__      r.reverse
       r.__hash__          r.__rmul__          r.sort

Cadenas
~~~~~~~

Diferentes sintaxis de cadena (simple, double o comilla triple)

.. code-block:: python

   s = 'Hola, cómo estás'
   s = "Hola, qué tal"
   s = '''Hola,                  # triplicando las comillas permite a
          cómo estás'''          # la cadena ocupar más de una línea
   s = """Hola,
   qué tal"""

.. sourcecode:: ipython

   In [1]: 'Hola, qué tal'
   ------------------------------------------------------------
      File "<ipython console>", line 1
       'Hola, qué tal'
              ^
   SyntaxError: invalid syntax

El carácter nueva línea es ``\n``, y el carácter tabulación es ``\t``.

.. tip:: Las cadenas son colecciones como las listas. Por lo tanto pueden ser indexados y segmentados, utilizando la mismas reglas de sintaxis.

Indexado

.. code-block:: python

   >>> a = "hola"
   >>> a[0]
   'h'
   >>> a[1]
   'o'
   >>> a[-1]
   'a'

.. tip:: (Recuerde que los valores negativos corresponden a un conteo desde la derecha.)

Segmentación (slicing)

.. code-block:: python

   >>> a = "hola, mundo!"
   >>> a[2:5] # elementos 2do al 5to (excluido): elementos 2, 3, 4
   'la,'
   >>> a[2:11:2] # sintaxis: a[inicio:final:paso]
   'l,mno'
   >>> a[::3] # cada tres caracteres, del inicio al final
   'hamd'

.. tip:: Acentos y caracteres especiales también pueden ser manejados en cadenas Unicode (véase http://docs.python.org/tutorial/introduction.html#unicode-strings).

Una cadena es un **objeto inmutable** y no es posible modificar su contenido. Sin embargo, se puede crear nuevas cadenas a partir del original.

.. sourcecode:: ipython

    In [53]: a = "hola, mundo!"
    In [54]: a[2] = 'z'
    ---------------------------------------------------------------------------
    Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
    TypeError: 'str' object does not support item assignment

    In [55]: a.replace('l', 'z', 1)
    Out[55]: 'hoza, mundo!'
    In [56]: a.replace('l', 'z')
    Out[56]: 'hzla, mundz!'

.. tip:: Las cadenas tienen muchos métodos útiles, como ``a.replace`` como se vio anteriormente. Recuerde la notación orientada a objetos ``a.`` y el autocompletado con tabulador o ``help(str)`` para buscar nuevos métodos.

.. seealso:: Python ofrece posibilidades avanzadas para la manipulación de cadenas, busqueda por patrones o formateado. El lector interesado puede consultar http://docs.python.org/library/stdtypes.html#string-methods y http://docs.python.org/library/string.html#new-string-formatting.

Sustitución de cadenas

.. code-block:: python

   >>> 'Un integer: %i; un float: %f; otra cadena: %s' % (1, 0.1, 'cadena')
   'Un integer: 1; un float: 0.100000; otra cadena: cadena'
   >>> i = 102
   >>> nombre_archivo = 'procesamiento_de_conjunto_de_datos_%d.txt' % i
   >>> nombre_archivo
   'procesamiento_de_conjunto_de_datos_102.txt'

Diccionarios
~~~~~~~~~~~~

.. tip:: Un diccionario es básicamente una tabla eficiente que **mapea claves a valores**. Es un contenedor **sin orden** 

.. code-block:: python

   >>> tel = {'emmanuelle': 5752, 'sebastian': 5578}
   >>> tel['francis'] = 5915
   >>> tel
   {'sebastian': 5578, 'francis': 5915, 'emmanuelle': 5752}
   >>> tel['sebastian']
   5578
   >>> tel.keys()
   ['sebastian', 'francis', 'emmanuelle']
   >>> tel.values()
   [5578, 5915, 5752]
   >>> 'francis' in tel
   True

.. tip:: Se puede utilizar para almacenar y recuperar valores convenientemente asociados a un nombre (una cadena de una fecha, un nombre, etc.) ver http://docs.python.org/tutorial/datastructures.html#dictionaries para más información.
Un diccionario puede tener claves (respuesta valores) con diferentes tipos.

.. code-block:: python

   >>> d = {'a':1, 'b':2, 3:'hola'}
   >>> d
   {'a': 1, 3: 'hola', 'b': 2}

Más contenedores de tipos
~~~~~~~~~~~~~~~~~~~~~~~~~

**Tuplas**

Las tuplas son basicamente listas immutables. Los elementos de una tupla se escriben entre paréntesis y separados por comas, o solamente separados por comas.

.. code-block:: python

   >>> t = 12345, 54321, 'hola!'
   >>> t[0]
   12345
   >>> t
   (12345, 54321, 'hola!')
   >>> u = (0, 2)
   >>> u
   (0, 2)

**Conjuntos:** sin orden, items únicos.

.. code-block:: python

   >>> s = set(('a', 'b', 'c', 'a'))
   >>> s
   set(['a', 'c', 'b'])
   >>> s.difference(('a', 'b'))
   set(['c'])

Operador de asignación
----------------------

.. tip:: La `referencia de bibliotecas Python <http://docs.python.org/reference/simple_stmts.html#assignment-statements>`_ dice:

..

  Las sentencias de asignación se utilizan para (re)vincular nombres a valores y modificar los atributos o elementos de los objetos mutables.

En pocas palabras, funciona de la siguiente manera (asignación simple):

#. una expresión en el lado derecho es evaluada, el correspondiente objeto se crea/obtiene

#. un **nombre** en el lado izquierdo es asignado, o vinculado al objeto r.h.s.

Cosas a tener en cuenta:

* un objeto puede estar vinculado a varios nombres:

  .. sourcecode:: ipython

     In [1]: a = [1, 2, 3]
     In [2]: b = a
     In [3]: a
     Out[3]: [1, 2, 3]
     In [4]: b
     Out[4]: [1, 2, 3]
     In [5]: a is b
     Out[5]: True
     In [6]: b[1] = 'hi!'
     In [7]: a
     Out[7]: [1, 'hi!', 3]

* para cambiar una lista *sin crear una copia*, use indexado/segmentado:

  .. sourcecode:: ipython

     In [1]: a = [1, 2, 3]
     In [3]: a
     Out[3]: [1, 2, 3]
     In [4]: a = ['a', 'b', 'c'] # Creando otro objeto.
     In [5]: a
     Out[5]: ['a', 'b', 'c']
     In [6]: id(a)
     Out[6]: 138641676
     In [7]: a[:] = [1, 2, 3] # Modificando objetos sobre la marcha.
     In [8]: a
     Out[8]: [1, 2, 3]
     In [9]: id(a)
     Out[9]: 138641676 # Lo mismo esta en Out[6], el tuyo puede ser diferente...

* el concepto clave aqui es **mutable vs. immutable**

  * mutable, objecto que puede cambiar sobre sobre la marcha
  * immutable, objecto que no puede modificarse despues de su creación

.. seealso:: Una explicación muy buena y detallada de los aspectos mencionados anteriormente se encuentra en el artículo `Types and Objects in Python <http://www.informit.com/articles/article.aspx?p=453682>`_ de David M. Beazley's.
