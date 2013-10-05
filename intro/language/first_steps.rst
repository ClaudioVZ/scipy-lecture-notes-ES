Primeros pasos
--------------

Inicie la consola **IPython** (consola mejorada interactiva Python):

* Escriba "ipython" en una terminal Linux/Mac, o desde la consola cmd de Windows.
* O iniciando el programa desde un menú, por ejemplo, en `Python(x,y)`_ o `EPD`_ si ha instalado una distribución científica Python.

.. _`Python(x,y)`: http://www.pythonxy.com/
.. _`EPD`: http://www.enthought.com/products/epd.php

.. tip::

Si usted no tiene IPython instalado en su equipo, otras consolas Python están disponibles, como la consola estandar python que se inicia escribiendo "python" en una terminal, o el intérprete Idle. Sin embargo, nos recomiendan usar la consola IPython debido a sus características mejoradas, especialmente para computación científica interactiva.

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

Tenga en cuenta que las variables ``a`` y ``b`` no se declararon su tipo antes de asignarles un valor. Por el contrario en C, se debe escribir:

  .. sourcecode:: c

      int a = 3;

Además, el tipo de una variable puede cambiar, en el sentido de que por un periodo de tiempo puede ser igual a un valor de un cierto tipo, y en otro periodo tiempo puede ser igual a otro valor de tipo diferente. Primero `b` fue igual a un número entero, pero se convirtio en cadena cuando se le asignó el valor ``hola``. Las operaciones con enteros (``b = 2*a``) están codificados de forma nativa en Python, y también lo son algunas operaciones sobre cadenas, tales como adiciones y multiplicaciones, que son respectivamente, concatenación y repetición.
