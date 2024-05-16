# Task7 
## Data Visualization
### 1)
``` python
plt.subplots(figsize=(12, 8))
sns.heatmap(train_df.corr(), annot=True)
```
this heatmap code that visulizes the correlation between the numerical features 

the  insights we found :

1- there is strong direct correlation between price (target coulmn) and carat,x,y,z
2- there is inverse relation between the categorical features and the price
### 2)
``` python
train_df.iloc[:, :].hist(figsize=(20, 15), bins=50)
```
this histogram code that visulizes the distribution of data

the insights we found :

1- there are right skew in almost numerical features and this leads us to think of the outliers and if there are outliers especially in price column
2- the ranges of data varies that leads us to say that the data needs to be scaled
### 3)
``` python
fig, axes = plt.subplots(2, 3, figsize=(16, 14))
for i, cols in enumerate(["carat","depth", "table", "x", "y", "z"], 1):
    sns.boxplot(data=train_df, x=cols)
    plt.subplot(2, 3, i)

plt.show()
```
this boxplot code used to  display the distribution of data based on a five-number summary: minimum, first quartile (Q1), median (Q2), third quartile (Q3), and maximum. It is useful for identifying outliers and understanding the spread and skewness of the data.

the insights we found :

1- there are alot of outliers in the data espacially in carat,depth and price
### 4)
we want to detect the outliers using the interquartile range (IQR)
``` python
def get_outliers(col, train_df = train_df):
    Q1 = train_df[col].quantile(0.25)
    Q3 = train_df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    iqr_outliers = train_df[(train_df[col] < lower_bound) | (train_df[col] > upper_bound)]
    
    return lower_bound, upper_bound,len(iqr_outliers)


for col in train_df.columns[:7]:
    lower_bound, upper_bound, outliers = get_outliers(col)
    print(f"Number of outliers detected based on IQR method ({str(col).center(8)}) : ", str(outliers).rjust(4), f"Lower = {str(round(lower_bound, 3)).rjust(10)}", f"Upper = {str(round(upper_bound, 2)).rjust(10)}")        
```
This code is designed to:
1) Detect Outliers: Identify potential outliers in the first seven columns of the DataFrame using the IQR method.
2) Summarize Outliers: Print a summary of the outlier detection results for each of these columns, including the count of outliers and the calculated lower and upper bounds for identifying outliers.

the insights we found :


1) As we thought before there are alot of outliers
2) in price column there are  2844 outliers
3) in depth column there are  2182 outliers
4) in carat column there are  1504 outliers
### WE Decide to handle the outliers
### 5)
``` python
sns.scatterplot(x=train_df["carat"], y=train_df["price"])        
```
scatter plot visualizes the correlation between two numerical values

the insights we found :


1) there are noticed outliers in carat after value 3
### 6)
``` python
print(f"Carat Outliers : {len(train_df[train_df['carat'] > 3])}") # Visualization
train_df = train_df[train_df["carat"] <= 3 ] # 3
```
this code gives the number of rows that have values of carat more than 3
We found there are 21 rows So,we drop it as it is a small value to drop (It wont affect)
### 7)
``` python
sns.scatterplot(x=train_df["depth"], y=train_df["price"])      
```
scatter plot visualizes the correlation between two numerical values

the insights we found :


1) there are noticed outliers in depth  before the value 50 and after value 75 
### 8)
``` python
print(f"Depth Outliers : {len(train_df[(train_df['depth'] < 50) | (train_df['depth'] > 75)])}") # Visualization
train_df = train_df[(train_df["depth"] >= 50) & (train_df["depth"] <= 75)] # Visulization
```
this code gives the number of rows that have values of carat more than 75 and less than 50
We found there are 14 rows So,we drop it as it is a small value to drop (It wont affect)
### 9)
``` python
sns.scatterplot(x=train_df["table"], y=train_df["price"])     
```
scatter plot visualizes the correlation between two numerical values

the insights we found :


1) there are noticed outliers in table  before the value 50 and after value 70 
### 10)
``` python
print(f"Table Outliers : {len(train_df[(train_df['table'] < 50) | (train_df['table'] > 70)])}") # Visualization
train_df = train_df[(train_df["table"] >= 50) & (train_df["table"] <= 70)] # Visulization
```
this code gives the number of rows that have values of carat more than 70 and less than 50
We found there are 9 rows So,we drop it as it is a small value to drop (It wont affect)
### 11)
``` python
sns.scatterplot(data=train_df, x="x", y="price")  
```
scatter plot visualizes the correlation between two numerical values

the insights we found :

1) there are noticed outliers in x  after value 9 
### 12)
``` python
print(f"X Outliers : {len(train_df[train_df['x'] > 9])}") # Visualization
train_df = train_df[train_df["x"] <= 9]
```
this code gives the number of rows that have values of x more than 9
We found there are 10 rows So,we drop it as it is a small value to drop (It wont affect)
### 13)
``` python
sns.scatterplot(data=train_df, x="y", y="price")
```
scatter plot visualizes the correlation between two numerical values

