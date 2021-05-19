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
