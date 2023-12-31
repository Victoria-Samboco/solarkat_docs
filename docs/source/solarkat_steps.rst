
SolarKAT Steps
==============
SolarKAT is composed of several steps, which are executed in the order below. This section includes the description and the respective parameters involved in each step.


1. Input data
^^^^^^^^^^^^^

SolarKAT ingests 1GC calibrated data in Measurement Set format from the MeerKAT telescope.

- ms: The Measurement Set to be processed (MS)

2. imaging
^^^^^^^^^^^

This step composes the first imaging cycles, which is performed before self-calibration. The solarkAT envolves two (2) imaging steps before self-cal. The imaging cycles are separated by a masking step. More information about the WSClean parameters can be found in the `WSClean manual <https://wsclean.readthedocs.io/en/latest/>`_ .


- `column`
- `niter`
- `padding`
- `size`
- `auto-threshold`
- `temp_dir`
- `save-source-list`
- `use-wgridder`
- `wgridder-accuracy`


3. self-calibration (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `input_ms.path`: '{recipe.ms}'
- `solver.terms`: [K]
- `K.type: phase`
- `K.direction_dependent`: false
- `K.freq_interval`: '0'
- `K.time_interval`: '4' 
- `K.initial_estimate`: true
- `input_ms.time_chunk`: '128'
- `solver.iter_recipe`: [100]
- `input_model.recipe`: MODEL_DATA
- `output.overwrite`: true
- `output.products`: [corrected_data]
- `output.columns`: [CORRECTED_DATA]


4. backup_model_data (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This step backups the *MODEL_DATA* of the original MS before processing. This is done just in case we want to compare the model of the main field before and after removing the inpluence of the Sun in the data. What this step does is basicaly renaming the *MODEL_DATA* column. You can use this step if you want to preserve the initial model_data of the original field for future use (for example for conparison with the improved model).

- `ms`:
- `oldname`: MODEL_DATA
- `newname`: MODEL_DATA_ORIGINAL


5. scan_numbers_extraction (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


- `ms`
- `outfile`: A text file containing the scans numbers.


6. load_scan_numbers (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Loading the scan numbers for future use.

- `scans_file` : Call the output file generated in the previous step.

7. split_ms_by_scan (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Split the MS using the scan_numbers loaded previously. PS: For this step to work you have to ensure to run simultaneously with the "load_scan_numbers" step. This is done in a loop over the scan list.

- `ms`
- `scan_list` : list of scans
- `vis` : Visibiliy (from CASA)
- `datacolumn`: Columns to be considered. SolarKAT uses the option 'all'
- `scan`: represents each scan in the scan_list
- `outputvis` : The output scans named acording to the scan numbers.

The output of this step are small chunks of data in MS format. This MS are named according to the scan numbers extracted from the MS. The pipeline make use of the substitutions and formulas features ofered by stimela. An example of the use is the "outputvis" parameter:

.. code-block:: console

    outputvis: ="{root.dir_out}" + "/" + "{root.perscan_dir_out}" + "/" + STRIPEXT(BASENAME(recipe.ms)) + "_scan_{recipe.scan}.ms"


This allows the pipeline to name the output scans according the scan numbers in a combination of substituitions and formulas. Now lets breackdown what is happening here:

Parameter strings starting with *=* represents formulas. in this case, the outputis parameter is a formula that 


8. get_perscan_old_coords (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `ms_list`
- `outfile` : Text file containing the original coordinates (RA/DEC)


9. get_sun_coordinates (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


- `ms`
- `outfile` : Text file containing the Sun coordinates (RA/DEC)


10. change_phase_centre_to_sun (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


- `ms_list` : list of scans to be processed
- `coords` : Sun coordinates
- `splitted_ms_dir` : The directory where the scans are stored

In this step the pipeline access the *splited_ms_dir* directory and the path to the sun coordinates file and use them to change the phase centre of each scan in the *ms_list* from the main field to teh Sun field.

11. image_sun (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Dirty image of the Sun for each scan.

- `ms`
- `ms_list`
- `niter`
- `size`
- `column`
- `wgrider-accuracy`
- `temp_dir`
- `no-update-model-required`
- `prefix`

The output of this step are images of the solar disk.


12. create_ds9_regions (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `ms`
- `input_file` : The file containing the Sun coordinates
- `output_dir` : Where the regions are stored


13. make_mask (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This step includes auxiliar steps (*load_scan_numbers* and *making_masks*) as a input for the main step (*make_mask*)

- `scan_list` : List of scan numbers
- `scans_file` : File containing the scan numbers
- `region_dir` : Path to the DS9 regions
- `restored_image` : Path to the solar images 
- `threshold` : Threshold
- `merge` : Full path to each region file acoording to the scan number
- `mask` : Output mask according to the scan number


14. deconvolve_sun (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Deconvolve the Sun for each scan MS.

- `ms` 
- `mask_list` 
- `ms_list` 
- `size`
- `niter`
- `multiscale`
- `threshold`
- `join-channels`
- `fit_spectral_pol`
- `auto-threshold`
- `save-source-list`
- `column`
- `temp_dir`
- `prefix`
- `fits-mask` 

Multiscale is important for this task as the Sun is an extendend source. See more details of the parameters here `WSClean manual <https://wsclean.readthedocs.io/en/latest/>`_ .

Note that the *threshold* depends on each dataset and can be defined from the Signal-to-noise (SNR) values on the initial dirty image or from other methods.
The *auto_threshold* is unset using the keyword *=UNSET*. The parameter *multiscale* is set to *true*  to allow deconvolution in the multiple scales of the Sun.  

15. predict_sun_model (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Predicting the Sun models.

- `ms`
- `ms_list`
- `scans`
- `mask_list`
- `size`
- `predict`
- `padding`
- `temp_dir`
- `prefix`

See more details of the steps in the `WSClean manual <https://wsclean.readthedocs.io/en/latest/>`_ .



16. quality_control_imaging1 (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This step is generaly to check the MODEL_DATA column after prediction

- `ms_list`

17. restore_phase_centre (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `ms_list` : List of MS scans
- `coords` : The path to the original field coordinates
- `splitted_ms_dir` : The directory where the MS scans are stored


18. quality_control_imaging2 (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This step is generaly to check if the rephasing step was successfull.

- `ms_list`


19. add_model_data_columnn (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Add columns to the original MS.

- `ms`
- `col_names` : Columns to add in the single (original) MS (*MODEL_DATA_SUN*, *CORRECTED_DATA_SUN*)
- `like-col` : Reference column

20. data_storage (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `ms`
- `ms_list` 
- `copycol` : Column to copy from each MS scan (*MODEL_DATA* column)
- `tocol` : Column to copy to in the single (original) MS (*MODEL_DATA_SUN*)


21. subtract_sun (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Subtract the Sun (*MODEL_DATA_SUN*) from the afected visibilities (*CORRECTED_DATA*) for each scan.

- `ms`
- `commands` : Command to exectute the subtraction 

Example:

.. code-block:: console

    commands: =LIST("set", "CORRECTED_DATA_SUN=CORRECTED_DATA-MODEL_DATA_SUN")

22. image-corrected-data (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This steps updates the main field MODEL_DATA after Sun subtraction. This is the refined model used in the *cal_and_peel_sol* step.

- `ms` 
- `padding`
- `niter`
- `size`
- `auto_threshold`
- `fits_mask`
- `column`
- `temp_dir`


23. save-flags-3 (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Manage flags before peeling

- `ms`
- `name`
- `mode`: save/restore


24. cal_and_peel_sol (required)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Calibration and peeling of the Sun

- `input_ms.path`: '{recipe.ms}'
- `input_ms.weight_column`: WEIGHT_SPECTRUM
- `input_ms.time_chunk`: '14'
- `input_ms.freq_chunk`: '0'
- `input_ms.select_uv_range`: [0,0]
- `input_ms.group_by`: [FIELD_ID,DATA_DESC_ID,SCAN_NUMBER]
- `input_model.recipe`: MODEL_DATA:MODEL_DATA_SUN 
- `input_model.apply_p_jones`: false
- `input_ms.is_bda`: False
- `solver.terms`: [K,dE]
- `solver.iter_recipe`: [100,50]
- `solver.propagate_flags`: false
- `solver.robust`: false 
- `solver.threads`: 4
- `dask.threads`: 8
- `output.gain_directory`: '{recipe.dir_out}/{recipe.qcal_output_dir}/peeled.qc'
- `output.log_directory`: '{recipe.dir_out}/{recipe.qcal_output_dir}/'
- `output.overwrite`: true
- `output.products`: [corrected_residual]
- `output.columns`: [CORRECTED_RESIDUAL]
- `output.subtract_directions`: [1]
- `output.flags`: true 
- `output.apply_p_jones_inv`: false
- `mad_flags.enable`: false
- `dask.scheduler`: threads
- `K.type`: phase  
- `K.direction_dependent`: false
- `K.time_interval`: 4
- `K.freq_interval`: 0
- `K.initial_estimate`: true
- `dE.type`: complex
- `dE.time_interval`: 14
- `dE.freq_interval`: 64 
- `dE.initial_estimate`: False
- `dE.direction_dependent`: true
- `dE.pinned_directions`: [0] 


25. save-flags-4 (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Manage flags after peeling (same as the step *save-flags-3*)

26. image (required)
^^^^^^^^^^^^^^^^^^^^

Image *CORRECTED_RESIDUAL* generated in the peeling process. This composes the last imaging cycle of the pipeline.


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
