Reusando código: scripts y módulos
==================================

Por ahora, hemos escrito todas las instrucciones en un intérprete. Para un mayor número de instrucciones debemos cambiar de rumbo y escribir código en archivos de texto (utilizando un editor de texto), que vamos a llamar a *scripts* o *módulos*. Utilice su editor de texto favorito (siempre que ofrezca un coloreado de sintaxis para Python) o el editor que viene con el con la distribución científica de Python que utiliza (por ejemplo, Scite en Python(x,y)).

Scripts
-------

.. tip:: Primero vamos a escribir un *script*, que es un archivo con una secuencia de instrucciones que se ejecutan cada vez que el script se llama. Las instrucciones pueden ser, por ejemplo código copiado y pegado desde el intérprete (pero tenga cuidado con las reglas de indentado!).

La extensión de los archivos Python son ``.py``. Escriba o copie y pegue las
siguientes líneas en un archivo llamado ``prueba.py``

.. code-block:: python

   mensaje = "Hola como estas?"
   for palabras in mensaje.split():
       print palabras

.. tip:: Ahora vamos a ejecutar el script de una forma interactiva, es decir, dentro del intérprete IPython. Este es quizás el uso más común de scripts en cálculo científico.

.. note:: en IPython, la sintaxis para ejecutar un script es ``%run script.py``. Por ejemplo,

   .. sourcecode:: ipython

      In [1]: %run prueba.py
      Hola
      como
      estas?

      In [2]: mensaje
      Out[2]: 'Hola como estas?'

   El script ha sido ejecutado. Por otra parte las variables definidas en el script (como ``mensaje``) están disponibles en el intérprete.

.. tip:: Otros intérpretes también ofrecen la posibilidad de ejecutar scripts(por ejemplo, ``execfile`` en el intérprete de Python estandar, etc.)

También es posible ejecutar este script como un *programa independiente*, mediante la ejecución de la secuencia de comandos en una terminal (consola Linux/Mac o la consola cmd de Windows). Por ejemplo, si estamos en la mismo directorio que el archivo prueba.py, se puede ejecutar esto en una consola:

.. sourcecode:: bash

   $ python prueba.py
   Hola
   como
   estas?

.. tip:: Los scripts independientes también pueden tener argumentos.

En ``archivo.py``

.. code-block:: python

   import sys
   print sys.argv

.. sourcecode:: bash

   $ python archivo.py argumento1 argumento2
   ['archivo.py', 'argumento1', 'argumento2']

.. note:: No implemente una opción de parseado usted mismo. Utilice módulos como ``optparse`` o ``argparse``.

Importando objectos desde módulos
---------------------------------

.. sourcecode:: ipython

   In [1]: import os

   In [2]: os
   Out[2]: <module 'os' from '/usr/lib/python2.7/os.pyc'>

   In [3]: os.listdir('.')
   Out[3]:
   ['conf.py',
    'basic_types.rst',
    'control_flow.rst',
    'functions.rst',
    'python_language.rst',
    'reusing.rst',
    'file_io.rst',
    'exceptions.rst',
    'workflow.rst',
    'index.rst']

O también:

.. sourcecode:: ipython

   In [4]: from os import listdir
   In [5]: listdir('.')
   Out[5]:
   ['conf.py',
    'basic_types.rst',
    'control_flow.rst',
    'functions.rst',
    'python_language.rst',
    'reusing.rst',
    'file_io.rst',
    'exceptions.rst',
    'workflow.rst',
    'index.rst']

Importando con abreviaturas:

.. sourcecode:: ipython

   In [6]: import numpy as np

.. warning::

   .. code-block:: python

      from os import *

   Esto se conoce como *importar una estrella* y por favor, **Debe usarlo con precaución**

   * Hace que el código sea más difícil de leer y entender: de dónde vienen estas variables?

   * Hace que sea imposible adivinar su funcionalidad por contexto y nombre (pista: `os.name` es el nombre del sistema operativo), y utilizar el autocompletado con tabulador.

   * Limita los nombres de las variables que se pueden utilizar: `os.name` podría anular `name`, o vice-versa.

    * Crea posibles conflictos de nombres entre módulos.

    * Hace que el código sea imposible de comprobar estáticamente para variables indefinidas.

.. tip:: Los módulos son una buena forma de organizar el código de una manera jerárquica. En realidad, todas las herramientas de cálculo científico que utilizamos son módulos

.. code-block:: python

   >>> import numpy as np # arreglos optimizados
   >>> np.linspace(0, 10, 6)
   array([  0.,   2.,   4.,   6.,   8.,  10.])
   >>> import scipy # cálculo científico 

En Python(x,y), Ipython(x,y) estos módulos se importan automaticamente al inicio:

.. code-block:: python

   >>> import numpy
   >>> import numpy as np
   >>> from pylab import *
   >>> import scipy

y no es necesario volver a importar estos módulos.

Creando módulos
---------------

.. tip:: Si queremos escribir programas organizados más grandes y mejores (comparados con scripts simples), donde se definen algunos objetos, (variables, funciones, clases) y queremos volver a utilizar varias veces, tenemos crear nuestros propios *módulos*.

