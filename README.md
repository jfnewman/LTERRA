# L-TERRA
Code to run the Lidar Turbulence Error Reduction Algorithm (L-TERRA) for vertically profiling lidars. 

# Introduction

This is a repository for the Lidar Turbulence Error Reduction Algorithm (L-TERRA), a Python toolbox that reads in lidar data and applies a series of turbulence intensity (TI) corrections. More information on the corrections used in L-TERRA can be found in the following reference: 
Newman, J. F. and A. Clifton, 2017: An error reduction algorithm to improve lidar turbulence estimates for wind energy. Wind Energ. Sci., 2, 77-95.

# Requirements

1. Python distribution with numpy
2. scipy
3. sklearn (not absolutely necessary, but nice for calculating metrics)

L-TERRA was developed and tested with Python 2.7.10. 

# Data Preparation 

L-TERRA requires data to be in numpy format (.npz) with one data file for each 10-minute period of the dataset. Numpy files are much smaller and easier to navigate than CSVs and processing time is reduced significantly by only reading in 10 minutes of data at a time. Scripts are included that read in WINDCUBE .rtd files or high-resolution ZephIR .ZPH files, perform some basic calculations, and write the data out to .npz files. 

Measurements from the reference instrument (sonic or cup anemometer) should also be written out to .npz files with the following naming convention:

Ref_YYYYMMDD_HHMM_UTC.npz

where HHMM UTC is the start of the 10-minute averaging period.

Reference instrument files should contain the following variables:

U: 10-min. mean horizontal wind speed 

wd: 10-min. mean wind direction

u_var: 10-min. streamwise variance

# Running Code 

L-TERRA is comprised of two main scripts:

`test_L_TERRA_corrections_XX.py`: Reads in lidar and reference data, loops through all combinations of corrections, and writes out TI mean absolute error (MAE) for each combination to a CSV file

`apply_L_TERRA_corrections_XX.py`: Reads in lidar data, applies user-selected set of corrections, and writes out corrected lidar TI values to a CSV file 

The testing script should be used to determine optimal model combinations for a given dataset, while the application script should be used to apply the optimal model combination to a new dataset. Versions of these scripts are given for use with both WINDCUBE v2  (WC) and ZephIR 300 (ZephIR) data. 

To use `test_L_TERRA_corrections_XX.py`:

* Put reference data and lidar data in two separate folders
* In code, specify:
    * Directory where reference data are stored (reference_directory)
    * Directory where lidar data are stored (lidar_directory)
    * Directory where CSV output file should be saved (main_directory)
    * Height where TI comparison is being made
    * Date range to read in
    * Wind speed and wind direction thresholds (if any) to determine valid data points

To use `apply_L_TERRA_corrections_XX.py`:

* Put lidar data for L-TERRA application in a folder
* In code, specify:
    * Directory where lidar data are stored (lidar_directory)
    * Directory where CSV output file should be saved (main_directory)
    * Height where TI correction is being applied
    * Date range to read in
    * Wind speed and wind direction thresholds (if any) to determine valid data points
    * Corrections to use for different stability conditions
