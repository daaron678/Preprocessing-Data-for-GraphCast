# Preprocessing-Data-for-GraphCast

This repo provides an overview of preparing the input dataset to be used by GraphCast operational, a model of GraphCast that forecasts atmospheric conditions using 37 pressure levels and 25km (.25 degree) resolution. Note that GraphCast provides its own datasets to fine-tune the model. Specifically, this repo uses an example of preprocessing for ERA data from June 30 through July 3. The data we need to get consists of 12 time steps (14 steps): 

* June 30: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 1: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 2: 0:00:00, 6:00:00, 12:00:00, 18:00:00
* July 3: 0:00:00, 6:00:00

This will be used to obtain 3-Day forecasts for the following times:

* 

The process for preprocessing is as follows:

* '