the insights we found :

1) there are noticed outliers in y  after value 10
### 14)
``` python
print(f"Y Outliers : {len(train_df[train_df['y'] > 10])}") # Visualization
train_df = train_df[train_df["x"] <= 9]
```
this code gives the number of rows that have values of y more than 10
We found there are 0 rows So,we drop it to make sure that there is not
### 15)
``` python
sns.scatterplot(data=train_df, x="z", y="price")
```
scatter plot visualizes the correlation between two numerical values

the insights we found :

1) there are noticed outliers in z  after value 10
### 16)
``` python
print(f"Z Outliers : {len(train_df[(train_df['z'] < 2) | (train_df['z'] > 7)])}") # Visualization
train_df = train_df[(train_df["z"] >= 2) & (train_df["z"] <= 7)]
```
this code gives the number of rows that have values of z more than 7 and less than 2
We found there are 0 rows So,we drop it to make sure that there is not

### 17)
``` python
train_df["price"].hist(bins=50)
```
this code gives the number of rows that have values of x more than 7 and less than 2
We found there are 0 rows So,we drop it to make sure that there is not
### 18)
``` python
``` python
mean_price_by_cut = train_df_copy.groupby("cut")["price"].mean().reindex(cut_column_unique_ordered)

# Create subplots
fig, axes = plt.subplots(1, 2, figsize=(14, 6))

sns.countplot(data=train_df_copy, x="cut", order=cut_column_unique_ordered, ax=axes[0], palette='rocket')
axes[0].set_title("Count of Diamonds by Cut")
axes[0].set_xlabel("Cut")
axes[0].set_ylabel("Count")

sns.barplot(x=mean_price_by_cut.index, y=mean_price_by_cut.values, ax=axes[1], palette='rocket')
axes[1].set_title("Mean Price of Diamonds by Cut")
axes[1].set_xlabel("Cut")
axes[1].set_ylabel("Mean Price")

# # Adjust layout
# plt.tight_layout()

# Show plot
plt.show()
```
This code will generate two side-by-side plots: one showing the count of diamonds for each cut category and another showing the mean price of diamonds for each cut category.
We found there are 0 rows So,we drop it to make sure that there is not

####the insights we found :


1) cut (J) that have the smallest count has the largest price average
### 19)
We do so for color and clarity and found that the price dosn't depend on ctegorical features (as there are inverse relation between them and the price )
### 20)

##### Feature engineering

``` python
train_df_copy["vol"] = train_df_copy["x"] * train_df_copy["y"] * train_df_copy["z"] # For Categorical Visualization
# train_df_copy.drop(["x", "y", "z"], axis=1, inplace=True)

train_df["vol"] = train_df["x"] * train_df["y"] * train_df["z"]                     # For Training
train_df.drop(["x", "y", "z"], axis=1, inplace=True)

test_df["vol"] = test_df["x"] * test_df["y"] * test_df["z"]                         # For Testing
test_df.drop(["x", "y", "z"], axis=1, inplace=True)
```
we made volume column in both tain and test dataframe and drop the x,y and z
this reduces the dimensionality as we drop 3 columns and replaces it with one column

### 21)
``` python
mean_vol_by_cut     = train_df_copy.groupby("cut")["vol"].mean().reindex(cut_column_unique_ordered)
mean_vol_by_color   = train_df_copy.groupby("color")["vol"].mean().reindex(color_column_unique_ordered)
mean_vol_by_clarity = train_df_copy.groupby("clarity")["vol"].mean().reindex(clarity_column_unique_ordered)

fig, axes = plt.subplots(1, 3, figsize=(18, 6))

# Mean volume by cut
sns.barplot(x=mean_vol_by_cut.index, y=mean_vol_by_cut.values, palette='rocket', ax=axes[0])
axes[0].set_title("Mean Volume of Diamonds by Cut")
axes[0].set_xlabel("Cut")
axes[0].set_ylabel("Mean Volume")

# Mean volume by color
sns.barplot(x=mean_vol_by_color.index, y=mean_vol_by_color.values, palette='rocket', ax=axes[1])
axes[1].set_title("Mean Volume of Diamonds by Color")
axes[1].set_xlabel("Color")
axes[1].set_ylabel("Mean Volume")

# Mean volume by clarity
sns.barplot(x=mean_vol_by_clarity.index, y=mean_vol_by_clarity.values, palette='rocket', ax=axes[2])
axes[2].set_title("Mean Volume of Diamonds by Clarity")
axes[2].set_xlabel("Clarity")
axes[2].set_ylabel("Mean Volume")

## Adjust layout
# plt.tight_layout()

# Show plot
plt.show()
```
This code will generate three side-by-side bar plots showing the mean volume of diamonds grouped by cut, color, and clarity, respectively.

####the insights we found :


1) as the volume of the diamond increases as the price increase
2) the category with highest volume has highest price
3) 









