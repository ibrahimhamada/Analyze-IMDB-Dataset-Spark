# Analyze-IMDB-Dataset-Spark
Project of the Big Data Analytics Course Offered in Fall 2022 @ Zewail City

In this project, we are analyzing the IMDB reviews dataset. Moreover, we will be applying some Machine Learning models and sentiment analysis on reviews to predict some of the labels of the datasets such as: rating, and spoiler alert.



## Data Collection:

The dataset is obtained from [IMDb Review Dataset - ebD | Kaggle](https://www.kaggle.com/datasets/ebiswas/imdb-review-dataset?select=sample.json). It is collected from IMDB with total records of 5,571,499 reviews for 453,528 total shows. The reviews are written by 1,699,310 users. 

The data is split into 6 json files each of which has the following elements:

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/c55bfd39-f492-47e0-af87-04a3c4c0523e)


We pulled data from Kaggle using Kaggle API, but AWS did not read the Kaggle CLI despite being put in the specified directory by Kaggle documentation. We overcome this issue by using Kaggle API inside the python environment.
On pulling the data, we made sure that we were on the EC2-user side and then pushed the data to the HDFS. 
In addition, we kept buck-up data in a separate S3. So that whenever the cluster terminates or fails, we will not have to bring it all again from Kaggle.
But right before pushing to the HDFS, we kept slicing the data so that when the data gets loaded on an RDD it won't fail. Doing slicing, we focused on maximizing the size of data that can be held in RDD, we finally agreed on using 100 mega per RDD as a way to parallelize the data while still keeping each RDD as an acceptable amount of data.


## Data Analysis:
We have defined multiple requirements to do on the data and since the data contains tv shows and movies, each requirement we made was being done on the tv shows alone and on the movies alone. 
The first step in most requirements is to filter out the nans present in the rating section when it was being used. 
Since it is one of the important sections it was being used in nearly all the requirements. To be able to do the requirements better, we
defined a set of helpful functions:


   1) extract_year_Movie():

The purpose of this function was to extract the year in the title of the movie using regex expressions and if it fails to extract the years in cases like the title provided is a title for a tv show, it would return None.

   2) extract_text_Movie():

The purpose of this function was to extract the text in the title of the movie using regex expressions and if it fails to extract the text in cases like the title provided is a title for a tv show, it would return None. So in summary, it used to identify the title of a movie.

   3) extract_years_Tv():

The purpose of this function was to extract the year in the title of a Tv Show using regex expressions and if it fails to extract the years in cases like the title provided is a title for a movie , it would return None. So in summary, it used to identify the year of a Tv Show.

   4) extract_text_Tv():

The purpose of this function was to extract the text in the title of the Tv Show using regex expressions and if it fails to extract the text in cases like the title provided is a title for a movie, it would return None. So in summary, it used to identify the title of a Tv Show




## Data Analysis Requirements:

After that, we started dealing with ranked_matches rdd to find the requirements as follows:

### I) Requirement 1:  (Lowest Rated Shows and Highest Rated Shows )

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/edd70c50-5da3-4cc6-a2d6-3bdac8b9e866)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/e3796842-77eb-4b75-a173-81e5267a0351)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/2159f168-a63b-43db-8a62-dcf6bf1ac17e)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/2266aa06-86a2-49e5-b0c0-dec78f260bde)



### II) Requirement 2:  (Number of Spoiled Reviews)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/0d4d1f95-06c1-4af8-9756-21760302fae6)

 
   
### III) Requirement 3:  (Number of Helpful Reviews for shows)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/bc94f0fe-a592-4578-bf0b-1855ea27e7f6)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/4cec8b9f-0394-4c16-a914-9ce9c3e34595)



### IV) Requirement 4: (Number of reviews for shows)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/d63d87d4-201f-4242-8c5d-fe79a3d9edb6)
 
![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/293772fe-d2e7-4b68-bacb-3b1c31d5ccd2)

 
### V) Requirement 5:  (The Highest Rated Show in each year)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/1c98e47e-4ee2-4445-bcde-b05b3eadddc4)



   
### VI) Requirement 6: (Years and number of movies and tv shows released)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/d374c7d3-c5f4-468f-8b2b-8dad75c427eb)

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/cbbebee3-16aa-46e3-9661-5dea2250f11d)


## Machine Learning Models:

In this part, we used SparkML to apply some ML model on the dataset in order to predict some important information. 
We firstly chose the columns of interest to be predicted after training the model on the given dataset. 
As shown above, the dataset has 2 interesting features: Rating, and Spoiler_tag. We decided to set the previously mentioned columns as the output of the model. 

We used the Logistic Regression Model, the Naive Bayes Model, and the Support Vector Classifier (SVC) to predict the spoiler_tag given a particular input review. Accordingly, we tried to perform sentiment analysis by using a set of labeled data on movie/series reviews.

We got the following results which indicate the edge of SVM on other Models in classifying the sentiment analysis of the review, therefore it would be the most suitable model option for us.

![image](https://github.com/ibrahimhamada/Analyze-IMDB-Dataset-Spark/assets/58476343/476597ac-d8b2-4a4a-89d9-def8b913f297)

