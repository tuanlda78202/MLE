# Purpose of EDA

This EDA step gives us a first look at the data.
You need to have a certain feel for what you have in hand before any modelling strategies.
EDA helps you visualize the complexity of the problem and outlines the first steps to take.

Data exploration should stop the first time before feature building but should also be done throughout the development of the system.
After the features are built, you also need to do the EDA again to see if the processed data is _clean_.
In addition, after building and analyzing the model, we often need to return to EDA to continue to discover what is still hidden in the problem data. The deeper you understand the data, the sooner you will be able to interpret the model's behaviour and make appropriate changes.

```{Note}
To distinguish the data before and after the preprocessing step, the columns in the original data table are called FIELD
The columns that have been processed and ready for model training are called FEATURE.
```

## Data Size

First, you need to figure out how many samples the data has and its many fields.
If the data is too small, there is a high chance that you cannot use Deep Learning to solve it but need to use other methods.
Knowing about the data size also helps you determine the training data (_training data_), and the validation data (_validation data_) sizes and prepare the memory accordingly.

## Meaning of each data field

It would help if you were mentally prepared to work with data more than with models when working with tabular data.
Knowing the meaning of each data field helps you have the right ways of handling and characterizing.
The meaning of each data field is often included with the tuple, or sometimes you have to infer it yourself based on the values ​​in the column.
For example, if the values ​​in a column are "Thai Binh", "Nghe An", "Ben Tre", ... then there is a high chance that this column is the name of the province.

However, there are cases where the values ​​in each field have been encoded as meaningless for information security reasons.
It is essential to know what the information field means in these cases.
It directly affects the processing characteristics and quality of the model.
If you don't know the meaning and especially the data encoded as numbers, good luck because there is a high chance there will come a time when the model has an undesirable quality that you cannot explain.

## Data type of each field

Machine Learning models for tabular data are pretty sensitive to the data type.
In general, Machine Learning models receive processed data in numeric form.
One of the requirements of a good model is stability with minor changes in the input.
This means that if the model inputs are values ​​close to each other, the outputs are also expected to be close.
If a numerically encoded item data, such as a user number, you think is numeric, the model learns that the
Users with similar codes will have similar characteristics.
This is unlikely to be accurate, especially when the code is entered at random.

## Probability distribution of each field

We need to know the probability distribution of each data field to plan the data cleaning and generate features related to that data.
For each column of data, the following cases need to be kept in mind:

1. **All values ​​in the column are equal:** For example, there is a column called "Year" in the data, and all values ​​are similar to 2020.
As such, this column has no predictive significance. We can delete this column when cleaning data.

2. **There are too many missing values:** If the meaning of this column is not essential, we can delete it.
If it matters, you need the right strategies.

1. **Invalid value appeared:**
If the "Age" field contains negative or greater than 200, they are most likely invalid values.
We can either reassign invalid values to the nearest valid value or treat them as missing data.


4. **Exception value appears:**
Let's say the monthly income field contains most of the values ​​in the range of 1-100 million, but there are a few exceptions that earn up to 10 billion.
If the value of 10 billion is kept unchanged, the model seems to be forcefully trained, and it has to strain to predict those outliers, causing the quality of the shared values ​​to suffer. Also, if the data normalization step is to be performed to the $[0, 1]$ segment (a widespread technique when dealing with data), then most values ​​are in the range $[0, 0.01]$. These minimal and close values ​​can lead to the model failing to distinguish between different income levels.
That's an example with numeric data. With category data with many different categories, some items account for 99% of the total sample, while the entire selection of many other categories is only 1%. We need to have special handling with data of this type.


## Correlation between Data fields

We also need to calculate the correlation between the data fields, especially between the prediction label and the remaining areas, when doing EDA.
If the correlation coefficient between a column and the label column is zero, there is a high probability that the column does not yield much predictive value.
You can give preference to columns with more significant correlations.
Conversely, if the correlation coefficient between a column and the label column is too high, there are two possibilities:

* That column is likely to yield good prediction results. Then we need to focus on cleaning and building features related to this column first.

* Data may leak (_data leakage_). Let's say you need to predict a user's age and realize a column has a correlation coefficient of 1.
If it's the "Year of Birth" column, you don't need to do a Machine Learning model, just a subtraction.
However, the problem could be predicting age when the "Year of Birth" field is missing but knowing much other information.
We can't use the "Year of Birth" field in this case and need to remove it.

In addition, if two columns other than labels are highly correlated, we should also check their significance to see if a column can be omitted.

------

Answering these questions will significantly help with data cleaning and feature building later.

Depending on the amount of time you have and your knowledge of the data, you can further analyze the data. The more you understand the data to build the right features, your model will be better.

```{Tip}
You should not spend much time on EDA at the beginning of a project but should stop at the above basic assessments to find the data fields that have a high probability of delivering exemplary results and building specific characteristics—based on those fields.
```

You will have to go back to EDA many more times after you have built your first model.
We need to quickly build a complete pipeline for the problem, including data processing, model training, and quality evaluation. You do not need to focus too much on building a good model from the beginning but should pay more attention to a complete system to evaluate the model quality and find out where to improve. Based on those assessments, you can make inferences and test them with data. From there, make the appropriate adjustments.