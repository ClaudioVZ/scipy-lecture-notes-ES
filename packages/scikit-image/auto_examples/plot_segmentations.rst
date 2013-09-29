

.. _example_plot_segmentations.py:


This example compares two segmentation methods in order to separate two
connected disks: the watershed algorithm, and the random walker algorithm.

Both segmentation methods require seeds, that are pixels belonging
unambigusouly to a reagion. Here, local maxima of the distance map to the
background are used as seeds.



.. image:: images/plot_segmentations_1.png
    :align: center


**Python source code:** :download:`plot_segmentations.py <plot_segmentations.py>`

.. literalinclude:: plot_segmentations.py
    :lines: 9-
    