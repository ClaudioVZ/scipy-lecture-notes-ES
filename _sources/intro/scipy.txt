Scipy: computación científica de alto nivel
===========================================

:Autores: Adrien Chauve, Andre Espaze, Emmanuelle Gouillart,
          Gaël Varoquaux, Ralf Gommers

..
    >>> import numpy as np
    >>> np.random.seed(0)

.. topic:: Scipy

   El paquete ``scipy`` contiene varias cajas de herramientas dedicadas a problemas comunes problemas en computación científica. Sus diferentes submódulos corresponden a diferentes aplicaciones, tales como interpolación, integración, optimización, procesamiento de imágenes, estadística, funciones especiales, etc.

   ``scipy`` se puede comparar con otras bibliotecas estándar de computación científica, como GSL (GNU Scientific Library para C y C++), o cajas de herramientas de Matlab. ``scipy`` es el paquete núcleo para rutinas científicas en Python; operando de manera eficiente los arreglos ``numpy``, por tanto numpy y scipy trabajan mano a mano.

   Antes de implementar una rutina, hay que comprobar si no está implementado en Scipy. Como programadores no profesionales, los científicos a menudo tienden a **reinventar la rueda**, lo que conduce a errores en el código, código no optimizado, código difícil de compartir e imposible de mantener. Por el contrario, las rutinas que contiene ``Scipy`` están optimizadas y probadas, por lo tanto se debe utilizar siempre que sea posible.

.. contents:: Contenido
    :local:
    :depth: 1

.. warning:: Este tutorial está lejos de ser una introducción a métodos numéricos. Enumerar los diferentes submódulos y funciones en scipy seria muy aburrido, en cambio nos concentramos en algunos ejemplos para dar una idea general de cómo utilizar ``scipy``  para computación científica.

:mod:`scipy` contiene sub-módulos para tareas específicas:

=========================== ===============================================
:mod:`scipy.cluster`         Vector quantization / Kmeans
:mod:`scipy.constants`       Constantes físicas y matemáticas
:mod:`scipy.fftpack`         Transformadas de Fourier
:mod:`scipy.integrate`       Rutinas para integración
:mod:`scipy.interpolate`     Interpolación
:mod:`scipy.io`              Entrada y salida
:mod:`scipy.linalg`          Rutinas para algebra lineal
:mod:`scipy.ndimage`         n-dimensional image package
:mod:`scipy.odr`             Orthogonal distance regression
:mod:`scipy.optimize`        Optimización
:mod:`scipy.signal`          Procesamiento de señales
:mod:`scipy.sparse`          Matrices dispersas
:mod:`scipy.spatial`         estructuras de datos espaciales y algoritmos
:mod:`scipy.special`         Funciones matemáticas especiales
:mod:`scipy.stats`           Estadística
=========================== ===============================================

Todos ellos dependen de :mod:`numpy`, en su mayoría son independientes el uno del otro. La forma estándar de importar Numpy y módulos Scipy es

.. code-block:: python

   >>> import numpy as np
   >>> from scipy import stats  # lo mismo para otros submódulos

El espacio de nombres principal ``scipy`` que contiene la mayoría de las funciones son en realidad funciones numpy (pruebe ``scipy.cos is np.cos``). Están expuestos por razones solamente históricas, por lo general no hay ninguna razón para utilizar ``import scipy`` en su código.

Entrada/salida de archivos: :mod:`scipy.io`
-------------------------------------------

* Cargar y guardar archivos de matlab

  .. code-block:: python

     >>> from scipy import io as spio
     >>> a = np.ones((3, 3))
     >>> spio.savemat('file.mat', {'a': a}) # savemat espera un diccionario
     >>> data = spio.loadmat('file.mat', struct_as_record=True)
     >>> data['a']
     array([[ 1.,  1.,  1.],
            [ 1.,  1.,  1.],
            [ 1.,  1.,  1.]])

* Lectura de imágenes

  .. code-block:: python

     >>> from scipy import misc
     >>> misc.imread('fname.png')
     >>> # Matplotlib tiene una función similar
     >>> import matplotlib.pyplot as plt
     >>> plt.imread('fname.png')

Ver también:

* Cargar archivos de texto: :func:`numpy.loadtxt`/:func:`numpy.savetxt`

