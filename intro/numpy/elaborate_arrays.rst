.. For doctests

   >>> import numpy as np
   >>> np.random.seed(0)
   >>> from matplotlib import pyplot as plt

.. currentmodule:: numpy

Arreglos más elaborados
=======================

.. contents:: Contenido
    :local:
    :depth: 1

Más tipos de datos
------------------

Conversión de tipos
...................

El tipo "más grande" prevalece en las operaciones de tipo mixto

.. code-block:: python

   >>> np.array([1, 2, 3]) + 1.5
   array([ 2.5,  3.5,  4.5])

La asignación no cambia el tipo!

.. code-block:: python

   >>> a = np.array([1, 2, 3])
   >>> a.dtype
   dtype('int32')
   >>> a[0] = 1.9     # <-- número de punto flotante truncado a número entero
   >>> a
   array([1, 2, 3])

Conversión forzada

.. code-block:: python

   >>> a = np.array([1.7, 1.2, 1.6])
   >>> b = a.astype(int)  # <-- truncado a entero
   >>> b
   array([1, 1, 1])

Redondeo

.. code-block:: python

   >>> a = np.array([1.2, 1.5, 1.6, 2.5, 3.5, 4.5])
   >>> b = np.around(a)
   >>> b                    # sigue siendo de punto flotante
   array([ 1.,  2.,  2.,  2.,  4.,  4.])
   >>> c = np.around(a).astype(int)
   >>> c
   array([1, 2, 2, 2, 4, 4])

Tamaños de tipos de datos
.........................

Enteros (con signo):

=================== ==============================================================
:class:`int8`        8 bits
:class:`int16`       16 bits
:class:`int32`       32 bits (el mismo :class:`int` en platforma 32-bit)
:class:`int64`       64 bits (el mismo :class:`int` en platforma 64-bit)
=================== ==============================================================

.. code-block:: python

   >>> np.array([1], dtype=int).dtype
   dtype('int64')
   >>> np.iinfo(np.int32).max, 2**31 - 1
   (2147483647, 2147483647)
   >>> np.iinfo(np.int64).max, 2**63 - 1
   (9223372036854775807, 9223372036854775807L)

Enteros sin signo:

=================== ==============================================================
:class:`uint8`       8 bits
:class:`uint16`      16 bits
:class:`uint32`      32 bits
:class:`uint64`      64 bits
=================== ==============================================================

.. code-block:: python

   >>> np.iinfo(np.uint32).max, 2**32 - 1
   (4294967295, 4294967295)
   >>> np.iinfo(np.uint64).max, 2**64 - 1
   (18446744073709551615L, 18446744073709551615L)

Números de punto flotante:

=================== ==============================================================
:class:`float16`     16 bits
:class:`float32`     32 bits
:class:`float64`     64 bits (el mismo :class:`float`)
:class:`float96`     96 bits, dependiente de la plataforma (el mismo :class:`np.longdouble`)
:class:`float128`    128 bits, dependiente de la plataforma (el mismo :class:`np.longdouble`)
=================== ==============================================================

.. code-block:: python

   >>> np.finfo(np.float32).eps
   1.1920929e-07
   >>> np.finfo(np.float64).eps
   2.2204460492503131e-16

   >>> np.float32(1e-8) + np.float32(1) == 1
   True
   >>> np.float64(1e-8) + np.float64(1) == 1
   False

Números complejos de punto flotante:

=================== ==============================================================
:class:`complex64`   dos números de punto flotante 32-bit
:class:`complex128`  dos números de punto flotante 64-bit
:class:`complex192`  dos números de punto flotante 96-bit, dependiente de la plataforma
:class:`complex256`  dos números de punto flotante 128-bit, dependiente de la plataforma
=================== ==============================================================

.. topic:: Tipos de datos pequeños

   Si usted no sabe si necesita los tipos de datos especiales, es probable que no los conozca.

   Comparación sobre el uso de ``float32`` en vez de ``float64``:

   - La mitad del tamaño en la memoria y en el disco
   - La mitad del ancho de banda de memoria necesaria (puede ser un poco más rápido en algunas operaciones )

     .. sourcecode:: ipython

        In [1]: a = np.zeros((1e6,), dtype=np.float64)

        In [2]: b = np.zeros((1e6,), dtype=np.float32)

        In [3]: %timeit a*a
        1000 loops, best of 3: 1.78 ms per loop

        In [4]: %timeit b*b
        1000 loops, best of 3: 1.07 ms per loop

   - Pero: con grandes errores de redondeo --- a veces en lugares sorprendentes
     (es decir, no lo utilice a menos que realmente lo necesite)

Tipos de datos estructurados
----------------------------

