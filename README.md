## Overview

The goal of this project was to leverage unsupervised machine learning to group traded
cryptocurrencies based on several defined features, and visualize the results. As a final step, 
the analysis created was then deployed to Amazon SageMaker.  To complete this project, the below
steps were followed. 

- Data Preprocessing 
- Dimension Reduction Using PCA
- Clustering Using K-Means Algorithm
- Data Visualization
- Deploy to SageMaker

### Data Preprocessing

To first retrieve the data needed, the provided CSV file was uploaded.  Data was also pulled from 
the provided json endpoint URL, but data access limitation issues arose while trying to leverage 
that data.  To start the preprocessing, the "Unnamed: 0" column was set to the index value of the 
dataframe.  This left the dataset with only the required columns needed for analysis.  For analysis 
purposes, only the cryptocurrencies currently being traded were wanted, so non-traded data was dropped. 
All NA values were dropped, and string replacement was used to drop blank cell rows.  Additionally, 
all rows were dropped that had 0 or blank data in the "TotalCoinsMined" column. Once all data was 
cleaned, data reduction was leveraged. 

### Dimension Reduction - PCA

Prior to reducing the data with PCA, dummy variables were created for non-numeric data.  The data
was then scaled as required, prior to PCA reduction.  Scaling the data is critical to help minimize
the risk of skewed results from outlier data.  The dataset was reduced to 3 dimensions with PCA and 
entered into a new dataframe for K-Means algorithm analysis. 

### Clustering with K-Means

Prior to running the K-Means algorithm an elbow curve was generated to find the best k-value.  Upon
analysis of the elbow curve generated I determined a k-value of 5 would be the most appropriate.  
This was due to the inflection point on the curve that most accurately represented where an "elbow" 
would be, and where we started to see less deterioration in inertia.  The K-Means algorithm model 
was then fit and executed on the new, PCA reduced dataframe.  Prior to plotting results for 
visualization, a new, combined dataframe was created (clustered_df), which joined the original 
dataframe with the PCA data dataframe.  

## Data Visualization

To start visualization of the K-Means output, a 3D graph was generated outlining the 5 clusters (k-value)
defined in the K-Means algorithm.  As can be seen from the graph, the clusters are pretty well defined
and mostly compact on the graph, aside for classes 2 and 3.  Based on analysis of the graph, most of the
TotalCoinsMined data closely matches the TotalCoinSupply data (shown when x and y are set to these variables).
There is a class 4 outlier that has a significantly larger coin supply vs. coins mined.  

A table visualization was also generated to show CoinName, Algorithm, ProofType, TotalCoinSupply, 
TotalCoinsMined, and Class.  This data can be sorted by each individual column name by clicking on 
on the column name (ascending and descending sort).  Lastly, a 2D scatter plot was generated to 
visualize TotalCoinsMined vs. TotalCoinSupply.  Prior to generating this plot I scaled the TotalCoinSupply
and TotalCoinsMined data to help with plotting.  Similar to the 3D plot previoulsy generated, it can
be seen that most of the TotalCoinsMined data is similar to the TotalCoinSupply data, causing the 
clustering to be mostly in the lower left corner of the plot.  There are a couple of outliers that
are noticed, one a class 4 and one a class 2 point.  The previous class 3 outlier mentioned from the 
3D plot is not apparent on the 2D plot.  

### Deploy To SageMaker

The file created for analysis and visualization was deployed to Amazon SageMaker as a final step.  
All plots had to be modified using Altair in order to visualize the plots on SageMaker.  The notebook
included titled, "crypto_clustering_sm" is the notebook modified on SageMaker.

## Final Analysis

During this analysis I found that a total of 533 cryptocurrencies are being traded.  Of the currencies
analyzed and plotted, most all follow the same pattern of equal TotalCoinsMined to TotalCoinSupply. 
This tells me that just about all currencies are being mined, seemingly on a regular basis.  
As for the clustering done via K-Means, the chosen k-vale of 5 seemed to fit the model well.
It captured the outlier datapoints and clustered them into their own class.  It is possible that a 
cluster amount (k-value) of 6 would have been a good option as well, as this likely would have captured the 
class 3 outlier seen on the 3D plot and classified it into its own cluster.  In the end, the analysis
done proves that the K-Means algorithm is effective for clustering and can be leveraged in multiple 
ways, for numerous types of analysis.   
