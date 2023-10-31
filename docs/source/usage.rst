Usage
=====

.. _installation:

.. Installation
..------------

..To use SolarKAT, first git clone it using:

.. code-block:: console

.. $ git clone https://github.com/Victoria-Samboco/solarkat_docs.git

SolarKAT Components
-------------------
SolarKAT is composed by three main components:  a python file where all the funtions are defined; a cab definition file (YAML) where all the inputs and outputs of the pipeline steps are defined; and a recipe, which contains the steps in which sequence the interference mitigation process will be executed. 



SolarKAT Steps
--------------
SolarKAT is composed by several steps which are executed in order:

### Input MS

### Calibration

### Backup model

### Scan numbers extraction

### load_scan_numbers

### split_ms_by_scan

### get_perscan_old_coords

### get_sun_coordinates

### shift_to_sun

### image_sun

### create_ds9_regions

### make_mask

### deconvolve_sun

### predict_sun_model

### quality_control_imaging

### rephase

### add_model_data_columnn

### data_storage

### subtract_sun

### image-corrected-data

### save-flags-3

### cal_and_peel_sol

### save-flags-4

### image



.. autofunction:: lumache.get_random_ingredients

.. The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
.. or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
.. will raise an exception.

.. autoexception:: lumache.InvalidKindError

.. For example:

.. >>> import lumache
.. >>> lumache.get_random_ingredients()
.. ['shells', 'gorgonzola', 'parsley']

