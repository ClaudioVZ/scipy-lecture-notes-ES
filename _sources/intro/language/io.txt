Entrada y Salida
================

Para ser exhaustivos, he aquí algo de información sobre la entrada y salida en Python.

Solamente las **cadenas** se escriben o leen a/desde archivos (otros tipos deben convertirse a cadenas). Para escribir en un archivo::

    >>> f = open('archivo_ejemplo', 'w') # creo un archivo
    >>> type(f)
    <type 'file'>
    >>> f.write('Esto es una prueba\ny otra prueba')
    >>> f.close()

Para leer un archivo

.. sourcecode:: ipython

    In [1]: f = open('archivo_ejemplo', 'r')

    In [2]: s = f.read()

    In [3]: print s
    Esto es una prueba 
    y otra prueba

    In [4]: f.close()

Para más detalles: http://docs.python.org/tutorial/inputoutput.html

Iterando sobre un archivo
~~~~~~~~~~~~~~~~~~~~~~~~~

.. sourcecode:: ipython

    In [6]: f = open('archivo_ejemplo', 'r')

    In [7]: for linea in f:
    ...:     print linea
    ...:     
    Esto es una prueba

    y otra prueba

    In [8]: f.close()

Modos de archivo
----------------

* Solamente lectura: ``r``
* Solamente escritura: ``w``

  * Nota: Crear un nuevo archivo o *sobreescribir* el archivo existente.

* Agregar al archivo: ``a``
* Leer y escribir: ``r+``
* Modo binario: ``b``

  * Nota: Usar para archivos binarios, especialmente en Windows.

