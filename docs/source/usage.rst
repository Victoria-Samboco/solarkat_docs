.. Usage
.. =====

.. .. _installation:

.. Installation
.. -----------

.. To use SolarKAT, first git clone it using:

.. code-block:: console

.. $ git clone https://github.com/ratt-ru/solarkat.git



SolarKAT Components
-------------------
SolarKAT is deployed in Stimela https://stimela.readthedocs.io/en/latest/fundamentals/basics.html, a workflow management framework. SolarKAT is composed by three main files:  

Python file
^^^^^^^^^^^
Where all the necessary functions are defined.

Cab
^^^
A YAML document, that tells stimela how to involke a certain SolarKAT task. A cab is Where all the inputs and outputs of the pipeline steps are defined. The cabs involkes python functions defined in the Python file as well as CASA tasks commands and softwares (such as QuartiCal, WSClean and Breizorro). 

Recipe
^^^^^^
Is a YAML file Which contains the SolarkATs workflow and in which sequence  of steps the interference mitigation process will be executed.  Each of the steps ivolkes a cab to execute a task. Each od the steps have parameters that are defined in the cab file.


.. autofunction:: lumache.get_random_ingredients

.. The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
.. or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
.. will raise an exception.

.. autoexception:: lumache.InvalidKindError

.. For example:

.. >>> import lumache
.. >>> lumache.get_random_ingredients()
.. ['shells', 'gorgonzola', 'parsley']


.. Commit Changes
.. After editing the files, commit the changes using Git. In the terminal
.. git add README.rst index.rst path/to/api_file.rst
.. git commit -m "Update documentation"
.. git push origin main 
.. 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
.. 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
