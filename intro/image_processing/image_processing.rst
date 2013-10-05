El submódulo dedicada al procesamiento de imágenes en scipy es :mod:`scipy.ndimage`.

.. code-block:: python

   >>> from scipy import ndimage

Rutinas de procesamiento de imágenes pueden ser clasificadas de acuerdo a la categoría de procesamiento que realizan.

Transformaciones geométricas en imágenes
........................................

Cambio de orientación, resolución, ..

.. code-block:: python

   >>> from scipy import misc
   >>> lena = misc.lena()
   >>> shifted_lena = ndimage.shift(lena, (50, 50))
   >>> shifted_lena2 = ndimage.shift(lena, (50, 50), mode='nearest')
   >>> rotated_lena = ndimage.rotate(lena, 30)
   >>> cropped_lena = lena[50:-50, 50:-50]
   >>> zoomed_lena = ndimage.zoom(lena, 2)
   >>> zoomed_lena.shape
   (1024, 1024)

.. figure:: image_processing/lena_transforms.png
   :align: center
   :scale: 70

.. sourcecode:: ipython

   In [35]: subplot(151)
   Out[35]: <matplotlib.axes.AxesSubplot object at 0x925f46c>

   In [36]: pl.imshow(shifted_lena, cmap=cm.gray)
   Out[36]: <matplotlib.image.AxesImage object at 0x9593f6c>

   In [37]: axis('off')
   Out[37]: (-0.5, 511.5, 511.5, -0.5)

   In [39]: # etc.

Filtrado de imágenes
....................

.. code-block:: python

   >>> from scipy import misc
   >>> lena = misc.lena()
   >>> import numpy as np
   >>> noisy_lena = np.copy(lena).astype(np.float)
   >>> noisy_lena += lena.std()*0.5*np.random.standard_normal(lena.shape)
   >>> blurred_lena = ndimage.gaussian_filter(noisy_lena, sigma=3)
   >>> median_lena = ndimage.median_filter(blurred_lena, size=5)
   >>> from scipy import signal
   >>> wiener_lena = signal.wiener(blurred_lena, (5,5))

.. figure:: image_processing/filtered_lena.png
   :align: center
   :scale: 80

Muchos otros filtros en :mod:`scipy.ndimage.filters` y :mod:`scipy.signal` se puede aplicar a las imágenes.

.. topic:: Ejercicio
   :class: green

   Comparar histogramas para las diferentes imágenes filtradas.

Morfología matemática
.....................

Morfología matemática es una teoría derivada de teória de conjuntos. Se caracterizan y transforman las estructuras geométricas. En particular las imágenes binarias (blanco y negro), se pueden transformar mediante esta teoría: los conjuntos para ser transformados son los grupos de píxeles vecinos de valor no cero. La teoría también se extendió a las imágenes en escala de grises.

.. image:: image_processing/morpho_mat.png
   :align: center

Operaciones elementales matemático-morfologícas usan un *elemento estructurante*
con el fin de modificar otras estructuras geométricas.

Primero vamos a generar un elemento estructurante

.. code-block:: python

   >>> el = ndimage.generate_binary_structure(2, 1)
   >>> el
   array([[False, True, False],
          [True, True, True],
          [False, True, False]], dtype=bool)
   >>> el.astype(np.int)
   array([[0, 1, 0],
          [1, 1, 1],
          [0, 1, 0]])

* **Erosion**

  .. code-block:: python

     >>> a = np.zeros((7,7), dtype=np.int)
     >>> a[1:6, 2:5] = 1
     >>> a
     array([[0, 0, 0, 0, 0, 0, 0],
            [0, 0, 1, 1, 1, 0, 0],
            [0, 0, 1, 1, 1, 0, 0],
            [0, 0, 1, 1, 1, 0, 0],
            [0, 0, 1, 1, 1, 0, 0],
            [0, 0, 1, 1, 1, 0, 0],
            [0, 0, 0, 0, 0, 0, 0]])
     >>> ndimage.binary_erosion(a).astype(a.dtype)
     array([[0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 1, 0, 0, 0],
            [0, 0, 0, 1, 0, 0, 0],
            [0, 0, 0, 1, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0]])
     >>> #Erosión elimina los objetos más pequeños que la estructura
     >>> ndimage.binary_erosion(a, structure=np.ones((5,5))).astype(a.dtype)
     array([[0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0]])

* **Dilatación**
 
  .. code-block:: python

     >>> a = np.zeros((5, 5))
     >>> a[2, 2] = 1
     >>> a
     array([[ 0.,  0.,  0.,  0.,  0.],
            [ 0.,  0.,  0.,  0.,  0.],
            [ 0.,  0.,  1.,  0.,  0.],
            [ 0.,  0.,  0.,  0.,  0.],
            [ 0.,  0.,  0.,  0.,  0.]])
     >>> ndimage.binary_dilation(a).astype(a.dtype)
     array([[ 0.,  0.,  0.,  0.,  0.],
            [ 0.,  0.,  1.,  0.,  0.],
            [ 0.,  1.,  1.,  1.,  0.],
            [ 0.,  0.,  1.,  0.,  0.],
            [ 0.,  0.,  0.,  0.,  0.]])

