# wildfire_capstone
My final project for the Flatiron Data Science Boot Camp attempts to use a Convolutional Neural Network that scans satellite images from Northern California and is ultimately able to distinguish between areas that have experienced a wildfire event (or fire instance as it will be refered to often) and those areas that have not.

## Table of Contents

This home repository contains the project environment, notebooks containing our exploratory data analysis, methods of model building, and conclusions as to the success of the overall project.

### Notebooks

### Reports
[Visualizations](reports/visualizations)

### Data
[How to access data](Notebooks/satellite_data_retrieval.ipynb)

### SRC

### Readme

[Read Me](README.md)


## Business Understanding

According to a [United States Congressional Report](https://fas.org/sgp/crs/misc/IF10244.pdf), published in 2020, since the year 2000, there have been on average 71,300 wildfires in the United States per year, and also on average 6.9 million acres of land burned per year. Whether the cause of these fires be from man-made or natural causes the damage they inflict on the environment, and the communities that they touch exact a very real cost in regards to the environment, economy, and in human lives. The burden of managing these wildfires is balanced between the states and the Federal government depending on the location of origin of the fire that needs to be responded to. The nature of wildfires is chaotic and very hard to predict, thus planning and allocating assets for a fire season is thus very hard to manage on the Federal and State level. Developing predictive methods, and utilizing all tools available to best estimate where to deploy assets for a fire season would bring a tremoundous advantage to managing the destruction of a given fire season.

This project really could have trained over any area known to have a high amount of wildfires to train it's model. However Northern California was chosen due to the authors XYZ

## Data Understanding

Our data understanding is broken down into two parts. Retrieving known instances of fire anomaly events in Northern California, and then using these known instances to query the Google Static API for relevant satellite imagery data corresponding to the location that a fire isntance was recorded.

To retrieve fire anomaly events we referenced the [NASA FIRMs archive data](https://firms.modaps.eosdis.nasa.gov/country/). This archive contains all recorded instances of fire anomalies over the planet earth, using the MODIS instrument system. The Moderate Resolution Imaging Spectroradiometer (MODIS) is a key instrument onboard the Earth Observing System (EOS) Terra and Aqua platforms, designed to monitor the Earth's atmosphere, ocean, and land surface with a set of visible, NIR, MIR, and thermal channels.

## Data Preparation

## Modeling

## Evaluation

## Next steps
