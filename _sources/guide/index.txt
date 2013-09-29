.. _guide

=================
Cómo contribuir
=================

:authors: Nicolas Rougier

.. topic :: Prólogo

   Utilice la palabra clave ``topic`` para cualquier prólogos


.. contents :: Capítulos contenidos
   : local:
   : depth: 1


Asegúrese de leer esta guía `Documentation style guide`_ y éstos
`tips, tricks`_ y convenciones sobre el contenido de la documentación y los flujos de trabajo.


¿Cómo colaborar?
===================

Elija un tema que todavía no está cubierto y escribir así!

Crear un nuevo directorio en ``intro`` o ``advanced`` y comenzar a
escritura en ``index.rst``. No se olvide de actualizar también el ``index.rst`` en
la parte correspondiente de tal manera que su nuevo tutorial aparece en la tabla de
contenidos.

También hay que tener en cuenta que estos tutoriales son para ser enseñado en diferentes lugares
y las diferentes partes se pueden combinar en un curso de Python para científicos
informática. Así que queremos que sean muy interactivos y razonablemente corto
(De una a dos horas) o su público sólo podría dormir mucho antes de que haya
terminado de hablar ...

Por último, pero no menos importante, el objetivo de este material es proporcionar un texto conciso
útil para el aprendizaje de las principales características del ecosistema scipy. Si usted desea
contribuir a material de referencia, le sugerimos que usted contribuye a la
documentación de los paquetes específicos que usted está interesado.

Mantenerlo concisa: párrafos colapso
====================================

La salida HTML se utiliza para la visualización en la pantalla mientras se enseña. El objetivo
es tener el mismo material de muestra como en las notas. Por lo tanto es necesario
ser una pantalla muy conciso, con bala listas más que en toda regla
párrafos y oraciones. Sin embargo, en el largo plazo, es útil tener
conversaciones más elaboradas que la gente puede leer y consultar. Para esto,
la directiva sphinx ``tip``creará párrafos plegables, que pueden
se oculta durante una presentación oral ::

    .. tip::

        A continuación inserte una discusión en toda regla, que será plegable en
        la versión HTML.

        Puede extenderse sobre varios párrafos

Esto hace de la siguiente manera:

    .. tip::

        A continuación inserte una discusión en toda regla, que será plegable en
        la versión HTML.

        Puede extenderse sobre varios párrafos

Figuras y ejemplos de código
============================

**Nosotros no revisamos las figuras en el depósito.**
Cualquier figura generada a partir de un script Python debe ser nombrado
``plot_xxx.py`` (xxx puede ser cualquier cosa, por supuesto) y poner en el directorio ``examples``. La imagen generada se llamará del nombre de script.

.. imagen:: auto_examples/images/plot_simple_1.png
   :target: auto_examples/plot_simple.html


Esta es la forma de incluir su imagen y vincularla con el código:

.. code-block:: rst

   .. image:: auto_examples/images/plot_simple_1.png
      :target: auto_examples/plot_simple.html

Se puede visualizar el código correspondiente utilizando la directiva ``literal-include``.

.. literal-include:: examples/plot_simple.py

.. note::

    El código para proporcionar este tipo de inclusión plan fue adoptado del
    proyecto scikits.learn y se puede encontrar en ``sphinxext/gen_rst.py``.

El uso de marcas
================

Hay tres tipos principales de marcas que se deben utilizar: *cursiva*, **negrita**
y ``fuente fija``. *Cursiva* debe ser utilizado en la introducción de una nueva técnica
plazo, **negrita** debe ser utilizado para dar énfasis y ``fuente fija`` para el código fuente.

.. topic :: Example:

    Cuando se utiliza la *programación orientada a objetos* en Python que se debe *utilizar* la palabra clave ``class`` para definir *clases*.

En reestructurado-text markup es::

    Cuando se utiliza la *programación orientada a objetos* en Python que se debe *utilizar* la palabra clave ``class`` para definir *clases*.

Enlaces a documentación del paquete
===================================

El objetivo de los apuntes de clase scipy no es duplicar o sustituir el
la documentación de los diferentes bultos. Usted debe vincular lo más
posible a la documentación original.

Para obtener la documentación API referencias cruzadas se prefiere usar el `intersphinx
extensión <http://sphinx-doc.org/latest/ext/intersphinx.html>`_. Esto proporciona
las directivas `:mod:`, `:class:` y `:func:` para reticular los módulos,
clases y funciones, respectivamente. Por ejemplo, el ```: func: `numpy.var``` se
crear un enlace como :func:`numpy.var`.

Capítulo, artículo, inciso, párrafo
===================================

Trate de evitar ir a continuación del párrafo granularidad o el documento podría llegar a ser difícil de leer:

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

La forma más fácil de hacer su propia versión de este material didáctico
es a la mesa bajo Github, y utilizar el sistema de control de versiones Git
mantener su propio tenedor. Para ello, lo único que tienes que hacer es crear una cuenta en github (este sitio) y haga clic en el botón *fork*, en la parte superior derecha de esta página. Puede usar git para tirar de su *fork* y hacer retroceder a la misma el cambios. Si quieres contribuir a los cambios de nuevo, sólo tiene que rellenar un *pull request*, utilizando el botón en la parte superior de la página fork.

Por favor, abstenerse de modificar el Makefile a menos que sea absolutamente
necesario.

Advertencias
============

.. note::
   
   Esta es una nota

.. warning::

   Esta es una advertencia

Limpieza de flotantes
=====================

Figuras posicionadas con `: align: right` son flotantes. Para limpiar, use::

    |clear-floats|

Referencias
===========

.. target-notes::

.. _`Documentation style guide`: http://documentation-style-guide-sphinx.readthedocs.org/en/latest/style-guide.html
.. _`tips, tricks`: http://docness.readthedocs.org/en/latest/index.html
