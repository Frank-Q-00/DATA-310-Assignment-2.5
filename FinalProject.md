# Data 310 Final Project: Trajectory Prediction Based on GPS Data 

<br>
<br>

## Problem Statement

<br>

**Initiative**
- GPS  data is critical in analyzing movements of individuals or groups of people in a certain area
- The prevalence of cellphone makes it easier to collect personal GPS data (many apps ask for access to your location when you’re using it)
- Decide to make use of the GPS data for predictions
- Due to the privacy concerns, I would start with my personal GPS data

<br>

**Goal**
- Based my historical GPS data with time (longitude, latitude, and time when I’m at the location), build a machine learning model that will predict my future movements (trajectories).
- Assess how the model can be improved in order to apply to a dataset with a large number of agents. 

<br>

**Significance**
- At individual level, getting people’s trajectory enables us to create personalized recommendations for that individual. 
  - if we predict that a person will travel from suburban area to city center,  then we can advertise apartments for lease. 
- At group level, the ability to predict people’s future trajectory is even more significant.
  - Urban planning: building a new shopping mall based on where people frequently go
  - Public health: predict the spread of a disease

<br>

**Central Question:
How to predict my future movement trajectory based on my movement records?**

<br>
<br>

## Data Description

<br>

**Data Source**
- The Moveflow App developed by Mr. Gregor Laemmel and Dr. Tyler Frazier. 
- Collect the user’s location data while giving user flowcoins (a cryptocurrency) back as reimbursement
- Receive the data compiled by Mr. Gregor in two files, one gpx file, one jsonl file. 

![](./FinalProject/moveflow_ss.png)

<br>

**GPX file:** contains 4 columns: longitude, latitude, altitude and time stamp. 

![](./FinalProject/gpx_ss.png)

<br>

**Jsonl file:** contains 3 more columns: speed, activity and course.

![](./FinalProject/jsonl_ss.png)

<br>

Although the jsonl file contains information that might be useful in the future (activity), I decide to use the gpx file since it’s time format is more readable and can be fit into the model.

<br>

Eventually,  I retrieve a pandas dataframe from the gpx file, the size of which is 5008 rows by 3 columns. The three columns are: X for longitude, Y for latitude, and time. 

![](./FinalProject/df_ss.png)

<br>
<br>



## Model and Results

<br>

**Model Description**
We use the scikit-mobility package in Python. This package is able to 
- Process mobility data of various forms, including GPS data
- Extract mobility metrics and patterns from data
- Generate synthetic individual trajectories and move flows

The majority of the codes used to generate the model come from the scikit-mobility github repository. 

<br>

**Trajectory Data Frame**

``` Python

import skmob

# Create a TrajDataFrame
tdf = skmob.TrajDataFrame(track_points)
tdf.columns = ['lng','lat','datetime']
tdf['uid'] = 1
tdf

tdf.plot_trajectory(zoom=12, weight=3, opacity=0.9, tiles='Stamen Toner')

```