Vamos a crear el módulo ``demo`` contenido en el archivo ``demo.py``:

.. literalinclude:: demo.py

.. tip:: En este archivo, definimos dos funciones ``print_a`` y ``print_b``. Si queremos llamar a la función ``print_a`` desde el intérprete. Podríamos ejecutar el archivo como un script, pero ya que sólo se quiere tener acceso a la función ``print_a``, es mejor **importarlo como un módulo**.
   La sintaxis es la siguiente.

.. sourcecode:: ipython

   In [1]: import demo

   In [2]: demo.print_a()
   a

   In [3]: demo.print_b()
   b

Importar un módulo permite el acceso a sus objetos, utilizando la sintaxis ``módulo.objecto``. No se olvide de poner el nombre del módulo antes del nombre del objeto, de lo contrario Python no reconocerá la instrucción.

Introspección

.. sourcecode:: ipython

   In [4]: demo?
   Type:       module
   String Form:<module 'demo' from 'demo.py'>
   File:       /home/varoquau/Projects/Python_talks/scipy_2009_tutorial/source/demo.py
   Docstring:  Módulo demo.


   In [5]: who
   demo

   In [6]: whos
   Variable   Type      Data/Info
   ------------------------------
   demo       module    <module 'demo' from 'demo.py'>

   In [7]: dir(demo)
   Out[7]:
   ['__builtins__',
   '__doc__',
   '__file__',
   '__name__',
   '__package__',
   'c',
   'd',
   'print_a',
   'print_b']


   In [8]: demo.
   demo.__builtins__      demo.__init__          demo.__str__
   demo.__class__         demo.__name__          demo.__subclasshook__
   demo.__delattr__       demo.__new__           demo.c
   demo.__dict__          demo.__package__       demo.d
   demo.__doc__           demo.__reduce__        demo.print_a
   demo.__file__          demo.__reduce_ex__     demo.print_b
   demo.__format__        demo.__repr__          demo.py
   demo.__getattribute__  demo.__setattr__       demo.pyc
   demo.__hash__          demo.__sizeof__

Importando objetos a partir de módulos en el espacio de nombres principal

.. sourcecode:: ipython

   In [9]: from demo import print_a, print_b

   In [10]: whos
   Variable   Type        Data/Info
   --------------------------------
   demo       module      <module 'demo' from 'demo.py'>
   print_a    function    <function print_a at 0xb7421534>
   print_b    function    <function print_b at 0xb74214c4>

   In [11]: print_a()
   a

.. warning:: **Módulos almacenados en caché**

   Los módulos se almacenan en caché: si modifica ``demo.py`` y lo vuelve a importar, obtendrá el módulo antiguo.

   Solución:

   .. sourcecode :: ipython

      In [10]: reload(demo)
      Out[10]: <module 'demo' from 'demo.pyc'>

'__main__' y cargando módulos
-----------------------------

Archivo ``demo2.py``:

.. literalinclude:: demo2.py

Importando:

.. sourcecode:: ipython

   In [11]: import demo2
   b

   In [12]: import demo2

Ejecutando:

.. sourcecode:: ipython

   In [13]: %run demo2
   b
   a

Scripts o módulos? Cómo organizar su código
-------------------------------------------

.. note:: Regla de oro

   * Los conjuntos de instrucciones que se llaman varias veces deben estar escritos dentro de **funciones** para una mejor reutilización del código.

   * Funciones (u otras partes de código) que se llaman varias veces desde scripts deben ser escritos dentro de un **módulo**, de modo que el módulo es importado en los demás scripts (no copie y pegue sus funciones en los demás scripts!).

Cómo encontrar módulos e importarlos
....................................

Cuando se ejecuta ``import mimodulo``, el módulo ``mimodulo`` se busca en una lista de directorios. Esta lista incluye por defecto una lista de la ruta de instalación (por ejemplo, ``/usr/lib/python``) así como la lista de los directorios especificados por la variable de entorno ``PYTHONPATH``.

La lista de directorios en los que busca Python viene dada por la variable ``sys.path`` 

.. sourcecode:: ipython

   In [1]: import sys

   In [2]: sys.path
   Out[2]: 
   ['',
    '/home/varoquau/.local/bin',
    '/usr/lib/python2.7',
    '/home/varoquau/.local/lib/python2.7/site-packages',
    '/usr/lib/python2.7/dist-packages',
    '/usr/local/lib/python2.7/dist-packages',
    ...]

Los módulos deben estar ubicados en la ruta de búsqueda, por lo tanto, se puede:

* Escribir sus propios módulos dentro de los directorios que ya están definidas en el ruta de búsqueda (por ejemplo, ``$HOME/.local/lib/python2.7/dist-packages``). También puede usar enlaces simbólicos (en Linux) para mantener el código de otro sitio.

* Modificar la variable de entorno ``PYTHONPATH`` para incluir el directorios que contienen los módulos definidos por el usuario.

.. tip:: En Linux/Unix, agregue la siguiente línea en un archivo para que sea leido por el shell al inicio (por ejemplo, /etc/profile, . profile)

   .. code-block:: python  
      export PYTHONPATH=$PYTHONPATH:/home/emma/user_defined_modules

