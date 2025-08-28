# Preprocessing-Data-for-GraphCast

This repo provides an overview of preparing the input dataset to be used by GraphCast operational, a model of GraphCast that forecasts atmospheric conditions using 37 pressure levels and 25km (.25 degree) resolution. Note that GraphCast provides its own datasets to fine-tune the model. Specifically, this repo uses an example of preprocessing for ERA data from June 30 through July 3. The data we need to get consists of 12 time steps (14 steps): 

* June 30: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 1: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 2: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 3: 0:00:00, 6:00:00

This will be used to obtain 3-Day forecasts for the following times:

* July 3: 12:00:00, 18:00:00
* July 4: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 5: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 6: 0:00:00

The workflow for preprocessing is as follows:

* Create an account with ERA5's Climate Data Store: https://cds.climate.copernicus.eu/datasets .
* Find the `ERA5 hourly data on single levels from 1940 to present` dataset: https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=overview
* In the download tab, from the "popular" panel select the variables "10 m u", "10 m v", "2 m temp", "Total Precipitation", "Mean Sea Level Pressure". In the "Radiation and Heat" panel select "TOA" (this is used as a forcing variable by GraphCast). In the "Other" panel select "Geopotential" and "Land-sea Mask".
* For "Year" select 2025, "Month" June and "Day" 30. We cannot select June 30 and July 1,2 at the same time. For time, select 00:00, 06:00, 12:00, and 18:00.
* Choose netcdf4 as the file type. Click submit form and navigate to "your request" to see the status of the request (single file requests should take about 3 minutes, pressure-level requests 5 minutes).
* Fill out the similar download form for the single-level  dataset for the days of July 1,2. July 1,2 can be filled out in the same request since they download the same time data. July 3 must be requested separately. 
* Once downloaded, there should be 3 folders for each day and inside the folders are instant and accumulated files.
* Now download the pressure-level dataset by finding the `ERA5 hourly data on pressure levels from 1940 to present` dataset.
* In the download tab under the "Variable" panel select "Geopotential", "Specific humidity", "U-component", "V-component", "Temperature" and "Vertical Velocity". Note that some variable names are the same as in the single-level dataset, but the single level variables will be named as surface variables when used by GraphCast.

Now open 'preprocessing.ipynb' and follow the steps listed there. An overview of each function is given: 

* `merge_single_ins_accum(input_folder, output_folder)`: function used to merge each single-level folder with components of an instant file and accumulated file into a single-level file. There should be 3 distinct single-level files within the output_folder after this step.

# Cloud Computing Using HiPerGator

* The merged dataset file should be uploaded to the user's blue storage in HiPerGator using the following command on a Linux shell terminal: `rsync -z [path to local file] [username]@hpg.rc.ufl.edu:[path to directory on HiPerGator]`. If using Bash, rsync must be installed. If using Window's Subsystem Linux (WSL), rsync is already available. Use 'wsl --install' if wsl is not already installed. Rsync allows large local files to be compressed and uploaded to a cloud system.
* Source files such as 'graphcast_demo.ipynb' (used to run GraphCast model and provided by Google's GraphCast git repo) can be uploaded with the user's home directory on HiPerGator, while input files should be uploaded to blue storage. To move a local file to HiPerGator, open a new terminal and use the command `scp [path to local file] [username]@hpg.rc.ufl.edu:[path to destination on HiPerGator]`.
* Some operations such as beginning an interactive session to run Jupyter Notebook can be done on HiPerGator's ondemand GUI; however the GUI can often fail to complete operations such as job script edits and moving files. So, it is useful to peform copying operations as well as checking the job request by connecting to the HiPerGator interface on the command line. To do this, open a terminal and login to HiPerGator using `ssh [username]@hpg.rc.ufl.edu`.
* Setting up environmental modules within Jupyter Notebook: 

