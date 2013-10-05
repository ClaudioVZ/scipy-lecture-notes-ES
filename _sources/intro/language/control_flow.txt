Control de flujo
================

Controla el orden en el que se ejecuta el código.

if/elif/else
------------

.. code-block:: python

   >>> if 2**2 == 4:
   ...     print 'Obvio!'
   ...
   Obvio!

.. code-block:: python

   >>> a = -1
   >>> if a > 0:
   ...     print 'Número positivo'
   ... else:
   ...     print 'Número negativo'
   ...
   Número negativo

**Los bloques de código son delimitados por indentación**

.. tip:: Escriba las siguientes líneas en el intérprete de Python, y tenga cuidado **respecto a la profundidad de indentación**. La consola IPython aumenta automáticamente la profundidad de indentado una columna después del signo ``:``, para disminuir la profundidad de indentado, presione la tecla de retroceso o la flecha izquierda. Pulse la tecla Intro dos veces para salir del bloque lógico.

.. sourcecode:: ipython

   In [1]: a = 10

   In [2]: if a == 1:
      ...:     print 1
      ...: elif a == 2:
      ...:     print 2
      ...: else:
      ...:     print('Diferente de 1 y 2')
      ...:
   Diferente de 1 y 2

La indentación es obligatoria en scripts. Como ejercicio, reescriba la líneas anteriores en el script ``indentado.py`` y ejecutelo en IPython usando ``run``.

for/range
----------

Iterando con indices

.. code-block:: python

   >>> for i in range(4):
   ...     print(i)
   0
   1
   2
   3

A menudo, el código es más legible si se itera sobre valores:

.. code-block:: python

   >>> for palabra in ('interesante', 'poderoso', 'legible'):
   ...     print('Python es %s' % palabra)
   Python es interesante
   Python es poderoso
   Python es legible

while/break/continue
---------------------

Bucle while al estilo C (problema de Mandelbrot)

.. code-block:: python

   >>> z = 1 + 1j
   >>> while abs(z) < 100:
   ...     z = z**2 + 1
   >>> z
   (-134+352j)

**Características más avanzadas**

``break`` sale del bucle encerrado por for/while

.. code-block:: python

   >>> z = 1 + 1j
   >>> while abs(z) < 100:
   ...     if z.imag == 0:
   ...         break
   ...     z = z**2 + 1

``continue`` la siguiente iteración de un bucle.

.. code-block:: python

   >>> a = [1, 0, 2, 4]
   >>> for element in a:
   ...     if element == 0:
   ...         continue
   ...     print 1. / element
   1.0
   0.5
   0.25

Expresiones condicionales
-------------------------

:``if <OBJECT>``:

 Se evalúa como False:
  * Cualquier número igual a cero (0, 0.0, 0+0j)
  * Un contenedor vacío (lista, tupla, conjunto, diccionario, ...)
  * ``False``, ``None``

 Se evalúa como True:
  * Todo lo demás

:``a == b``:

 Prueba de igualdad, con operadores lógicos

 .. code-block:: python

    >>> 1 == 1.
    True

:``a is b``:

 Prueba de identidad: ambos lados son el mismo objeto

 .. code-block:: python

    >>> 1 is 1.
    False
    >>> a = 1
    >>> b = 1
    >>> a is b
    True

:``a in b``:

 Para todas las colecciones ``b``: ``b`` contiene ``a``

 .. code-block:: python

    >>> b = [1, 2, 3]
    >>> 2 in b
    True
    >>> 5 in b
    False

 Si ``b`` es un diccionario, se prueba si ``a`` es un clave en ``b``.

Iteración avanzada
------------------

Iterando sobre una *secuencia*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Se puede iterar sobre una secuencia (cadenas, listas, claves en un diccionario, lineas en un archivo, ...)

.. code-block:: python

   >>> vocales = 'aeiou'
   >>> for i in 'poderoso':
   ...     if i in vocales:
   ...         print i,
   o e o o

.. code-block:: python

   >>> mensaje = "Hola como estas?"
   >>> mensaje.split() # devuelve una lista
   ['Hola', 'como', 'estas?']
   >>> for palabra in mensaje.split():
   ...     print palabra
   ...
   Hola
   como
   estas?

.. tip:: Pocos lenguajes (en particular, los lenguajes de computación científica) permiten bucles sobre cualquier cosa menos sobre enteros/índices. Con Python es posible hacer un bucle sobre los objetos de interés sin preocuparse por los índices que a menudo no importan. Esta característica hace al código más legible.

.. warning:: No es seguro modificar la secuencia que se está iterando.

Seguimiento de una enumeración
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Una tarea común es iterar sobre una secuencia mientras se enumera los elementos.

* Puede utilizarse un bucle while con un contador como el ejemplo anterior. O un bucle for

.. code-block:: python

   >>> palabras = ('interesante', 'poderoso', 'legible')
   >>> for indice in range(0, len(palabras)):
   ...     print indice, palabras[indice] 
   0 interesante
   1 poderoso
   2 legible

* Pero, Python provee la palabra clave ``enumerate``

.. code-block:: python

   >>> for indice, elemento in enumerate(palabras):
   ...     print indice, elemento
   0 estupendo
   1 poderoso
   2 legible

Bucle sobre un diccionario
~~~~~~~~~~~~~~~~~~~~~~~~~~

Use **iteritems**

.. code-block:: python

   >>> d = {'a': 1, 'b':1.2, 'c':1j}
   >>> for clave, valor in d.iteritems():
   ...     print('Clave: %s con valor: %s' % (clave, valor))
   Clave: a con valor: 1
   Clave: c con valor: 1j
   Clave: b con valor: 1.2

Listas por comprensión
----------------------

.. code-block:: python

   >>> [elemento**2 for elemento in range(4)]
   [0, 1, 4, 9]

_____


.. topic:: Ejercicio
   :class: green

   Calcular los decimales de Pi usando la formula de Wallis:

   .. math::
       \pi = 2 \prod_{i=1}^{\infty} \frac{4i^2}{4i^2 - 1}

.. :ref:`pi_wallis`
