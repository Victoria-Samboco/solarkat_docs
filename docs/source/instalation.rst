
Usage
=====

.. _installation:

Installation
------------

Git clone
^^^^^^^^^
To use SolarKAT, first git clone it using:

.. code-block:: console

	$ git clone https://github.com/ratt-ru/solarkat.git


.. pip install
.. ^^^^^^^^^^^

.. .. code-block:: console

.. 	$ pip install solarkat


Operating system 
-----------------

- `Linux (recommended) or macOS`



Execution
---------

.. _execution: 

Once SolarKAT has been successfully installed, it can be executed from the command line using:


.. code-block:: console

	$ stimela run solarkat.yaml <recipe name> obs = <observation option>


SolarKAT can be run specifying the steps to run in case the user want to run specific tasks, e.g:


.. code-block:: console

	$ stimela run solarkat.yaml solarkat obs = L1 -s image-1:image_sun


This example runs the pipeline from the first imaging step (image-1) to the image_sun step. If the start and end steps are not specified, the pipeline runs by default from the first to the last step in the recipe. 