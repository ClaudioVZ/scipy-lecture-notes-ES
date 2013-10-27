.. _guide

===============
Cómo contribuir
===============

:Autores: Nicolas Rougier

.. topic:: Prólogo

   Utilice la palabra clave ``topic`` para cualquier prólogo

.. contents:: Contenido
   :local:
   :depth: 1

Asegúrese de leer `Documentation style guide`_, `tips, tricks`_ y convenciones sobre el contenido de la documentación y flujo de trabajo.

Cómo colaborar?
===============

Elija un tema que todavía no está cubierto y escriba así!

Crear un nuevo directorio en ``intro`` o ``advanced``, crear un archivo ``index.rst`` y comenzar a escribir. No se olvide de actualizar el archivo ``scipy-lecture-notes/index.rst`` de tal manera que su nuevo tutorial aparezca en la tabla de contenidos.

También hay que tener en cuenta que estos tutoriales pueden ser usados para la enseñanza y las diferentes partes se pueden combinar para formar un curso de Python para computación científica. Queremos que sean muy interactivos y razonablemente cortos (de una a dos horas) o su público podría dormirse mucho antes de que haya terminado de hablar ...

Por último, pero no menos importante, el objetivo de este material es proporcionar un texto conciso para el aprendizaje de las principales características del ecosistema scipy. Si usted desea contribuir al material de referencia, le sugerimos contribuir a la documentación de los paquetes específicos en los que está interesado.

Mantengalo conciso: párrafos colapsados
=======================================

La versión HTML se utiliza para mostrarlo en pantalla mientras se enseña. El objetivo es tener el mismo material como en las notas de muestra. Por tanto es necesario mostrarlo de una concisa, con párrafos y oraciones cortas. Sin embargo, a largo plazo, es útil tener párrafos más elaborados que la gente pueda leer y consultar. Para esto, la directiva sphinx ``tip``creará párrafos plegables, que se pueden ocultar durante una presentación oral

.. code-block:: rst

   .. tip:: A continuación inserte una discusión en toda regla, que será plegable en la versión HTML.
      
      Puede extenderse sobre varios párrafos

Esto hace de la siguiente manera:

.. tip:: A continuación inserte una discusión en toda regla, que será plegable en la versión HTML.
   
   Puede extenderse sobre varios párrafos

Figuras y ejemplos de código
============================

**Nosotros no revisamos las figuras en el repositorio.** Cualquier figura generada a partir de un script Python debe ser nombrado como ``plot_xxx.py`` (xxx puede ser cualquier cosa, por supuesto) y debe estar en el directorio ``examples``. La imagen generada se llamará usando el nombre del script.

.. image:: auto_examples/images/plot_simple_1.png
   :target: auto_examples/plot_simple.html

Esta es la forma de incluir una imagen y vincularla con el código:

.. code-block:: rst

   .. image:: auto_examples/images/plot_simple_1.png
      :target: auto_examples/plot_simple.html

Se puede visualizar el código correspondiente utilizando la directiva ``literalinclude``.

.. literalinclude:: examples/plot_simple.py

.. note:: El código para proporcionar este tipo de inclusión fue adoptado del proyecto scikits.learn y se puede encontrar en ``sphinxext/gen_rst.py``.

Usando un lenguaje de marcado
=============================

Hay tres tipos principales de marcado que se debe utilizar: *cursiva*, **negrita**
y ``fuente fija``. *Cursiva* debe ser utilizado en la introducción a una nueva técnica, **negrita** debe ser utilizado para dar énfasis y ``fuente fija`` para el código fuente.

.. topic:: Ejemplo:

   Cuando se utiliza la *programación orientada a objetos* en Python que se debe *utilizar* la palabra clave ``class`` para definir *clases*.

El texto anterior en reStructuredText es:

.. code-block:: rst

   Cuando se utiliza la *programación orientada a objetos* en Python que se debe *utilizar* la palabra clave ``class`` para definir *clases*.

Enlaces a documentación del paquete
===================================

El objetivo de los apuntes de clase scipy no es duplicar o sustituir la documentación de los diferentes paquetes. Se recomienda crear enlaces a la documentación original.

Para obtener referencias cruzadas a la documentación API es preferible usar `intersphinx
extension <http://sphinx-doc.org/latest/ext/intersphinx.html>`_. Esto proporciona
las directivas `:mod:`, `:class:` y `:func:` para enlazar los módulos, clases y funciones respectivamente. Por ejemplo, ``:func:`numpy.var``` crea un enlace a :func:`numpy.var`.

Capítulo, artículo, inciso, párrafo
===================================

Trate de evitar párrafos sin estrutuctura o el documento podría llegar a ser difícil de leer:

.. code-block:: rst

   ===================
   Título del capítulo
   ===================

   Contenido de la muestra.

   Section
   =======

   Subsection
   ----------

   Paragraph
   .........

   Y un poco de texto.


Usando Github
=============

La forma más fácil de hacer su propia versión de este material didáctico es Github, utilizando el sistema de control de versiones git para mantener su propia versión. Para ello, lo único que tienes que hacer es crear una cuenta en github (este sitio) y hacer clic en el botón *fork*, en la parte superior derecha de esta página. Puede usar ``git pull`` para actualizar tu *fork*. Si quiere contribuir a la versión original, sólo tiene que hacer un *pull request*, utilizando el botón en la parte superior de la página de su fork.

Por favor, abstengase de modificar el Makefile a menos que sea absolutamente necesario.

Advertencias
============

.. note:: Esta es una nota

.. warning:: Esta es una advertencia

Limpieza de objetos flotantes
=============================

Figuras posicionadas con `:align: right` son flotantes. Para limpiar, use

.. code-block:: rst

   |clear-floats|

Referencias
===========

.. target-notes::

.. _`Documentation style guide`: http://documentation-style-guide-sphinx.readthedocs.org/en/latest/style-guide.html
.. _`tips, tricks`: http://docness.readthedocs.org/en/latest/index.html