* Cargar archivos de texto/csv de forma hábil: :func:`numpy.genfromtxt`/:func:`numpy.recfromcsv`

* Rápido y eficiente, formato propio de numpy en archivo binario: :func:`numpy.save`/:func:`numpy.load`

Funciones especiales: :mod:`scipy.special`
------------------------------------------

Las funciones especiales son funciones trascendentales. La cadena de documentación del módulo :mod:`scipy.special` está bien escrita, así que no vamos a enumerar todas las
funciones aquí. Los frecuentemente utilizadas son:

* Función de Bessel, tales como :func:`scipy.special.jn` (función de Bessel de enésimo orden)

* Función elíptica (:func:`scipy.special.ellipj` para la función elíptica jacobiana, ...)

* Función Gamma: :func:`scipy.special.gamma`, también tenga en cuenta :func:`scipy.special.gammaln` dará el registro de Gamma con una mayor precisión numérica.

* Erf, la función error o el área bajo la curva de Gauss: :func:`scipy.special.erf`

.. _scipy_linalg:

Operaciones de álgebra lineal: :mod:`scipy.linalg`
--------------------------------------------------

El módulo :mod:`scipy.linalg` ofrece operaciones estándar para álgebra lineal, basándose en una aplicación eficiente subyacente (BLAS, LAPACK).

* El función :func:`scipy.linalg.det` calcula el determinante de una matriz cuadrada

  .. code-block:: python

     >>> from scipy import linalg
     >>> arr = np.array([[1, 2],
     ...                 [3, 4]])
     >>> linalg.det(arr)
     -2.0
     >>> arr = np.array([[3, 2],
     ...                 [6, 4]])
     >>> linalg.det(arr)
     0.0
     >>> linalg.det(np.ones((3, 4)))
     Traceback (most recent call last):
     ...
     ValueError: expected square matrix

* La función :func:`scipy.linalg.inv` calcula la inversa de una matriz cuadrada

  .. code-block:: python

     >>> arr = np.array([[1, 2],
     ...                 [3, 4]])
     >>> iarr = linalg.inv(arr)
     >>> iarr
     array([[-2. ,  1. ],
            [ 1.5, -0.5]])
     >>> np.allclose(np.dot(arr, iarr), np.eye(2))
     True

  Finalmente calcular la inversa de una matriz singular (su determinante es cero) generará un error ``LinAlgError``

  .. code-block:: python

     >>> arr = np.array([[3, 2],
     ...                 [6, 4]])
     >>> linalg.inv(arr)
     Traceback (most recent call last):
     ...
     LinAlgError: singular matrix

* Operaciones más avanzadas están disponibles, por ejemplo descomposición en valores singulares (SVD)

  .. code-block:: python

     >>> arr = np.arange(9).reshape((3, 3)) + np.diag([1, 0, 1])
     >>> uarr, spec, vharr = linalg.svd(arr)

  Los valores propios son

  .. code-block:: python

     >>> spec    # doctest: +ELLIPSIS
     array([ 14.88982544,   0.45294236,   0.29654967])

  La matriz original puede ser obtenida mediante multiplicación matricial de los resultados de ``svd`` con ``np.dot``

  .. code-block:: python

     >>> sarr = np.diag(spec)
     >>> svd_mat = uarr.dot(sarr).dot(vharr)
     >>> np.allclose(svd_mat, arr)
     True

  SVD se utiliza comúnmente en estadística y procesamiento de señales. Otros métodos de descomposiciones estándar están disponibles (QR, LU, Cholesky, Schur), támbien solucionadores de sistemas lineales, están disponibles en :mod:`scipy.linalg`.

Transformada rápida de Fourier: :mod:`scipy.fftpack`
----------------------------------------------------

El módulo :mod:`scipy.fftpack` permite calcular la transformada rápida de Fourier. A modo de ejemplo, una señal de entrada (con ruido) puede verse

.. code-block:: python

   >>> time_step = 0.02
   >>> period = 5.
   >>> time_vec = np.arange(0, 20, time_step)
   >>> sig = np.sin(2 * np.pi / period * time_vec) + \
   ...       0.5 * np.random.randn(time_vec.size)

