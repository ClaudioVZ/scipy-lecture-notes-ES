Manejo de excepciones en Python
===============================

Es muy poco probable que se hayan producido excepciones si tiene escrito todos los comandos anteriores del tutorial. Por ejemplo, es posible que se haya producido una excepción si introdujo un comando con un error tipográfico.

Las excepciones se producen por diferentes tipos de errores que surgen al ejecutar código Python. En su propio código, también puede detectar errores o definir tipos personalizados de errores. Es posible que desee ver las descripciones en `the built-in Exceptions <http://docs.python.org/2/library/exceptions.html>`_.

Excepciones
-----------

Excepciones producidas por errores en Python:

.. sourcecode:: ipython

    In [1]: 1/0
    ---------------------------------------------------------------------------
    ZeroDivisionError: integer division or modulo by zero

    In [2]: 1 + 'e'
    ---------------------------------------------------------------------------
    TypeError: unsupported operand type(s) for +: 'int' and 'str'

    In [3]: d = {1:1, 2:2}

    In [4]: d[3]
    ---------------------------------------------------------------------------
    KeyError: 3

    In [5]: l = [1, 2, 3]

    In [6]: l[4]
    ---------------------------------------------------------------------------
    IndexError: list index out of range

    In [7]: l.foobar
    ---------------------------------------------------------------------------
    AttributeError: 'list' object has no attribute 'foobar'

Como puede ver, hay **diferentes tipos** de excepciones para diferentes errores.

Atrapando excepciones
---------------------

try/except
~~~~~~~~~~~

.. sourcecode:: ipython

    In [8]: while True:
       ...:     try:
       ...:         x = int(raw_input('Digite un numero: '))
       ...:         break
       ...:     except ValueError:
       ...:         print('No es un numero valido.  Pruebe de nuevo...')
       ...:         
    Digite un numero: a
    No es un numero valido.  Pruebe de nuevo...
    Digite un numero: 1

    In [9]: x
    Out[9]: 1

try/finally
~~~~~~~~~~~~

.. sourcecode:: ipython

    In [10]: try:
       ....:     x = int(raw_input('Digite un numero: '))
       ....: finally:
       ....:     print('Gracias')
       ....:     
    Digite un numero: a
    Gracias

 ---------------------------------------------------------------------------
    ValueError: invalid literal for int() with base 10: 'a'

Importante para gestión de recursos (por ejemplo, cerrar un archivo)

Es más fácil pedir perdón que permiso
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. sourcecode:: ipython

    In [11]: def imprime_ordenado(coleccion):
       ....:     try:
       ....:         coleccion.sort()
       ....:     except AttributeError:
       ....:         pass
       ....:     print coleccion
       ....:  

    In [12]: imprime_ordenado([1, 3, 2])
    [1, 2, 3]

    In [13]: imprime_ordenado(set((1, 3, 2)))
    set([1, 2, 3])

    In [14]: imprime_ordenado('132')
    132

Agregando excepciones
---------------------

* Capturando y agregando una excepción:

  .. sourcecode:: ipython

    In [15]: def filtro_nombre(nombre):
       ....:     try:
       ....:         nombre = nombre.encode('ascii')
       ....:     except UnicodeError, e:
       ....:         if nombre == 'Gaël':
       ....:             print('OK, Gaël')
       ....:         else:
       ....:             raise e
       ....:     return nombre
       ....: 

    In [16]: filtro_nombre('Gaël')
    OK, Gaël
    Out[16]: 'Ga\xc3\xabl'

    In [17]: filtro_nombre('Stéfan')
    ---------------------------------------------------------------------------
    UnicodeDecodeError: 'ascii' codec can't decode byte 0xc3 in position 2: ordinal not in range(128)

* Excepciones para pasar mensajes entre las partes del código:

  .. sourcecode:: ipython

    In [18]: def tendon_de_aquiles(x):
       ....:     if abs(x - 1) < 1e-3:
       ....:         raise StopIteration
       ....:     x = 1 - (1 - x)/2.0
       ....:     return x
       ....:

    In [19]: x = 0

    In [20]: while True:
       ....:     try:
       ....:         x = tendon_de_aquiles(x)
       ....:     except StopIteration:
       ....:         break
       ....:
       ....:

    In [21]: x
    Out[21]: 0.9990234375

Utilice excepciones para notificar determinadas condiciones que se cumplen (por ejemplo, StopIteration) o no se cumplen (por ejemplo, agregar errores personalizado)
