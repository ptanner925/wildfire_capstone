# Wildfire_Capstone
My final project for the Flatiron Data Science Boot Camp attempts to use a Convolutional Neural Network that scans satellite images from Northern California and is ultimately able to distinguish between areas that have experienced a wildfire event and those areas that have not experienced a wildfire event.

## Table of Contents

This home repository contains notebooks containing my exploratory data analysis, ways of retrieving satellite images, methods of model building, and conclusions as to the success of the overall project.

### Notebooks
[Jupyter Notebooks](Notebooks)

### Reports
[Visualizations](reports/visualizations)

### Data
[How to access data](https://github.com/ptanner925/wildfire_capstone/blob/main/Notebooks/satelitte_data_retrieval.ipynb)

### Modeling
[Model Building](https://github.com/ptanner925/wildfire_capstone/blob/main/Notebooks/Final_Notebook.ipynb)

### Readme

[Read Me](README.md)


## Business Understanding

According to a [United States Congressional Report](https://fas.org/sgp/crs/misc/IF10244.pdf), published in 2020, since the year 2000, there have been on average 71,300 wildfires in the United States per year, and also on average 6.9 million acres of land burned per year. Whether the cause of these fires be from man-made or natural causes the damage they inflict on the environment, and the communities that they touch exact a very real cost in regards to the environment, economy, and in human lives. The burden of managing these wildfires is balanced between the states and the Federal government depending on the location of origin of the fire that needs to be responded to. The nature of wildfires is chaotic and very hard to predict, thus planning and allocating assets for a fire season is difficult to manage on the Federal and State level. Developing predictive methods, and utilizing all tools available to best estimate where to deploy assets for a fire season would bring a tremoundous advantage to managing the destruction of a given fire season.

This project really could have trained over any area known to have a high amount of wildfires to train it's model. However Northern California was chosen because of the numerous wildfires that occur there every year, approximately 7400.


## Data Understanding

Our data understanding is broken down into two parts. Retrieving known instances of fire anomaly events in Northern California, and then using these known instances to query the Google Static API for relevant satellite imagery data corresponding to the location that a wildfire instance was recorded.

To retrieve fire anomaly events I referenced the [NASA FIRMs archive data](https://firms.modaps.eosdis.nasa.gov/country/). This archive contains all recorded instances of fire anomalies over the planet earth, using the MODIS instrument system. The Moderate Resolution Imaging Spectroradiometer (MODIS) is a key instrument onboard the Earth Observing System (EOS) Terra and Aqua platforms, designed to monitor the Earth's atmosphere, ocean, and land surface with a set of visible, NIR, MIR, and thermal channels. After selecting for the years between 2015 and 2019 for fire anomalies in the United States. The resulting datasets were then combined and cleaned to select for fire instances between latitudes of (38.0881 & 40.8336) and longitudes between (-123.1208 & -120.2933), comprising an area over northern California of approximately 70,000 km^2. Each fire instance comes with a confidence rating that determines the system's confidence that it has succesfully detected a fire instance. For my purposes I selected those fire instances within the given square area with a confidence level greater than or equal to 75%. 

To retrieve our non-fire instances, a function was written to randomly generate coordinates over the same square area. Wildfires are overwhelming small. So our chances of retrieving an image over an area with the same relative area are relatively low, but not zero. Duplicates were removed, also images captured over the ocean, or in the San Fransisco Bay Area were taken out of the dataset. This process gave me approximately 20,000 images to work with. The map below represents the 10,896 wildfire instances between the years of 2016 and 2019 that make up our fire class area images.

![recorded fire instances](/reports/visualizations/composite_fire_map_2015_to_2019_v2.png)

## Satellite Image Examples: 

![fire class](/reports/visualizations/fire_class_example.png)
![non fire class](/reports/visualizations/non_fire_class.png)



## Data Preparation

After retrieving the images from the google api the images were classified into fire and non-fire classes, meaning that the image had been identified as an area having experienced a recorded fire anomaly event and those that have not. Because we are running a CNN, a supervised machine learning method, labels need to be attributed to images to help reduce loss, and increase recall and accuracy. After this was done, each image was converted into a 3rd Order Tensor object, and divided by 255. The values of the RGB pixels range from 0-255. Dividing each pixel by 255 normalizes our color values to between 0-1, and helps our CNN algorithm converge faster.



## Modeling
The machine learning model used to distinguish between wildfire areas and non-fire areas was a Convolutional Neural Netowrk. Convolutional Neural Networks are a common approach when it comes to image classification. They work in two parts. The first part takes a hidden layer combined with feature extraction, which performs a series of convolution and pooling operations where relevant features of the image are detected. The second part is classification, in which the fully connected layers serve as a classifier on top of these extracted features. They assign a probability for the object on the image being what the algorithm predicts. In this case I used a Sigmoid activated neuron to make the binary classification: fire area or non-fire area.



## Evaluation


The model performed ok. The best model accuracy achieved was approximately 52%. The model was predicting that an area that had experienced a wildfire was just as likely to have not experienced a wildfire, and vice-versa. Here you can see in the confusion matrix below, in which 0 represents wildfire areas, and 1 represents non-fire areas, that we have an almost perfect balance between false positives to true positives. 

![confusion_matrix](/reports/visualizations/confusion_matrix_test_best.png)


As I attempted to make the model more and more complex, the model started to classify most images as non-wild fire areas, most likely due to overfitting and overlapping features between the two types of areas. Eventually at some point 50% accuracy was the best intended target I could hope for. As you can see in the two graphs below, even in the best model's performance, the metrics of accuracy and validation loss remain relatively stagnant. Other more advanced models that ran for longer epochs and had more finely tuned hyperparameters saw a degredation in accuracy, and soaring rates of validation loss. Neither situation is ideal.

### MVP model metrics

![mvp_model](/reports/visualizations/model_eval.png) 

### More complex model metrics
![Complex model](/reports/visualizations/complex_model_eval.png)

I have several theories about why the model performed the way it did. The first is that there was not quite as varied a range of features for the model to pick up on as I had hoped. One image of a forest in Northern California, kinda looks like the other. For example lets take another look at the Google Maps Satellite Images so we can discuss why it might be so difficult for our model to extract features in order to make a more accurate classification.

![fire_class_2](/reports/visualizations/fire_class_2_v2.png) ![non_fire_2](/reports/visualizations/non_fire_class_2.png)

First of all the Google Static Maps API, does not provide historical images of the day of a fire event. Rather all images retrieved are the most recently taken image of the queried area, it's likely that the images you see here were taken on the same day. Furthermore, besides the subtle scorch marks we see in the image on our left, our fire class image, we ourselves can hardly detect a difference between the two. A convolutional network doesn't "know" the difference between scorched earth, and dirt, all it detects is a value that to us would appear as the color brown.

My original intention was to use the NASA Earth API to retrieve historical satellite images to retrieve photos of an area the day of the fire but this too was problematic. While you can request historic images, the nature of satellites is they can only be at one place at one time, so if the area and date of the request made for an image does not coincide with a satellite orbit, you get the next image taken closest to the date queried. Even so, if you do manage to get an image from the day of a fire, and get a good luck at conditions and features from that day, you run into similar problems as I did with the Google Static Maps API. To demonstrate this I have provided a NASA satellite below using the NASA Earth API.


### NASA Example Image
![NASA Example](/reports/visualizations/nasa_image_example.png)

Can you tell the difference between a snow capped mountain range, smoke, and a cloud? Because this image contains all three. And most likely to a convolutional neural network, these amorphous random white blobs are most likely all the same. The problem here really lies in using raw satellite imaging data. There are better and more constructive methods.

## Next steps
Moving forward I would like to replicate a model developed by a joint [Google & Stanford](https://arxiv.org/pdf/2010.07445v1.pdf) study that took aggregated satellite photos from a wildfire area leading up to and including the day of fire ignition. The aggregated satellite photos are a collection of different features of an environment measured by various satellites. These featurs include: elevation, drought, vegetation index, and temperature. They later combined these images into a single tensor. 

![Google/Standford study](/reports/visualizations/google_study_1.png)

They then fed this object into a type of Convolutional Neural Network called a U-NET, and used this to predict how large a wildfire event would be in terms of area burned.

![Google/Standford study 2](/reports/visualizations/google_study_2.png)

Building models such as this may be more helpful to state and federal authorities in regards to planning for a fire season. They allow you to determine which fires to let burn and which ones they should target. It should be said that wildfires, while mostly man-made, are also apart of the natural process and typically only 1% turn into the large scale events we see on the news. Finding the right balance may be key to combating the long term health of the United States' forests and preventing the large scale fire events that we see today.

Thank you for joining me. I look forward to tackling this problem again with my hard won insights and welcome any feedback or collabarative interests you may have on this subject.