# Choose promotions by clustering based on customer's purchase behaviour
This is a data science project that explore Starbucks App simulated customer purchase behavior data. The goal of this project is to create a promotion offer recommendation engine for the users.

There is the link of the article published on Medium:  
https://medium.com/@ygsunshine86/what-promotion-to-send-to-your-customer-hear-what-data-has-to-say-26506f8d2b72

# Installation
Clone this repo: https://github.com/Ygsunshine007/Udacity_Starbucks_Capstone.git
I used python 3.6 to create this project and the libraries I used are:
1. Pandas
2. Numpy
3. Matplotlib
4. Scikit-learn
5. Math
6. Json
7. Time

# Problem Statement
I will examine the data set and combine transaction, demographic and offer data to determine which demographic groups respond best to which offer type. In the following section I aim to:

Analyse customer characteristics and demographics
What offer should be sent to each customer based on their demographics?

# File Descriptions
### Starbucks_Capstone_notebook.ipynb
This is the file that describes the steps I took in creating the recommendation engines. The code I used to solve the tasks I mentioned in the Project Motivation section is in this file.
### The .csv files
These are the files that was generated during the data cleaning phase.
1. master_df.csv - This is the initial combination of 3 data sets mentioned above.
2. df_use.csv - This is the data that contains initial variables and dummy variables
### The .json files
These files are located in the 'data' folder. The folder contains 3 files:
1. portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
2. profile.json - demographic data for each customer
3. transcript.json - records for transactions, offers received, offers viewed, and offers completed


## Data Cleaning
During the data cleaning phase, I conducted several tasks:
1. Divide channels column in portfolio data frame into 4 dummy columns: web, email, mobile, and social.
2. Create a dictionary with offer id as keys and aliases for the purpose of simplifying the merging of 3 data sets later.
3. Merging profile and transactions data frames.
4. Map the value column in the merged data frame into offer_id, transaction_amount, transaction, and reward_achieved.
5. Check whether transactions were caused by offers. If it were, I created a column called transaction_from_offer and assign 1, and another column called transaction_offer_viewed and assign the offer id. If it weren't, assign 0 to transaction_from_offer, and assign 'no transaction' to transaction_offer_viewed.
6. Classifying gender which contains None values by using Random Forest Classifier

## Project Metrics
I am planning to use an unsupervised machine learning model with K-Means is used to cluster the customers.

The number of clusters is choosed with SSE metrics :

The Silhouttee score)
The silhouette value is a measure of how similar an object is to its own cluster (cohesion) compared to other clusters (separation). The silhouette ranges from −1 to +1, where a high value indicates that the object is well matched to its own cluster and poorly matched to neighboring clusters.

The Inertia / Sum Square Error (SSE) value, can be recognized as a measure of how internally coherent clusters are. We seek to minimize the value.
After cluster is determined, different measurements can be used to determine which offer to be overweigh in each group. In practice, even if a group of customer show strong perferrence of a type offer, it is not a good practice to only send that offer and ignore other offers. Customer perferrence could change over the time and sometimes it is also important to influenece customer behaviors to acheive maximum results. The recommendation derived from the anlaysis can guide the markting group to decide which offer to overwieght in real case senarios, based on customer perferrence. Depends on the outcome of the analysis, I will most likely compare spend amount as an indicator of customer perference. I am using a simple assumption that the spend of the transaction shows the customer's perference of an offer.

With a focus on the average transaction amount, I will consider the following metrix

The average transaction amount. If the experiment group has higher average transaction amount, the recommendation engine can be considered useful. Percentage of offer completed. If the experiment group has higher percentage of offer completed, the recommendation engine can be considered useful. Ratio of transaction from offer and offer received. The higher it is for the experiment group, the better. Ratio of transaction not from offer and offer received. The lower it is for the experiment group, the better.

## Data Exploration / Visualization
The code I used to visualize the data can be found in the notebook. The results of the exploration are:
1. Most of the app users come from lower-middle class although Starbucks items are not cheap.
2. Users who joined on earlier years might have discontinued their memberships.
3. Users who joined on the year 2016 made the most average amount of transaction
4. Users who joined on the year 2017 made the most amount of transaction, although individually they don’t really spend a lot.
5. Female users tend to spend more to get the rewards offered.
6. Most of the app users are male, but they don’t really try to complete an offer should they view it.

## Clustering
I used standard scaler to scale the data. This is necessary because there are columns with really big values and those with just 0 and 1. In other words, there are big differences in variance for each feature that were used in clustering.

The method of choosing how many clusters (k) I used was the elbow method. After computing and plotting the SSE vs K, I found that I should use 4 clusters to segment the Starbucks App users.

Cluster 0 represents the customers that have highest income and make most frequent purchases. 

## Recommendation based on Clusters
The tasks I did are as follows:
1. I have to consider which users don't like offers. Therefore, I will create a dataframe containing the user_id and transaction not from offer ratio. If the ratio is greater than the mean value, I will not recommend anything to these users.
2. Create cluster-promotion matrix.
3. Check whether a user likes to be given promotion or not. If they don't like promotion, do not recommend anything.
3. If they like promotion, check which cluster a user belongs to.
4. Recommend top 3 offers which the cluster this user belongs to like, and get the list of these offer ids.  
5. If the user id is new, I will give a list of most popular offers.

## Recommendation based on user similarities
We look at average net spend per transaction as an indicator for customer's perference. From the table above we pick the maximum average net spend per each cluster, then we compare different methods to receive the offer and pick the highest percentage: 

Cluster 0: Discount Difficulty 10 reward 2 Web
Cluster 1: Bogo Difficulty 10 reward 10 Social
Cluster 2: Discount Difficulty 20 reward 5 Web
Cluster 3: Discount Difficulty 20 reward 5 Web


# Licensing, Author, Acknowledgements
I made this project to create recommendation engines for Starbucks Apps. If you can improve this, I will be glad to hear from you! Feel free to use / tweak this and mention me so I can take a look at your work.

As usual, I benefit greatly from Udacity, stackoverflow and sklearn documentations. Thank you all!