=============== ====================
``codigo_sensor``  (cadena de 4 caracteres)
``posicion``     (número de punto flotante)
``valor``        (número de punto flotante)
=============== ====================

.. code-block:: python

   >>> muestras = np.zeros((6,), dtype=[('codigo_sensor', 'S4'),
   ...                                  ('posicion', float), ('valor', float)])
   >>> muestras.ndim
   1
   >>> muestras.shape
   (6,)
   >>> muestras.dtype.names
   ('codigo_sensor', 'posicion', 'valor')
   >>> muestras[:] = [('ALFA',   1, 0.37), ('BETA', 1, 0.11), ('TAU', 1,   0.13),
   ...                ('ALFA', 1.5, 0.37), ('ALFA', 3, 0.11), ('TAU', 1.2, 0.13)]
   >>> muestras
   array([('ALFA', 1.0, 0.37), ('BETA', 1.0, 0.11), ('TAU', 1.0, 0.13),
          ('ALFA', 1.5, 0.37), ('ALFA', 3.0, 0.11), ('TAU', 1.2, 0.13)], 
         dtype=[('codigo_sensor', 'S4'), ('posicion', '<f8'), ('valor', '<f8')])

Para acceder a los campos se debe indexar los nombres de los campos

.. code-block:: python

   >>> muestras['codigo_sensor']
   array(['ALFA', 'BETA', 'TAU', 'ALFA', 'ALFA', 'TAU'], 
         dtype='|S4')
   >>> muestras['valor']
   array([ 0.37,  0.11,  0.13,  0.37,  0.11,  0.13])
   >>> muestras[0]
   ('ALFA', 1.0, 0.37)
   >>> muestras[0]['codigo_sensor']
   >>> 'ALFA'
   >>> muestras[0]['codigo_sensor'] = 'TAU'
   >>> samples[0]
   ('TAU', 1.0, 0.37)

Campos múltiples a la vez

.. code-block:: python

   >>> muestras[['posicion', 'valor']]
   array([(1.0, 0.37), (1.0, 0.11), (1.0, 0.13), (1.5, 0.37), (3.0, 0.11),
          (1.2, 0.13)], 
         dtype=[('posicion', '<f8'), ('valor', '<f8')])

Usando indexado fancy, en la forma usual

.. code-block:: python

   >>> muestras[muestras['codigo_sensor'] == 'ALFA']
   array([('ALFA', 1.5, 0.37), ('ALFA', 3.0, 0.11)], 
         dtype=[('codigo_sensor', 'S4'), ('posicion', '<f8'), ('valor', '<f8')])

.. note:: Hay un montón de otras sintaxis para construir arreglos estructurados, consulte `aquí <http://docs.scipy.org/doc/numpy/user/basics.rec.html>`__ y `aquí <http://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html#specifying-and-constructing-data-types>`__.

:class:`maskedarray`: tratando con datos que faltan
---------------------------------------------------

* Para números de punto flotante se podría utilizar NaN , pero las máscaras funcionan para todos los tipos

  .. code-block:: python

     >>> x = np.ma.array([1, 2, 3, 4], mask=[0, 1, 0, 1])
     >>> x
     masked_array(data = [1 -- 3 --],
                  mask = [False  True False  True],
            fill_value = 999999)
     <BLANKLINE>

     >>> y = np.ma.array([1, 2, 3, 4], mask=[0, 1, 1, 1])
     >>> x + y
     masked_array(data = [2 -- -- --],
                  mask = [False  True  True  True],
            fill_value = 999999)
     <BLANKLINE>

* Versión enmascarada de funciones comunes

  .. code-block:: python

     >>> np.ma.sqrt([1, -1, 2, -2])
     masked_array(data = [1.0 -- 1.41421356237 --],
                  mask = [False  True False  True],
            fill_value = 1e+20)
     <BLANKLINE>

.. note:: Hay otros útiles :ref:`array siblings <array_siblings>`
_____

Este tema esta fuera de los capítulos sobre Numpy, tomemos un momento para
recordar las buenas prácticas de codificación, lo que realmente vale la pena a largo plazo:

.. topic:: Buenas prácticas

   * Nombres de variables explícitos (sin necesidad de un comentario que explique lo que está en la variable)

   * Estilo: espacio después de las comas, antes y despues de ``=``, etc.

   Un cierto número de reglas para escribir código ``hermoso`` (y más importante, el uso de las mismas convenciones para todos los demás!) están en `Style Guide for Python Code <http://www.python.org/dev/peps/pep-0008>`_ y `Docstring Conventions <http://www.python.org/dev/peps/pep-0257>`_ (manejo de cadenas de ayuda).

   * Excepto en algunos casos raros, los nombres de variables y comentarios en inglés.
