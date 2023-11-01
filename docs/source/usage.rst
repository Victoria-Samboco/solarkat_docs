.. Usage
.. =====

.. .. _installation:

.. Installation
..- -----------

.. To use SolarKAT, first git clone it using:

.. .. code-block:: console

.. $ git clone https://github.com/Victoria-Samboco/solarkat_docs.git

SolarKAT Components
-------------------
SolarKAT is deployed in 'Stimela <https://stimela.readthedocs.io/en/latest/fundamentals/basics.html>'_, a workflow management framework. SolarKAT is composed by three main components:  

Python file
^^^^^^^^^^^
Where all the funtions are defined.

Cab
^^^
A YAML document that tells stimela how to involke a certain SolarKAT task. A cab is Where all the inputs and outputs of the pipeline steps are defined. The cabs involkes python functions defined in the Python file as well as CASA tasks commands. 

Recipe
^^^^^^
Is a YAML file Which contains the SolarkATs workflow and in which sequence  of steps the interference mitigation process will be executed.  Each of the steps ivolkes a cab to execute a task. Each od the steps have parameters that are defined in the cab file.


SolarKAT Steps
--------------
SolarKAT is composed by several steps which are executed in order:

Input MS
^^^^^^^^
Calibration
^^^^^^^^^^^

Backup_model
^^^^^^^^^^^^

Scan_numbers_extraction
^^^^^^^^^^^^^^^^^^^^^^^

load_scan_numbers
^^^^^^^^^^^^^^^^^

split_ms_by_scan
^^^^^^^^^^^^^^^^

get_perscan_old_coords
^^^^^^^^^^^^^^^^^^^^^^

get_sun_coordinates
^^^^^^^^^^^^^^^^^^^

change_phase_centre_to_sun
^^^^^^^^^^^^^^^^^^^^^^^^^^

image_sun
^^^^^^^^^
create_ds9_regions
^^^^^^^^^^^^^^^^^^

make_mask
^^^^^^^^^

deconvolve_sun
^^^^^^^^^^^^^^

predict_sun_model
^^^^^^^^^^^^^^^^^

quality_control_imaging
^^^^^^^^^^^^^^^^^^^^^^^

restore_phase_centre
^^^^^^^^^^^^^^^^^^^^

add_model_data_columnn
^^^^^^^^^^^^^^^^^^^^^^

data_storage
^^^^^^^^^^^^

subtract_sun
^^^^^^^^^^^^

image-corrected-data
^^^^^^^^^^^^^^^^^^^^

save-flags-3
^^^^^^^^^^^^

cal_and_peel_sol
^^^^^^^^^^^^^^^^

save-flags-4
^^^^^^^^^^^^

image
^^^^^




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

