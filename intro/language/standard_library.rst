Biblioteca estándar
===================

.. note:: Documentos de referencia para esta sección:

  * The Python Standard Library documentation:
    http://docs.python.org/library/index.html

  * Python Essential Reference, David Beazley, Addison-Wesley Professional

``os`` módulo: funciones del sistema operativo
----------------------------------------------

*La manera portátil de usar el sistema operativo depende de sus funciones.* 

Manipulación de directorios y archivos
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Carpeta actual:

.. sourcecode:: ipython

   In [16]: import os

   In [17]: os.getcwd()
   Out[17]: '/Users/cburns/src/scipy2009/scipy_2009_tutorial/source'

Lista de carpetas:

.. sourcecode:: ipython

   In [31]: os.listdir(os.curdir)
   Out[31]:
   ['.index.rst.swo',
    '.python_language.rst.swp',
    '.view_array.py.swp',
    '_static',
    '_templates',
    'basic_types.rst',
    'conf.py',
    'control_flow.rst',
    'debugging.rst',
    ...

Creando una carpeta:

.. sourcecode:: ipython

   In [32]: os.mkdir('junkdir')

   In [33]: 'junkdir' in os.listdir(os.curdir)
   Out[33]: True

Renombrando una carpeta:

.. sourcecode:: ipython

   In [36]: os.rename('junkdir', 'foodir')

   In [37]: 'junkdir' in os.listdir(os.curdir)
   Out[37]: False

   In [38]: 'foodir' in os.listdir(os.curdir)
   Out[38]: True

Borrando una carpeta:

.. sourcecode:: ipython

   In [41]: os.rmdir('foodir')

   In [42]: 'foodir' in os.listdir(os.curdir)
   Out[42]: False

Creando un archivo:

.. sourcecode:: ipython

   In [44]: fp = open('junk.txt', 'w')

   In [45]: fp.close()

   In [46]: 'junk.txt' in os.listdir(os.curdir)
   Out[46]: True

Borrando un archivo:

.. sourcecode:: ipython

   In [47]: os.remove('junk.txt')

   In [48]: 'junk.txt' in os.listdir(os.curdir)
   Out[48]: False

``os.path``: manipulanado el path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``os.path`` proporciona operaciones comunes con el path.

.. sourcecode:: ipython

   In [70]: fp = open('junk.txt', 'w')

   In [71]: fp.close()

   In [72]: a = os.path.abspath('junk.txt')

   In [73]: a
   Out[73]: '/Users/cburns/src/scipy2009/scipy_2009_tutorial/source/junk.txt'

   In [74]: os.path.split(a)
   Out[74]: ('/Users/cburns/src/scipy2009/scipy_2009_tutorial/source',
              'junk.txt')

   In [78]: os.path.dirname(a)
   Out[78]: '/Users/cburns/src/scipy2009/scipy_2009_tutorial/source'

   In [79]: os.path.basename(a)
   Out[79]: 'junk.txt'

   In [80]: os.path.splitext(os.path.basename(a))
   Out[80]: ('junk', '.txt')

   In [84]: os.path.exists('junk.txt')
   Out[84]: True

   In [86]: os.path.isfile('junk.txt')
   Out[86]: True

   In [87]: os.path.isdir('junk.txt')
   Out[87]: False

   In [88]: os.path.expanduser('~/local')
   Out[88]: '/Users/cburns/local'

   In [92]: os.path.join(os.path.expanduser('~'), 'local', 'bin')
   Out[92]: '/Users/cburns/local/bin'

Ejecutando un comando externo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. sourcecode:: ipython

   In [8]: os.system('ls')
   basic_types.rst   demo.py          functions.rst  python_language.rst  standard_library.rst
   control_flow.rst  exceptions.rst   io.rst         python-logo.png
   demo2.py          first_steps.rst  oop.rst        reusing_code.rst

.. note:: Alternativa a ``os.system``

   Una notable alternativa a ``os.system`` es el `módulo sh <http://amoffat.github.com/sh/>`_. Que proporciona una manera mucho más conveniente de obtener la salida, flujo de errores y código a la salida de un comando externo.

   .. sourcecode:: ipython

       In [20]: import sh
       In [20]: com = sh.ls()

       In [21]: print com
       basic_types.rst   exceptions.rst   oop.rst              standard_library.rst
       control_flow.rst  first_steps.rst  python_language.rst
       demo2.py          functions.rst    python-logo.png
       demo.py           io.rst           reusing_code.rst

       In [22]: print com.exit_code
       0
       In [23]: type(com)
       Out[23]: sh.RunningCommand

Recorrer una carpeta
~~~~~~~~~~~~~~~~~~~~

``os.path.walk`` genera una lista de nombres de archivos en un árbol de carpetas.

.. sourcecode:: ipython

   In [10]: for dirpath, dirnames, filenames in os.walk(os.curdir):
      ....:     for fp in filenames:
      ....:         print os.path.abspath(fp)
      ....:
      ....:
   /Users/cburns/src/scipy2009/scipy_2009_tutorial/source/.index.rst.swo
   /Users/cburns/src/scipy2009/scipy_2009_tutorial/source/.view_array.py.swp
   /Users/cburns/src/scipy2009/scipy_2009_tutorial/source/basic_types.rst
   /Users/cburns/src/scipy2009/scipy_2009_tutorial/source/conf.py
   /Users/cburns/src/scipy2009/scipy_2009_tutorial/source/control_flow.rst
   ...

Variables de entorno:
~~~~~~~~~~~~~~~~~~~~~

.. sourcecode:: ipython

   In [9]: import os

   In [11]: os.environ.keys()
   Out[11]:
   ['_',
    'FSLDIR',
    'TERM_PROGRAM_VERSION',
    'FSLREMOTECALL',
    'USER',
    'HOME',
    'PATH',
    'PS1',
    'SHELL',
    'EDITOR',
    'WORKON_HOME',
    'PYTHONPATH',
    ...

   In [12]: os.environ['PYTHONPATH']
   Out[12]: '.:/Users/cburns/src/utils:/Users/cburns/src/nitools:
   /Users/cburns/local/lib/python2.5/site-packages/:
   /usr/local/lib/python2.5/site-packages/:
   /Library/Frameworks/Python.framework/Versions/2.5/lib/python2.5'

   In [16]: os.getenv('PYTHONPATH')
   Out[16]: '.:/Users/cburns/src/utils:/Users/cburns/src/nitools:
   /Users/cburns/local/lib/python2.5/site-packages/:
   /usr/local/lib/python2.5/site-packages/:
   /Library/Frameworks/Python.framework/Versions/2.5/lib/python2.5'

``shutil``: Operaciones de alto nivel con archivos
--------------------------------------------------

El módulo ``shutil`` proporciona operaciones útiles con archivos:

    * ``shutil.rmtree``: Borrado recursivo de un árbol de directorios.
    * ``shutil.move``: Copia recursiva de un archivo o directorio a otra ubicación.
    * ``shutil.copy``: Copiar archivos o directorios.

``glob``: Coincidencia de patrones en archivos
----------------------------------------------

El módulo ``glob`` provee búsqueda por coincidencia de patrones en archivos.

Buscar todos los archivos que terminen en ``.txt``:

.. sourcecode:: ipython

   In [18]: import glob

   In [19]: glob.glob('*.txt')
   Out[19]: ['holy_grail.txt', 'junk.txt', 'newfile.txt']

``sys``: Información específica del sistema
-------------------------------------------

Sistema de información específica relacionada con el intérprete de Python.

* Qué versión de Python estás ejecutando y donde está instalado:

  .. sourcecode:: ipython

     In [116]: import sys

     In [117]: sys.platform
     Out[117]: 'linux2'

     In [118]: sys.version
     Out[118]: '2.7.3 (default, Apr 10 2013, 05:46:21) \n[GCC 4.6.3]'

     In [119]: sys.prefix
     Out[119]: '/usr'

* Lista de argumentos de la línea de comandos pasados ​​a un script Python:

  .. sourcecode:: ipython

     In [120]: sys.argv
     Out[120]: ['/usr/bin/ipython']

``sys.path`` es una lista de cadenas que especifica la ruta de búsqueda de
módulos. Iniciada desde PYTHONPATH:

.. sourcecode:: ipython

   In [121]: sys.path
   Out[121]:
   ['',
    '/Users/cburns/local/bin',
    '/Users/cburns/local/lib/python2.5/site-packages/grin-1.1-py2.5.egg',
    '/Users/cburns/local/lib/python2.5/site-packages/argparse-0.8.0-py2.5.egg',
    '/Users/cburns/local/lib/python2.5/site-packages/urwid-0.9.7.1-py2.5.egg',
    '/Users/cburns/local/lib/python2.5/site-packages/yolk-0.4.1-py2.5.egg',
    '/Users/cburns/local/lib/python2.5/site-packages/virtualenv-1.2-py2.5.egg',
    ...

``pickle``: fácil persistencia
------------------------------

Útil para almacenar objetos arbitrarios a un archivo. No es seguro o rápido!

.. sourcecode:: ipython

   In [1]: import pickle

   In [2]: l = [1, None, 'Stan']

   In [3]: pickle.dump(l, file('test.pkl', 'w'))

   In [4]: pickle.load(file('test.pkl'))
   Out[4]: [1, None, 'Stan']

.. topic:: Ejercicio

   Escriba un programa para buscar su ``PYTHONPATH`` para el módulo ``site.py`` .

:ref:`path_site`
