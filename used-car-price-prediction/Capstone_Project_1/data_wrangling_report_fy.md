## [Used Cars Dataset Vehicles listings from Craigslist.org](https://www.kaggle.com/austinreese/craigslist-carstrucks-data) Data Wrangling 

## What kind of cleaning steps did you perform?

**1. Import the necessary libraries**  
_Import numpy as np_  
  _Import pandas as pd_
  _Import matplotlib as plt_  
  _Import seaborn as sns_

**2. Load data**  
_pd.read\_csv_

**3. Check the information**  
I got a general idea of the data set by _fd.shape_,  _Df.head()_, _df.info()_ to know the size of the data set, the numbers of columns and so forth.

**4. Drop the unnecessary columns**  
This file contains 25 columns,  I only kept the useful ones. The purpose of this step is to figure out the columns that are useful for further analysis, as well as to make the data frame easy to run and read.
_df=df.drop(columns=drop\_columns)_

**5. Deal with missing data**  

  * Firstly  I need to figure out how many null values in each column. For those columns that do not have any values at all, I could delete them directly.  
_df.isnull().sum()_

  * Second step I clarified the rest columns and fill the missing values with any value we choose, and this is the better technique to remove missing values from the data set.  

  * For the columns which are numeric I replaced the Null by their mean number.
For 'manufacturer', 'model',  'paint_color' I replaced the null by 'unknown' 
For 'year' I replaced the null by mode number.
  * Use _df.isnull().sum()_ to make sure all the missing data are filled up.

**6. Drop the duplicated data**  
Each row has a specific id, so I check the duplicated ones according to their id.
_clean_data.duplicated()_  
_clean_data=cleaned_df.drop_duplicates(['id'])_

**7. Drop the fake data**  
Based on common sense, I found that some of the data are not real. For example, maximum of price in the data set is  3600028900, which is obviously a fake data. By observing the scatter chart, I set a gross range for data set, for example, the 'price' should be from  0 to 20000, and the 'odometer' should be 0 to 40000.

_df1=df[df['price']<200000]_
clean_data=df1[df1['odometer']<400000]
And replace the 'o' by median number.  
_clean_data['price']=clean_data['price'].replace(0,clean_data['price'].median())_

**8. Transforming Data Using Function**  
In order to make the kilometer data more intuitive, I divided the 'odometer' into three classes, namely 'low odometer', 'medium odometer' and 'high odometer'. The purpose of this step is to prepare for the comparison box chart which shows the relationship of price and odometer.

**9. Detecting & Filtering Outliers**  
A large number of outliers can be found in the box chart from last step. I used **IQR rule** and the **box chart** to detect and extract the outliers from the data set.

## 2. How did you deal with missing values, if any?  
The fast way to deal with the missing data is to drop the rows with missing values  by _using dropna()_. But I don't think it is the best method. I prefer to sort the columns firstly, and then fill the missing values with any value we choose as appropriate.

## 3. Were there outliers, and how did you handle them?
I tend to use Box Plot to detect Outliers. A typical Box Plot is calculated based on the following five values:   
A. The minimum value of a group of samples  
B. The maximum value of a set of samples  
C. Median of a group of samples  
D. Lower Quartile (Lower Quartile/Q1)  
E. Upper Quartile (Upper Quartile/Q3)

Here is the steps:

1. get the **IQR** by _scipy.stats_  
2. get the **Q1** and **Q3** by _.describe()_

_price\_stats=clean\_data['price'].describe()_  
_Q3=price\_stats['75%']_  
_upper\_bound=Q3+(iqr*1.5)_  
_Q1=price\_stats['25%']_  
_lower\_bound=Q1-(iqr*1.5)_  
_print(upper\_bound)_  
_print(lower\_bound)_

3. Append the outliers in a list  
   _outlier\_above=[n for n in clean\_data['price'].values if n>upper\_bound]_  

4. delete the outliers from the data set  
_clean\_data=clean\_data[~clean\_data['price'].isin(outlier\_above)]_