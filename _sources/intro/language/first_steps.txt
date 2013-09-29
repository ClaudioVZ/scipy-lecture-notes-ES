Primeros pasos
-------------


Inicie el shell **IPython** (shell Python interactivo mejorado):

* escriba "ipython" en una terminal Linux/Mac, o desde el shell cmd de Windows,
* **o** iniciando el programa desde un menú, por ejemplo, en `Python(x,y)`_ o `EPD`_ si ha instalado una distribución científica Python.

.. _`Python(x,y)`: http://www.pythonxy.com/
.. _`EPD`: http://www.enthought.com/products/epd.php

.. tip::

    Si usted no tiene IPython instalado en su equipo, otras shell Python están disponibles, como el shell python que se inicia escribiendo "python" en una terminal, o el intérprete Idle. Sin embargo, nos recomiendan usar el shell IPython debido a sus características mejoradas, especialmente para cálculo científico interactivo.

Después de iniciar el interprete, teclee::

    >>> print "Hola, mundo!"
    Hola, mundo!

.. tip::

    Si el mensaje "Hola, mundo!" fue mostrado. Usted acaba de ejecutar su primera instrucción Python, felicitaciones!

Para empezar, escriba las siguientes instrucciones::

    >>> a = 3
    >>> b = 2*a
    >>> type(b)
    <type 'int'>
    >>> print b
    6
    >>> a*b 
    18
    >>> b = 'hola' 
    >>> type(b)
    <type 'str'>
    >>> b + b
    'holahola'
    >>> 2*b
    'holahola'

.. tip::

  Las variables ``a`` y ``b`` se han definidon anteriormente. Tenga en cuenta que no se declarar el tipo de una variable antes de asignarle un valor. Por el contrario en C, se debe escribir:

  .. sourcecode:: c

      int a = 3;

  Además, el tipo de una variable puede cambiar, en el sentido de que en un punto del tiempo puede ser igual a un valor de un cierto tipo, y un segundo punto en el tiempo, puede ser igual a un valor de un tipo diferente. Primero `b` fue igual a un número entero, pero se convirtio en cadena cuando se le asignó el valor `'hola'`. Operaciones en enteros (``b = 2*a``) están codificados de forma nativa en Python, y también lo son algunas operaciones sobre cadenas, tales como adiciones y multiplicaciones, que son respectivamente, concatenación y repetición.
