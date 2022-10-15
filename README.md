# Patterns in Road Accidents: The case of United Kingdom
![london_congested.jpeg](images/london_congested.jpeg)
### Index:

* [Abstract](#section1)
* [Data](#section2)
* [Exploratory Data Analysis (EDA)](#section3)
* [Machine Learning to Predict Accident Severity](#section4)
* [Validation Metrics](#section5)
* [Accident hotspots in the UK](#section6)
* [Tools](#section7)
* [References](#section8)

<a id='section1'></a>
### Abstract
World Health Organization declares road traffic injuries as the leading cause of death for children and young adults (2022). Furthermore, millions of people are affected by traffic accidents every year. This project is designed to support the discussions about the road safety in the UK by providing a comprehensive view on the spatial, temporal, and demographic patterns in road accidents. Among the results of this project, I present the junctions and interregional roads that had been the accident hotspots in Great Britain between the years 2016 and 2020. Furthermore, I provide a machine learning model predicting the severity of hypothetical accidents based on factors such as the age and sex of driver, vehicle type, latitude and longitude, day of the week, light conditions etc.

<a id='section2'></a>
#### Data
For this project, I downloaded the [road safety datasets](https://www.data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data) for 2016-2020 which was published by the UK Department for Transport. The three datasets that are used in the project provide information on i) all registered accidents, with details on ii) the vehicles and iii) the casualties involved. The data is in encoded form rather than being textual strings and the lookup table is available from the same website.

The shapes of the datasets are

i)	accident dataset: 597973 rows × 37 columns, 
ii)	vehicle dataset: 1101591 rows × 27 columns, 
iii)	casualty dataset: 781716 rows × 18 columns.

I combined the datasets using the accident index and the vehicle reference. Prior to any data wrangling, the merged dataset had the shape of 781716 rows × 80 columns.

<a id='section3'></a>
### Exploratory Data Analysis (EDA)
Some portion of this project was dedicated to understand whether accident severity may be considered as a function of certain conditions. In the dataset, accident severity is described in three classes: Slight, serious, and fatal. Exploratory data analysis reveals that the presence of some of the conditions may be playing a role in the severity of an accident. In fact, the investigation of the i) first point of impact, ii) light conditions, iii) weather conditions, and iv) casualty type parameters show that: 

•	The likelihood of an accident being deathly is higher if the accidental hit is received from the front of the vehicle.
•	Most accidents take place in daylight and good weather. 
•	The proportion of fatal accidents that take place in dark over daylight is higher in comparison with the respective proportions in the slightly severe and seriously severe cases. 
•	The relative proportions of different weather conditions hardly vary between the each class of accident severity. This may point out that accident severity may be independent of the weather conditions
•	Overall, 50% of all the casualties in road accidents are car occupants. However, the largest two classes of casualties are the pedestrians and cyclists in large cities, for example in London. In fact, car occupants make up only the 5% of the casualty population in London.

<a id='section4'></a>
### Machine Learning to Predict Accident Severity
I built both a logistic regression and a KNN classification model to predict severity of hypothetical accidents. Both models predict whether a hypothetical accident would be classified as a slightly severe or a seriously severe accident based on information about 38 features. These features are listed as follows (not in the order of importance):

i.	Driver-related features: age of driver, sex of driver, driver home area type, driver imd decile, journey purpose of driver.
ii.	Vehicle-related features: age of vehicle, vehicle type, vehicle brand, engine capacity cc, propulsion code, towing and articulation, vehicle left hand drive.
iii.	Spatio-temporal features: easting, northing, vehicle direction from, vehicle direction to, month, day of week, hour.
iv.	Road-related features: road type, first road class, second road class, speed limit, junction location, junction detail, junction control, road surface conditions, special conditions at site, carriageway hazards, urban or rural area, trunk road flag, vehicle location restricted lane, pedestrian crossing human control, pedestrian crossing physical facilities.
v.	Other features: first point of impact, vehicle manoeuvre, light conditions, weather conditions

The original classes in the accident severity (slight, serious, and fatal) were reduced to two classes by grouping the serious and fatal together (as ‘serious’). As a matter of fact, both the serious and fatal accidents would need similar level of attention. This decision was also based on the fact that the three classes in the target had very different sizes. Especially, the size of the ‘fatal’ accidents is substantially smaller than the slightly severe accidents. It is known that the logistic regression in the case of imbalanced target class sizes favor the major class if the imbalance in the training dataset is not treated. Combining these classes reduced this imbalance but not entirely. The training dataset was treated for imbalance using i) SMOTE, ii) undersampling, and iii) oversampling separately. The logistic regression classification model that was trained with the oversampled dataset was found to give the best result.