* **Apertura**

  .. code-block:: python

     >>> a = np.zeros((5,5), dtype=np.int)
     >>> a[1:4, 1:4] = 1; a[4, 4] = 1
     >>> a
     array([[0, 0, 0, 0, 0],
            [0, 1, 1, 1, 0],
            [0, 1, 1, 1, 0],
            [0, 1, 1, 1, 0],
            [0, 0, 0, 0, 1]])
     >>> #Apertura elimina objetos pequeños
     >>> ndimage.binary_opening(a, structure=np.ones((3,3))).astype(np.int)
     array([[0, 0, 0, 0, 0],
            [0, 1, 1, 1, 0],
            [0, 1, 1, 1, 0],
            [0, 1, 1, 1, 0],
            [0, 0, 0, 0, 0]])
     >>> # Opening can also smooth corners
     >>> ndimage.binary_opening(a).astype(np.int)
     array([[0, 0, 0, 0, 0],
            [0, 0, 1, 0, 0],
            [0, 1, 1, 1, 0],
            [0, 0, 1, 0, 0],
            [0, 0, 0, 0, 0]])

* **Cierre:** ``ndimage.binary_closing``

.. topic:: Ejercicio
   :class: green

   Compruebe que las cantidades iniciales a erosionar, se dilataran.

Una operación de apertura elimina estructuras pequeñas, mientras que una operación de cierre rellena pequeños agujeros. Por tanto, estas operaciones pueden ser utilizados para "limpiar" una imagen.

.. code-block:: python

   >>> a = np.zeros((50, 50))
   >>> a[10:-10, 10:-10] = 1
   >>> a += 0.25*np.random.standard_normal(a.shape)
   >>> mask = a>=0.5
   >>> opened_mask = ndimage.binary_opening(mask)
   >>> closed_mask = ndimage.binary_closing(opened_mask)

.. figure:: image_processing/morpho.png
   :align: center
   :scale: 75

.. topic:: Ejercicio
   :class: green

   Compruebe que el área del cuadrado reconstruido es menor que el área del cuadrado inicial. (Lo contrario ocurriría si la etapa de cierre se realiza *antes* que la apertura).

Para imágenes *en escala de grises*, erosionar (respuesta a dilatar) equivale a la sustitución de un píxel por el valor mínimo (respuesta maximal) entre los píxeles incluidos en el elemento estructurante centrado en el píxel de interés.

.. code-block:: python

   >>> a = np.zeros((7,7), dtype=np.int)
   >>> a[1:6, 1:6] = 3
   >>> a[4,4] = 2; a[2,3] = 1
   >>> a
   array([[0, 0, 0, 0, 0, 0, 0],
          [0, 3, 3, 3, 3, 3, 0],
          [0, 3, 3, 1, 3, 3, 0],
          [0, 3, 3, 3, 3, 3, 0],
          [0, 3, 3, 3, 2, 3, 0],
          [0, 3, 3, 3, 3, 3, 0],
          [0, 0, 0, 0, 0, 0, 0]])
   >>> ndimage.grey_erosion(a, size=(3,3))
   array([[0, 0, 0, 0, 0, 0, 0],
          [0, 0, 0, 0, 0, 0, 0],
          [0, 0, 1, 1, 1, 0, 0],
          [0, 0, 1, 1, 1, 0, 0],
          [0, 0, 3, 2, 2, 0, 0],
          [0, 0, 0, 0, 0, 0, 0],
          [0, 0, 0, 0, 0, 0, 0]])

Mediciones en imágenes
......................

Primero vamos a generar una imagen binaria sintética agradable.

.. code-block:: python

   >>> x, y = np.indices((100, 100))
   >>> sig = np.sin(2*np.pi*x/50.)*np.sin(2*np.pi*y/50.)*(1+x*y/50.**2)**2
   >>> mask = sig > 1

Ahora veremos variada información acerca de los objetos en la imagen

.. code-block:: python

   >>> labels, nb = ndimage.label(mask)
   >>> nb
   8
   >>> areas = ndimage.sum(mask, labels, xrange(1, labels.max()+1))
   >>> areas
   array([ 190.,   45.,  424.,  278.,  459.,  190.,  549.,  424.])
   >>> maxima = ndimage.maximum(sig, labels, xrange(1, labels.max()+1))
   >>> maxima
   array([  1.80238238,   1.13527605,   5.51954079,   2.49611818,
            6.71673619,   1.80238238,  16.76547217,   5.51954079])
   >>> ndimage.find_objects(labels==4)
   [(slice(30L, 48L, None), slice(30L, 48L, None))]
   >>> sl = ndimage.find_objects(labels==4)
   >>> import pylab as pl
   >>> pl.imshow(sig[sl[0]])   # doctest: +ELLIPSIS
   <matplotlib.image.AxesImage object at ...>

.. figure:: image_processing/measures.png
   :align: center
   :scale: 80

Ver el sumario de ejercicios en :ref:`summary_exercise_image_processing` para una ejemplo más avanzado.