El observador no conoce la frecuencia de la señal, sólo el paso de tiempo de muestreo de la señal ``sig``. La señal se supone que procede de una función real para la cual la transformada de Fourier será simétrica.
La función :func:`scipy.fftpack.fftfreq` genera las frecuencias de muestreo y :func:`scipy.fftpack.fft` calcula la transformada rápida de Fourier

.. code-block:: python

   >>> from scipy import fftpack
   >>> sample_freq = fftpack.fftfreq(sig.size, d=time_step)
   >>> sig_fft = fftpack.fft(sig)

Debido a que la potencia resultante es simétrica, sólo la parte positiva del espectro es utilizada para encontrar la frecuencia

.. code-block:: python

   >>> pidxs = np.where(sample_freq > 0)
   >>> freqs = sample_freq[pidxs]
   >>> power = np.abs(sig_fft)[pidxs]

.. plot:: pyplots/fftpack_frequency.py
   :scale: 70

La frecuencia de la señal se puede encontrar

.. code-block:: python

   >>> freq = freqs[power.argmax()]
   >>> np.allclose(freq, 1./period)  # check that correct freq is found
   True

Ahora, el ruido de alta frecuencia será eliminado de la señal transformada de Fourier

.. code-block:: python

   >>> sig_fft[np.abs(sample_freq) > freq] = 0

La señal filtrada resultante puede ser calculado con la función :func:`scipy.fftpack.ifft`

.. code-block:: python

   >>> main_sig = fftpack.ifft(sig_fft)

El resultado se puede ver con

.. code-block:: python

   >>> import pylab as plt
   >>> plt.figure()
   >>> plt.plot(time_vec, sig)
   >>> plt.plot(time_vec, main_sig, linewidth=3)
   >>> plt.xlabel('Time [s]')
   >>> plt.ylabel('Amplitude')

.. plot:: pyplots/fftpack_signals.py
    :scale: 70

.. topic:: `numpy.fft`

   Numpy también tiene una implementación de FFT (:mod:`numpy.fft`). Sin embargo, en general es preferible usar :mod:`scipy.fftpack`, ya que utiliza implementaciones subyacentes más eficientes.

.. topic:: Ejemplo práctico: Encontrar periodicidad cruda

   .. plot:: intro/solutions/periodicity_finder.py