The model was trained and tested using the data from the year 2017. The model is validated twice, using the data from 2016 and 2020 separately. Both the test and validation were carried out both for the logistic regression and KNN classification and it is found that logistic regression classification performs better than the KNN classifier in this specific problem.

<a id='section5'></a>
### Validation Metrics
The accuracy of the initial logistic regression model (before treatment with oversampling) is 80%, which may not seem too bad at first sight. The confusion matrix, however, tells a different story. Out of 5111 seriously severe accidents, the model was able to predict 215 true positives only. This means that the initial model fails in predicting the seriously-severe accidents, which is too expensive for the case of the road accidents. Evidently, accuracy is not a suitable evaluation metric for this model. In the case of the accident severity predictions, recall and confusion matrix will be preferred as the model evaluation metrics.

It is notable that all of the imbalance treatment techniques (SMOTE, oversampling, and undersampling) increased the recall at the cost of lower precision, which is actually preffered in the case of road accidents. In other words, I would prefer this model to predict a potentially 'slightly-severe' accident as 'seriously-severe', rather than a 'seriously-severe' potential accident as 'slightly-severe'. False positives are preferred over false negatives here. The final logistic regression model which was trained with the oversampled dataset was able to predict 3455 seriously severe accidents in the test set which included 5111 seriously severe accidents.

Finally, drivers' attention to the road and how responsible both the drivers and the pedestrians behave in traffic may be among many of the non-quantifiable features that are affecting the occurrence and severity of an accident. These are not incorporated into this model.

<a id='section6'></a>
### Accident hotspots in the UK

Based on the data covering a 5-year period between the years 2016 and 2020, there are 24 junctions in the UK which are hotspots for seriously severe accidents. It is notable that almost all of them are 'Give way or uncontrolled' type of junctions. A preventative action proposal may be installing traffic lights to these junctions. In addition to that, presence of traffic police at these junctions from time to time may help control the number of accidents in these junctions.

![junctions.png](images/junctions.png)

This study also identfied the top 10 interregional roads (A-Roads) with the highest number of accidents. The majority of them are in England, one is partially in Scotland and one is in the Wales. Higher population density of England in comparison with the Scotland and the Wales may be playing a role in occurence of more accidents in England.

![A-roads.png](images/A-roads.png)

<a id='section7'></a>
### Tools
Jupyter notebook 6.4.8.
Numpy, pandas, sklearn
Seaborn, matplotlib
MySql
Tableau

<a id='section8'></a>
### References

[World Health Organization](https://www.who.int/news-room/fact-sheets/detail/road-traffic-injuries#:~:text=Road%20traffic%20injuries%20are%20the,pedestrians%2C%20cyclists%2C%20and%20motorcyclists)

[Tablaeu file: UK_Road_Safety](https://public.tableau.com/views/UK_Road_Safety/Numberofaccidentsperyearseverity?:language=en-US&:display_count=n&:origin=viz_share_link)

[Tablaeu file: UK_accident_hotspot_junctions](https://public.tableau.com/views/UK_accident_hotspot_junctions/Junctionsthathostedseriouslysevereaccidentswith100andmorecasualtiesoverthe5yearperiod2016-2020_24junctionsoverall_?:language=en-US&:display_count=n&:origin=viz_share_link)

[UK Road Safety Data](https://www.data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data)

[Daily Life In London](https://www.gettyimages.com/detail/news-photo/cars-and-buses-queue-in-a-long-traffic-jam-on-knightsbridge-news-photo/1229230414?adppopup=true)