En Windows, http://support.microsoft.com/kb/310519 explica cómo manejar las variables de entorno.

* o modifique la variable ``sys.path`` con un script Python.

.. tip::

   .. code-block:: python

      import sys
      nuevo_path = '/home/emma/user_defined_modules'
      if nuevo_path not in sys.path:
          sys.path.append(nuevo_path)

   Este método no es muy robusto, sin embargo, hace que el código sea menos portable (la ruta depende del usuario) y porque hay que añadir el directorio a su sys.path cada vez que desee importar un módulo en este directorio.
   
   Vea http://docs.python.org/tutorial/modules.html para más información acerca de los módulos.

Paquetes
--------

Un directorio que contiene muchos módulos se llama un *paquete*. Un paquete
es un módulo con submódulos (submódulos con submódulos, etc).
Un archivo especial llamado ``__init__.py`` (que puede estar vacío) le dice a Python que el directorio es un paquete Python, del cual los módulos pueden ser importados.

.. sourcecode:: bash

   $ ls
   cluster/        io/          README.txt@     stsci/
   __config__.py@  LATEST.txt@  setup.py@       __svn_version__.py@
   __config__.pyc  lib/         setup.pyc       __svn_version__.pyc
   constants/      linalg/      setupscons.py@  THANKS.txt@
   fftpack/        linsolve/    setupscons.pyc  TOCHANGE.txt@
   __init__.py@    maxentropy/  signal/         version.py@
   __init__.pyc    misc/        sparse/         version.pyc
   INSTALL.txt@    ndimage/     spatial/        weave/
   integrate/      odr/         special/
   interpolate/    optimize/    stats/
   $ cd ndimage
   $ ls
   doccer.py@   fourier.pyc   interpolation.py@  morphology.pyc   setup.pyc
   doccer.pyc   info.py@      interpolation.pyc  _nd_image.so
   setupscons.py@
   filters.py@  info.pyc      measurements.py@   _ni_support.py@
   setupscons.pyc
   filters.pyc  __init__.py@  measurements.pyc   _ni_support.pyc  tests/
   fourier.py@  __init__.pyc  morphology.py@     setup.py@


Desde Ipython:

.. sourcecode:: ipython

   In [1]: import scipy

   In [2]: scipy.__file__
   Out[2]: '/usr/lib/python2.7/dist-packages/scipy/__init__.pyc'

   In [3]: scipy.__version__
   Out[3]: '0.9.0'

   In [4]: import scipy.ndimage.morphology

   In [5]: from scipy.ndimage import morphology

   In [6]: morphology.binary_dilation?
   Type:       function
   String Form:<function binary_dilation at 0x9646294>
   File:       /usr/lib/python2.7/dist-packages/scipy/ndimage/morphology.py
   Definition: morphology.binary_dilation(input, structure=None, iterations=1, mask=None, output=None, border_value=0, origin=0, brute_force=False)

Buenas practicas
----------------

* Use **nombres significativos** para los objetos

* **Indentado: no es opcional!**

  .. tip:: El indentado es obligatorio en Python! Cada bloque de comandos después de un ``:`` aumenta un nivel de indentado adicional con respecto a la línea anterior. Como también, después de ``def f():`` o ``while:``. Al final de tales bloques lógicos, se debera disminuir la profundidad de indentado (o aumentar si se introduce un nuevo bloque, etc)

     El uso estricto del indentado es el precio a pagar por deshacerse de ``{`` or ``;`` caracteres que delimitan bloques lógicos en otros lenguajes. La indentación inadecuada produce errores tales como:

     .. sourcecode:: ipython

        ------------------------------------------------------------
        IndentationError: unexpected indent (prueba.py, line 2)

     El indentado puede ser un poco confuso al principio. Sin embargo, con una clara indentación y en ausencia de caracteres extra, el código es muy agradable de leer en comparación con otros lenguajes.

* **Profundidad de indentado**: En un editor de texto, el indentado puede ser cualquier número positivo de espacios (1, 2, 3, 4, ...). Sin embargo, se considera una buena práctica **indentar con 4 espacios**. Usted puede configurar el editor para asignar a la tecla ``tab`` un indentado de 4 espacio. En Python(x,y), el editor es ya configurado de esa manera.

* **Normas de estilo**

  **Líneas Largas**: no se debe escribir líneas muy largas que se extiendan por más de (por ejemplo) 80 caracteres. Las líneas largas se pueden dividir con el carácter ``\``
  
  .. code-block:: python

     >>> linea_larga = "Esta una línea muy muy larga \
     ... que se divide en dos partes."

  **Espacios**

  Escriba código bien espaciado: ponga espacios en blanco después de las comas, ponga espacios en blanco antes y después de los operadores aritméticos, etc.

  .. code-block:: python

     >>> a = 1 # si
     >>> a=1 # demasiado estrecho

  Un cierto número de normas para escribir código ``hermoso`` (y lo más importante el uso de la misma convención para cualquier persona!) se dan en `Style Guide for Python Code <http://www.python.org/dev/peps/pep-0008>`_.