.. topic:: Ejemplo práctico: Gaussian image blur

   Convolución:

   .. math::

      f_1(t) = \int dt'\, K(t-t') f_0(t')

   .. math::

      \tilde{f}_1(\omega) = \tilde{K}(\omega) \tilde{f}_0(\omega)

   .. plot:: intro/solutions/image_blur.py

.. topic:: Ejercicio: Imagen de alunizaje con ruido
   :class: green

   .. image:: ../data/moonlanding.png
     :scale: 70

   1. Examine la imagen moonlanding.png proporcionada, que está en gran medida
      contaminada con ruido periódico. En este ejercicio, nuestro objetivo es limpiar
      el ruido utilizando la transformada rápida de Fourier.

   2. Cargar la imagen usando :func:`pylab.imread`.

   3. Encontrar y utilizar la función FFT 2-D en :mod:`scipy.fftpack`, y graficar el
      espectro de la imagen (de la transformada de Fourier). Tiene algún problema
      en visualizar el espectro? Si es así, porqué?

   4. El espectro consta de componentes de alta y baja frecuencia. El ruido está contenida en la parte de alta frecuencia del espectro, por lo que se debe establecer algunos de sus componentes a cero (use segmentado de arreglos o slicing).

   5. Aplicar la transformada inversa de Fourier para ver la imagen resultante.

Optimización y ajuste: :mod:`scipy.optimize`
-------------------------------------------

Optimización es el problema de encontrar una solución numérica a un
minimización o igualdad.

El módulo :mod:`scipy.optimize` proporciona algoritmos útiles para la minimización de funciones (escalares o multidimensionales), ajuste de curvas y búsqueda de raices.

.. code-block:: python

   >>> from scipy import optimize

**Encontrar los mínimos de una función escalar**

Vamos a definir la siguiente función:

.. code-block:: python

   >>> def f(x):
   ...     return x**2 + 10*np.sin(x)

y la graficamos:

.. code-block:: python

   >>> x = np.arange(-10, 10, 0.1)
   >>> plt.plot(x, f(x)) # doctest:+SKIP
   >>> plt.show() # doctest:+SKIP

.. plot:: pyplots/scipy_optimize_example1.py

Esta función tiene un mínimo global en torno a -1.3 y un mínimo local en torno a 3.8.

La forma general y eficiente para encontrar el mínimo de esta función es llevar a cabo un descenso de gradiente partiendo de un punto inicial dado. El algoritmo BFGS es una buena manera de hacer esto

.. code-block:: python

   >>> optimize.fmin_bfgs(f, 0)
   Optimization terminated successfully.
            Current function value: -7.945823
	    Iterations: 5
	    Function evaluations: 24
	    Gradient evaluations: 8
   array([-1.30644003])

Un posible problema con este enfoque es, si la función tiene mínimos locales
el algoritmo puede encontrar estos mínimos locales en lugar de los mínimos globales de pendiendo del punto inicial:

.. code-block:: python

   >>> optimize.fmin_bfgs(f, 3, disp=0)
   array([ 3.83746663])

Si no conocemos el intérvalo del mínimo global para elegir el punto inicial, tenemos que recurrir a una más costoso optimización global. Para encontrar el mínimo global, el algoritmo más simple es el algoritmo de fuerza bruta, en la que el
la función se evalúa en cada punto de una cuadrícula:

.. code-block:: python

   >>> grid = (-10, 10, 0.1)
   >>> xmin_global = optimize.brute(f, (grid,))
   >>> xmin_global
   array([-1.30641113])

Para tamaños de cuadrícula más grandes, :func:`scipy.optimize.brute` se vuelve muy lento. :func:`scipy.optimize.anneal` ofrece una alternativa, utilizando recocido simulado. Existen algoritmos más eficientes para diferentes tipos de problemas de optimización global, pero están fuera del alcance de ``scipy``. Algunos paquetes útiles para optimización global son OpenOpt_, IPOPT_, PyGMO_ y PyEvolve_.

.. _OpenOpt: http://openopt.org/Welcome
.. _IPOPT: https://github.com/xuy/pyipopt
.. _PyGMO: http://pagmo.sourceforge.net/pygmo/index.html
.. _PyEvolve: http://pyevolve.sourceforge.net/

Para encontrar el mínimo local, vamos a restringir la variable en el intervalo
``(0, 10)`` usando :func:`scipy.optimize.fminbound`:

.. code-block:: python

   >>> xmin_local = optimize.fminbound(f, 0, 10)    # doctest: +ELLIPSIS
   >>> xmin_local
   3.8374671...

.. note:: Encontrar los mínimos de una función se discute con más detalles en el capítulo avanzado: :ref:`mathematical_optimization`.

**Encontrar las raíces de una función escalar**

Para encontrar una raíz, o el punto donde ``f(x) = 0``, de la función ``f`` de arriba
podemos utilizar por ejemplo :func:`scipy.optimize.fsolve`:

.. code-block:: python

   >>> root = optimize.fsolve(f, 1)  # our initial guess is 1
   >>> root
   array([ 0.])

Tenga en cuenta que sólo una raíz puede ser encontrada. Una inspeción a la gráfica de ``f`` revela que hay una segunda raíz alrededor de -2.5. Encontramos el valor exacto de la misma mediante un ajuste a la estimación inicial:

.. code-block:: python

   >>> root2 = optimize.fsolve(f, -2.5)
   >>> root2
   array([-2.47948183])

**Ajuste de curvas**

Supongamos que tenemos datos de una muestra de ``f`` con un poco de ruido:

.. code-block:: python

   >>> xdata = np.linspace(-10, 10, num=20)
   >>> ydata = f(xdata) + np.random.randn(xdata.size)

Ahora bien, si se conoce la forma funcional de la función de la cual las muestras fueron dibujadas (``x^2 + sin(x)`` en este caso), pero no la amplitud de los términos, puede ser encontrados mediante una ajuste por mínimos cuadrados. Primero tenemos que definir la funcionar a ajustar

.. code-block:: python

   >>> def f2(x, a, b):
   ...     return a*x**2 + b*np.sin(x)

Entonces podemos usar :func:`scipy.optimize.curve_fit` para encontrar ``a`` y ``b``:

.. code-block:: python

   >>> guess = [2, 2]
   >>> params, params_covariance = optimize.curve_fit(f2, xdata, ydata, guess)
   >>> params
   array([ 0.99925147,  9.76065551])

Ahora hemos encontrado los mínimos y las raíces de ``f`` y usando un ajuste de la curva, ponemos todos los resultados juntos en un gráfico:

.. plot:: pyplots/scipy_optimize_example2.py

.. note:: En scipy >= 0.11 se unificaron las interfaces para todos los algoritmos de minimización y búsqueda de raices están disponibles en :func:`scipy.optimize.minimize`, :func:`scipy.optimize.minimize_scalar` y :func:`scipy.optimize.root`. Se permite comparar fácilmente diferentes algoritmos a través de la palabra clave ``method``.
   Usted puede encontrar algoritmos con las mismas funcionalidades para problemas multi-dimensional en :mod:`scipy.optimize`.

.. topic:: Ejercicio: Ajuste de datos de temperatura
   :class: green

   Las temperaturas extremas en Alaska para cada mes, a partir de enero (en grados centígrados)

   .. code-block:: python

      max:  17,  19,  21,  28,  33,  38, 37,  37,  31,  23,  19,  18
      min: -62, -59, -56, -46, -32, -18, -9, -13, -25, -46, -52, -58

   1. Grafique las temperaturas extremas.

   2. Definir una función que puede describir las temperaturas min y máx.
      Sugerencia: esta función tiene un periodo de 1 año.
      Sugerencia: incluir un tiempo compensado.
   3. Ajuste esta función a los datos con :func:`scipy.optimize.curve_fit`.
   4. Graficar el resultado. El ajuste es razonable? Si no, por qué?
   5. Si el tiempo no es compensado el ajuste de las temperaturas mínima y máxima tiene la misma exactitud?

.. topic:: Ejercicio: Minimización 2-D
   :class: green

   .. plot:: pyplots/scipy_optimize_sixhump.py

   La función six-hump camelback

   .. math:: f(x, y) = (4 - 2.1x^2 + \frac{x^4}{3})x^2 + xy + (4y^2 - 4)y^2

   tiene múltiples mınimos globales y locales. Encuentre los mínimos globales de esta función.

   Sugerencia:

   - Puede restringir las variables a ``-2 < x < 2`` y ``-1 < y < 1``.
   - Use :func:`numpy.meshgrid` y :func:`pylab.imshow` para encontrar visualmente las regiones.
   - Use :func:`scipy.optimize.fmin_bfgs` o otro minimizador multidimensional.

   Cuántos mínimos globales hay, y cuál es el valor de la función en esos puntos? Qué sucede con una estimación inicial de ``(x, y) = (0, 0)``?

   Ver el sumario de ejercicios en :ref:`summary_exercise_optimize` para ejemplos más avanzados.

Estadística y números aleatorios: :mod:`scipy.stats`
----------------------------------------------------

El módulo :mod:`scipy.stats` contiene herramientas estadísticas y probabilísticas
descripciones de procesos aleatorios. Generadores de números aleatorios para diferentes
procesos aleatorios se puede encontrar en :mod:`numpy.random` .

Histograma y la función de densidad de probabilidad
...................................................

Mediante observaciones de un proceso aleatorio, su histograma es un estimador del proceso aleatorio PDF (probability density function):

.. code-block:: python

   >>> a = np.random.normal(size=1000)
   >>> bins = np.arange(-4, 5)
   >>> bins
   array([-4, -3, -2, -1,  0,  1,  2,  3,  4])
   >>> histogram = np.histogram(a, bins=bins, normed=True)[0]
   >>> bins = 0.5*(bins[1:] + bins[:-1])
   >>> bins
   array([-3.5, -2.5, -1.5, -0.5,  0.5,  1.5,  2.5,  3.5])
   >>> from scipy import stats
   >>> b = stats.norm.pdf(bins)  # norm is a distribution

.. sourcecode:: ipython

   In [1]: pl.plot(bins, histogram)
   In [2]: pl.plot(bins, b)

.. plot:: pyplots/normal_distribution.py
   :scale: 70

Si sabemos que el proceso aleatorio pertenece a una determinada familia de procesos aleatorios, tales como procesos normales, podemos hacer un ajuste de máxima credibilidad de las observaciones para estimar los parámetros de la distribución subyacente. Aquí ajustamos a un proceso normal los datos observados

.. code-block:: python

   >>> loc, std = stats.norm.fit(a)
   >>> loc     # doctest: +ELLIPSIS
   -0.045256707490...
   >>> std     # doctest: +ELLIPSIS
   0.9870331586690...

.. topic:: Ejercicio: Distribuciones de probabilidad
   :class: green

   Generar 1000 variables aleatorias desde una distribución gamma con un parámetro de forma igual a 1, despues graficar el histograma de las muestras. Puede graficar la pdf de arriba (debe coincidir)?

   Adicional: las distribuciones tienen una serie de métodos útiles. Explore la documentación o mediante la implementación del tabulador IPython. Puede encontrar el parámetro de forma de 1 vuelta usando el método ``fit`` para variables aleatorias?

Percentiles
...........

La mediana es el valor situado al medio de las observaciones por debajo y arriba

.. code-block:: python

   >>> np.median(a)     # doctest: +ELLIPSIS
   -0.058028034...

También se conoce como el percentil 50, porque el 50 % de las observaciones están por debajo

.. code-block:: python

   >>> stats.scoreatpercentile(a, 50)     # doctest: +ELLIPSIS
   -0.0580280347...

Del mismo modo, podemos calcular el percentil 90

.. code-block:: python

   >>> stats.scoreatpercentile(a, 90)     # doctest: +ELLIPSIS
   1.231593551...

El percentil es un estimador de una CDF: función de distribución acumulada (cumulative distribution function).

Pruebas estadísticas
....................

Una prueba estadística es un indicador de decisión. Por ejemplo, si tenemos dos
series de observaciones, que suponemos se generan a partir de una distribución normal, podemos utilizar la `Prueba t de Student <http://es.wikipedia.org/wiki/Prueba_t_de_Student>`__ para decidir si los dos conjuntos de observaciones son significativamente diferentes

.. code-block:: python

   >>> a = np.random.normal(0, 1, size=100)
   >>> b = np.random.normal(1, 1, size=10)
   >>> stats.ttest_ind(a, b)   # doctest: +ELLIPSIS
   (-3.75832707..., 0.00027786...)

La salida resultante se compone de:

* El valor estadístico T: es un número cuyo signo es proporcional a la diferencia entre los dos procesos aleatorios y la magnitud está relacionada a cuanto diferen.

* El *valor p*: la probabilidad de que ambos procesos sean idénticos. Si es cercano a 1, los dos procesos son casi con toda seguridad idénticos. Cuanto más cercano sea a cero, lo más probable es que los procesos tengan diferentes medias.

Interpolación: :mod:`scipy.interpolate`
---------------------------------------

El módulo :mod:`scipy.interpolate` es útil para ajustar una función a partir de datos experimental y evaluar los puntos que se requieren. El módulo se basa en `FITPACK Fortran subroutines`_ del proyecto netlib_.

.. _`FITPACK Fortran subroutines` : http://www.netlib.org/dierckx/index.html
.. _netlib : http://www.netlib.org

Suponiendo que los datos experimentales son cercanos a una función seno

.. code-block:: python

   >>> measured_time = np.linspace(0, 1, 10)
   >>> noise = (np.random.random(10)*2 - 1) * 1e-1
   >>> measures = np.sin(2 * np.pi * measured_time) + noise

La clase :class:`scipy.interpolate.interp1d` puede construir una función de interpolación lineal

.. code-block:: python

   >>> from scipy.interpolate import interp1d
   >>> linear_interp = interp1d(measured_time, measures)

La instancia :obj:`scipy.interpolate.linear_interp` puede evaluarlo para un tiempo requerido

.. code-block:: python

   >>> computed_time = np.linspace(0, 1, 50)
   >>> linear_results = linear_interp(computed_time)

La interpolación cúbica puede seleccionarse al proporcionar al argumento opcional la palabra clave ``kind``

.. code-block:: python

   >>> cubic_interp = interp1d(measured_time, measures, kind='cubic')
   >>> cubic_results = cubic_interp(computed_time)

Los resultados se muestran en la siguiente figura creada con Matplotlib:

.. plot:: pyplots/scipy_interpolation.py

:class:`scipy.interpolate.interp2d` es similar a :class:`scipy.interpolate.interp1d`, pero para arreglos 2-D. Tenga en cuenta que para los valores `interp`, el tiempo calculado debe permanecer dentro del intervalo de tiempo medido. Ver el sumario de ejercicios :ref:`summary_exercise_stat_interp` para un ejemplo más avanzado de interpolación con splines.

Integración numérica: :mod:`scipy.integrate`
--------------------------------------------

La rutina de integración más genérica es :func:`scipy.integrate.quad`

.. code-block:: python

   >>> from scipy.integrate import quad
   >>> res, err = quad(np.sin, 0, np.pi/2)
   >>> np.allclose(res, 1)
   True
   >>> np.allclose(err, 1 - res)
   True

Otros esquemas de integración están disponibles como ``fixed_quad``,
``quadrature``, ``romberg``.

:mod:`scipy.integrate` también cuenta con rutinas para integrar Ecuaciones Diferenciales Ordinarias (ODE). En particular, :func:`scipy.integrate.odeint` es un integrador de propósito general utilizando LSODA (Solucionador Livermore para Ecuaciones Diferenciales ordinarias con método Automático de conmutación
para problemas rígidos y no rígidos), consulte `ODEPACK Fortran library`_ para más detalles.

.. _`ODEPACK Fortran library` : http://people.sc.fsu.edu/~jburkardt/f77_src/odepack/odepack.html

``odeint`` resuelve sistemas de ecuaciones diferenciales ordinarias de primer orden de la forma

.. code-block:: python

   dy/dt = rhs(y1, y2, .., t0,...)

A modo de introducción, vamos a resolver la ecuación diferencial ordinaria ``dy/dt = -2y`` para ``t = 0 .. 4`` , con la condición inicial ``y(t=0) = 1``. Primero se debe calcular la derivada de la función en una posición definida

.. code-block:: python

   >>> def calc_derivative(ypos, time, counter_arr):
   ...     counter_arr += 1
   ...     return -2 * ypos
   ...

El argumento adicional ``counter_arr`` se ha añadido para mostrar que la
función puede ser llamada varias veces para un solo paso de tiempo, hasta que el solucionador llege a converger. La matriz de contadores se define como

.. code-block:: python

   >>> counter = np.zeros((1,), dtype=np.uint16)

A continuación se calcula la trayectoria

.. code-block:: python

   >>> from scipy.integrate import odeint
   >>> time_vec = np.linspace(0, 4, 40)
   >>> yvec, info = odeint(calc_derivative, 1, time_vec,
   ...                     args=(counter,), full_output=True)

La función derivada ha sido llamada más de 40 veces (el número de pasos de tiempo)

.. code-block:: python

   >>> counter
   array([129], dtype=uint16)

y el número acumulado de iteraciones para cada uno de los primeros 10 pasos de tiempo
puede obtenerse con

.. code-block:: python

   >>> info['nfe'][:10]
   array([31, 35, 43, 49, 53, 57, 59, 63, 65, 69], dtype=int32)

Tenga en cuenta que el solucionador requiere más iteraciones para la primera etapa de tiempo. La solución de la trayectoria ``yvec`` puede ahora ser graficada:

.. plot:: pyplots/odeint_introduction.py
   :scale: 70

Otro ejemplo con :func:`scipy.integrate.odeint` será un oscilador amortiguado masa-resorte (oscilador de segundo orden). La posición de la masa unida a un resorte obedece
la ecuación diferencial de segundo orden ``y'' + 2 eps wo y' + wo^2 y = 0`` con ``wo^2 = k/m`` siendo ``k`` la constante del resorte , ``m`` la masa y ``eps = c/(2 m wo)`` siendo ``c`` el coeficiente de amortiguamiento. Para este ejemplo, elegimos los parámetros

.. code-block:: python

   >>> mass = 0.5  # kg
   >>> kspring = 4  # N/m
   >>> cviscous = 0.4  # N s/m

por lo que el sistema es subamortiguado, porque

.. code-block:: python

   >>> eps = cviscous / (2 * mass * np.sqrt(kspring/mass))
   >>> eps < 1
   True

Para usar el solucionador :func:`scipy.integrate.odeint` la ecuación de segundo orden debe ser transformado en un sistema de dos ecuaciones de primer orden mediante el vector ``Y = (y, y')``. Serás conveniente definir ``nu = 2 eps * wo = c / m`` y ``om = wo^2 = k/m``

.. code-block:: python

   >>> nu_coef = cviscous / mass
   >>> om_coef = kspring / mass

La función que calculará la velocidad y la aceleración será

.. code-block:: python

   >>> def calc_deri(yvec, time, nuc, omc):
   ...     return (yvec[1], -nuc * yvec[1] - omc * yvec[0])
   ...
   >>> time_vec = np.linspace(0, 10, 100)
   >>> yarr = odeint(calc_deri, (1, 0), time_vec, args=(nu_coef, om_coef))

La posición final y la velocidad se muestran en la siguiente figura creada con Matplotlib:

.. plot:: pyplots/odeint_damped_spring_mass.py
   :scale: 70

Scipy no tiene solucionadores de ecuaciones diferenciales parciales (PDE). Algunos paquetes Python para la resolver PDE están disponibles, tales como fipy_ o SfePy_.

.. _fipy: http://www.ctcms.nist.gov/fipy/
.. _SfePy: http://code.google.com/p/sfepy/

Procesamiento de señales: :mod:`scipy.signal`
---------------------------------------------

.. code-block:: python

   >>> from scipy import signal

* :func:`scipy.signal.detrend`: eliminar la tendencia lineal de la señal

  .. code-block:: python

     t = np.linspace(0, 5, 100)
     x = t + np.random.normal(size=100)

     pl.plot(t, x, linewidth=3)
     pl.plot(t, signal.detrend(x), linewidth=3)

  .. plot:: pyplots/demo_detrend.py
     :scale: 70

* :func:`scipy.signal.resample`: remuestrea una señal a `n` puntos usando FFT.

  .. code-block:: python

     t = np.linspace(0, 5, 100)
     x = np.sin(t)

     pl.plot(t, x, linewidth=3)
     pl.plot(t[::2], signal.resample(x, 50), 'ko')

  .. plot:: pyplots/demo_resample.py
     :scale: 70

  .. only:: latex

  Note como en el lado de la ventana del remuestreo es menos precisa y tiene un efecto de ondulación.

* :mod:`scipy.signal` tiene muchas funciones como: :func:`scipy.signal.hamming`,
  :func:`scipy.signal.bartlett`, :func:`scipy.signal.blackman`...

* :mod:`scipy.signal` tiene filtros (median filter :func:`scipy.signal.medfilt`,
  Wiener :func:`scipy.signal.wiener`), discutiremos esto en la sección de imagen.

Procesamiento de imágenes: :mod:`scipy.ndimage`
-----------------------------------------------

.. include:: image_processing/image_processing.rst

Sumario de ejercicios para computación científica
-------------------------------------------------

El sumario de ejercicios utiliza principalmente Numpy, Scipy y Matplotlib. Se proporcionan algunos ejemplos de la vida real en computación científica con Python. Los fundamentos para trabajar con Numpy y Scipy fueron introducidos, se invita a el usuario interesado probar los ejercicios.

.. only:: latex

    .. toctree::
       :maxdepth: 1

       summary-exercises/stats-interpolate.rst
       summary-exercises/optimize-fit.rst
       summary-exercises/image-processing.rst
       summary-exercises/answers_image_processing.rst
       summary-exercises/routing_reservoir.rst
       summary-exercises/routing_river_channel.rst
       summary-exercises/answers_routing_river_channel.rst
       summary-exercises/fit_flow_rate.rst

.. only:: html

   **Ejercicios:**

   .. toctree::
       :maxdepth: 1

       summary-exercises/stats-interpolate.rst
       summary-exercises/optimize-fit.rst
       summary-exercises/image-processing.rst
       summary-exercises/routing_reservoir.rst
       summary-exercises/routing_river_channel.rst
       summary-exercises/fit_flow_rate.rst

   **Soluciones propuestas:**

   .. toctree::
      :maxdepth: 1

      summary-exercises/answers_image_processing.rst
      summary-exercises/answers_routing_river_channel.rst
