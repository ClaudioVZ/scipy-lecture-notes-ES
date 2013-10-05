.. For doctests
   >>> import numpy as np
   >>> from matplotlib import pyplot as plt


.. currentmodule:: numpy

Operaciones avanzadas
=====================

.. contents:: Contenido
    :local:
    :depth: 1

Polinomios
-----------

Numpy contiene polinomios en diferentes bases :

Por ejemplo, :math:`3x^2 + 2x - 1`

.. code-block:: python

   >>> p = np.poly1d([3, 2, -1])
   >>> p(0)
   -1
   >>> p.roots
   array([-1.        ,  0.33333333])
   >>> p.order
   2

.. code-block:: python

   >>> x = np.linspace(0, 1, 20)
   >>> y = np.cos(x) + 0.3*np.random.rand(20)
   >>> p = np.poly1d(np.polyfit(x, y, 3))

   >>> t = np.linspace(0, 1, 200)
   >>> plt.plot(x, y, 'o', t, p(t), '-')   # doctest: +ELLIPSIS
   [<matplotlib.lines.Line2D object at ...>, <matplotlib.lines.Line2D object at ...>]

.. plot:: pyplots/numpy_intro_9.py

Ver http://docs.scipy.org/doc/numpy/reference/routines.polynomials.poly1d.html
para más información.

Más polinomios (con más bases)
..............................

Numpy tiene una interfaz para polinomios más sofisticada , que soporta
por ejemplo bases de Chebyshev.

:math:`3x^2 + 2x - 1`

.. code-block:: python

   >>> p = np.polynomial.Polynomial([-1, 2, 3]) # coeficientes en orden diferente!
   >>> p(0)
   -1.0
   >>> p.roots()
   array([-1.        ,  0.33333333])
   >>> p.degree()  # generalmente en polinomios no siempre muestra el 'orden'
   2

Ejemplo de uso de polinomios en bases de Chebyshev, para polinomios en el rango ``[-1, 1]``

.. code-block:: python

   >>> x = np.linspace(-1, 1, 2000)
   >>> y = np.cos(x) + 0.3*np.random.rand(2000)
   >>> p = np.polynomial.Chebyshev.fit(x, y, 90)

   >>> t = np.linspace(-1, 1, 200)
   >>> plt.plot(x, y, 'r.')   # doctest: +ELLIPSIS
   [<matplotlib.lines.Line2D object at ...>]
   >>> plt.plot(t, p(t), 'k-', lw=3)   # doctest: +ELLIPSIS
   [<matplotlib.lines.Line2D object at ...>]

.. plot:: pyplots/numpy_intro_10.py

Los polinomios de Chebyshev tienen algunas ventajas en interpolación.

Cargando archivos de datos
--------------------------

Archivos de texto
.................

Ejemplo: :download:`populations.txt <../../data/populations.txt>`:

.. include:: ../../data/populations.txt
   :end-line: 5
   :literal:

.. code-block:: python

   >>> data = np.loadtxt('data/populations.txt')
   >>> data    # doctest: +ELLIPSIS
   array([[  1900.,  30000.,   4000.,  48300.],
          [  1901.,  47200.,   6100.,  48200.],
          [  1902.,  70200.,   9800.,  41500.],
   ...

.. code-block:: python

   >>> np.savetxt('pop2.txt', data)
   >>> data2 = np.loadtxt('pop2.txt')

.. note:: Si usted tiene un archivo de texto complicado, puede probar con:

   - ``np.genfromtxt``

   - Usar las funciones de E/S de Python, por ejemplo regexps para parsear (Python es bastante adecuado para esto)

.. topic:: Recuerde: Navegando por el sistema de archivos con IPython

   .. sourcecode:: ipython

      In [1]: pwd      # muestra el directorio actual
      '/home/user/stuff/2011-numpy-tutorial'
      In [2]: cd ex
      '/home/user/stuff/2011-numpy-tutorial/ex'
      In [3]: ls
      populations.txt	species.txt

Imágenes
........

Usando Matplotlib

.. code-block:: python

   >>> img = plt.imread('data/elephant.png')
   >>> img.shape, img.dtype
   ((200, 300, 3), dtype('float32'))
   >>> plt.imshow(img)     # doctest: +ELLIPSIS
   <matplotlib.image.AxesImage object at ...>
   >>> plt.savefig('plot.png')

   >>> plt.imsave('red_elephant', img[:,:,0], cmap=plt.cm.gray)

Esto guarda solo un canal (de RGB)

.. code-block:: python

   >>> plt.imshow(plt.imread('red_elephant.png'))  # doctest: +ELLIPSIS
   <matplotlib.image.AxesImage object at ...>

Otras bibliotecas

.. code-block:: python

   >>> from scipy.misc import imsave
   >>> imsave('tiny_elephant.png', img[::6,::6])
   >>> plt.imshow(plt.imread('tiny_elephant.png'), interpolation='nearest')  # doctest: +ELLIPSIS
   <matplotlib.image.AxesImage object at ...>

.. plot:: pyplots/numpy_intro_3.py

Formato propio de Numpy
.......................

Numpy tiene su propio formato binario, no es portátil pero con eficiente E/S

.. code-block:: python

   >>> data = np.ones((3, 3))
   >>> np.save('pop.npy', data)
   >>> data3 = np.load('pop.npy')

Formatos bien conocidos (y más obscuros) de archivo
...................................................

* HDF5: `h5py <http://code.google.com/p/h5py/>`__, `PyTables <http://pytables.org>`__
* NetCDF: ``scipy.io.netcdf_file``, `netcdf4-python <http://code.google.com/p/netcdf4-python/>`__, ...
* Matlab: ``scipy.io.loadmat``, ``scipy.io.savemat``
* MatrixMarket: ``scipy.io.mmread``, ``scipy.io.mmread``

... si alguien lo usa, es probable que haya una biblioteca de Python para ello.

.. topic:: Ejercicio: Archivos de datos de texto
   :class: green

   Escribir un script en Python para cargar los datos de :download:`populations.txt <../../data/populations.txt>` y elimine la última columna y las primeras 5 filas. Guarde el pequeño conjunto de datos en ``pop2.txt``.

.. loadtxt, savez, load, fromfile, tofile

.. real life: point to HDF5, NetCDF, etc.

.. EXE: use loadtxt to load a data file
.. EXE: use savez and load to save data in binary format
.. EXE: use tofile and fromfile to put and get binary data bytes in/from a file
   follow-up: .view()
.. EXE: parsing text files -- Python can do this reasonably well natively!
   throw in the mix some random text file to be parsed (eg. PPM)
.. EXE: advanced: read the data in a PPM file


.. topic:: El interior de Numpy

   Si está interesado en el funcionamiento interno de Numpy, hay una buena discusión en :ref:`advanced_numpy`.

