# Ecommerce-Data-Cleaning-and-Visualisation-in-Pandas
This readme file provides a step-by-step description of the data cleaning and EDA project using **Pandas**.

## 1. Downloading [the dataset from Kaggle](https://www.kaggle.com/datasets/nynkeugerard/dirty-ecommerce-data-eda-r)
This dataset contains a large amount of data that was intentionally generated with many errors for practicing data cleaning..

## 2. Importing the necessary libraries and the dataset.

![image](https://github.com/user-attachments/assets/e6c25343-6edf-4c31-956d-d7565ec172e5)

After importing all the necessary modules, we can see the table.

![image](https://github.com/user-attachments/assets/12b6fad6-e6c4-4c07-8563-c7f3b5b8e3d9)

Let's first take a quick look at the summary of the table.

![image](https://github.com/user-attachments/assets/fec5e598-0c1c-4726-948a-c34187b13431)

It immediately stands out that there are columns with the float data type, although they should be of type int, as they contain whole numbers. This will be fixed in the following steps.

## 3. Data cleaning
### 1) Removing duplicates
First, we remove all identical rows.

![image](https://github.com/user-attachments/assets/457dd32d-ef69-44cc-bfb7-902345025dbf)

After removing the duplicates, we can see that the number of records has decreased by 500.

![image](https://github.com/user-attachments/assets/7c0de272-3f01-4042-b3c4-e9606a76f070)

Since there were completely identical rows, duplicates were also present among the OrderID values, which should have been unique.

### 2) Unique OrderID
Let's check the number of unique OrderIDs.

![image](https://github.com/user-attachments/assets/69a08dec-aac6-48f6-a85e-dfe8f66e88b5)

There are a total of 9500 orders, but only 6123 unique OrderIDs, which violates the uniqueness condition. Therefore, we need to create a new OrderID column where all values will be unique.

![image](https://github.com/user-attachments/assets/798996b1-d53a-46f8-818a-c24e08a1b4cc)

An additional DataFrame, df2, was created to avoid accidentally losing the existing data. After that, a new column, NewOrderID, was created in it, filled with unique values starting from 100,000.

![image](https://github.com/user-attachments/assets/4fff20bd-2e97-459d-9126-6c9c428c5491)

We replace the values in the OrderID column with the values from the NewOrderID column, and then delete the latter. While checking for uniqueness, we see that all identifiers are now unique.

### 3) Converting the data to the correct types.

![image](https://github.com/user-attachments/assets/34bbd4d0-dd98-449d-bb83-0880d372b701)

It is noticeable that many columns contain data of type object and float, but for many of these columns, these data types are not suitable, so we will replace them with appropriate ones.

#### a) Datetime

First, we will convert the time-related data into the Datetime format.

![image](https://github.com/user-attachments/assets/afce5b7a-a8b3-4d23-8fe7-688b0a7b75ad)

#### b) Category

Let's review the unique values in the columns where there should be few distinct values.

![image](https://github.com/user-attachments/assets/cd704c61-1b09-4419-a306-ca95f5efacca)
![image](https://github.com/user-attachments/assets/0f939d92-9b89-4833-98ea-2fd231cc7ada)

These columns should be converted to the Category type, as this type is suitable for data with few unique values.

![image](https://github.com/user-attachments/assets/1c60da27-7fd9-44a3-9fdd-6b507a89a382)

#### c) Integer

We will convert the data representing whole numbers to the Int64 type.

![image](https://github.com/user-attachments/assets/420d6f3f-56b8-4384-8c41-58d6c7483685)

#### d) String

For the postal code, it is better to use a string type, as postal codes can contain leading zeros. However, in our case, the postal codes are not informative, as they most often do not correspond to the city they are marked with. Therefore, it is better to remove this column.

![image](https://github.com/user-attachments/assets/54d006ba-2370-4ea9-b984-04969b1dc77f)

#### Correct data types

Now all the data has been converted to the correct data types.

![image](https://github.com/user-attachments/assets/3beae764-793b-4c98-9251-16fa280b9330)

It can be noticed that there are both int64 and Int64 data types. The difference between them is that Int64 allows for missing values, while int64 does not. This is important in our case because the primary key cannot have missing values.

### 4) Location

Let's take a look at the columns that correspond to location.

![image](https://github.com/user-attachments/assets/1e552d95-987d-42f6-bce1-4f293c842bab)

As we can see, the values do not match between themselves, so they need to be corrected to ensure consistency.

#### a) Removing unnecessary data

I noticed that the region column contains only cardinal directions, and this data doesn't make sense, as it's unclear what they are associated with. Therefore, this column should also be removed.

![image](https://github.com/user-attachments/assets/19840453-86ac-4e74-bca0-7f3caf3e491f)

#### b) Correcting the incorrect category names.

First, we will add "Unknown" to the City category, replace all occurrences of "InvalidCity" with "Unknown," and then remove the unnecessary categories.

![image](https://github.com/user-attachments/assets/bf7e6865-60eb-4157-be9c-19e81821706e)

#### c) Filling the missing values
Now, let's fill all the missing values.

![image](https://github.com/user-attachments/assets/7058a9c5-c5de-428a-af81-3a4e477e5ade)
![image](https://github.com/user-attachments/assets/39a07645-e8e5-4461-85a6-9315d0c2dd5b)

#### d) Setting the matching criteria

To ensure all location data is correct, we will write a function that will fix incorrect values. First, we will create dictionaries with the correct mappings.

![image](https://github.com/user-attachments/assets/db2c3e00-28fc-45f4-9447-051e43379895)

After that, we need to write the function itself. To check if the location is correct, we will first look at the city. If the city is known, we will use the dictionaries to fill in the corresponding country and state. If only the state is known, we will use the dictionaries to determine the country. If only the country is known, no more specific information can be obtained.

![image](https://github.com/user-attachments/assets/9a25c939-fa52-4467-b91c-6db23486aa5b)

The corrected data will be saved in a new DataFrame, df3.

#### Correct location data

Now, let's compare.

![image](https://github.com/user-attachments/assets/2784a727-8a17-4cbe-a663-c16b4250fe7d)
![image](https://github.com/user-attachments/assets/181e5cf2-251b-4441-9f6d-943dba05443f)

As we can see, the location information is now displayed correctly.

### 5) Products

![image](https://github.com/user-attachments/assets/15216c48-4b5d-4f08-bbaa-292ebe92e47f)

First, we need to remove incorrect category names and fill in the missing values.

#### a) Correcting the incorrect category names

![image](https://github.com/user-attachments/assets/3c909d25-7c77-40f8-b34f-5ff94beb5652)

![image](https://github.com/user-attachments/assets/b8695558-e34a-4f44-8a7b-c970cd5ca41b)

#### b) Filling the missing values

We add the "Unknown" category to each category set.

![image](https://github.com/user-attachments/assets/5626d7c9-512b-4a2e-b8b6-d673d0c9b300)

After that, we fill all the missing values with this value.

![image](https://github.com/user-attachments/assets/81ebfa19-ca53-4717-9e22-776f19bad9b9)

Now, let's see the result.

![image](https://github.com/user-attachments/assets/d4ee5040-5171-4173-bb0a-eaea9baa89c8)

We can see that the products do not consistently match the categories or subcategories. Therefore, we need to set the correct mappings, similar to how we handled the location data.

#### c) Setting the matching criteria

First, we will create the mapping dictionaries.

![image](https://github.com/user-attachments/assets/02da9999-7a94-45c2-8c08-a8c8520466e0)

After that, we will write a function similar to the previous one.

![image](https://github.com/user-attachments/assets/298eff13-abd3-4256-a653-27dd8c0323ff)

Now, let's save the corrected data in a new DataFrame and compare them.

![image](https://github.com/user-attachments/assets/efcdb80a-5742-4590-9329-45cca1214a3e)

![image](https://github.com/user-attachments/assets/3ef047f5-a23f-480f-829e-f84aad94bdd1)

Now, we can see that all the category mappings are correct.

#### d) Identifier

The product identifier is ProductID. The identifier should be unique for each individual product, so let's first check the number of unique IDs.

![image](https://github.com/user-attachments/assets/35e16222-5372-46ac-b1d3-fb92c7da80f8)

We have 1000 unique IDs, but only 5 unique products (with clearly specified names), and there are also products with unknown names. Let's check how many of them there are.

![image](https://github.com/user-attachments/assets/f1894119-e3e4-4dd7-897e-844c4e31b0f2)

So, we have 1600 values, each of which should have a unique ID, but there are only 1000 unique IDs in total.

Let's take a look at the products with ID 3225, for example.

![image](https://github.com/user-attachments/assets/3cb88a8f-4819-4367-ae05-da8379aa8c58)

We can see that the same identifier has been assigned to many different products, which means this identifier is not correct, and the column can be removed.

![image](https://github.com/user-attachments/assets/e38c371e-4dfd-4cc4-8bca-db4c65f241f8)

### 6) Order and delivery dates.

Let's check the order and delivery dates. Delivery cannot occur before the order, so let's see if there are any records where the delivery date is earlier than the order date.

![image](https://github.com/user-attachments/assets/9519fbca-389f-4f42-ba61-ec41927a5e8c)

We can see that there are many incorrect data entries. To fix this, we will change the order date to match the delivery date.

![image](https://github.com/user-attachments/assets/1b015348-3338-4b49-a62c-9b07b1c0ce2d)

### 7) Personal customer data.

"Let's look at the clients' personal data.

![image](https://github.com/user-attachments/assets/22df693a-7191-407d-858d-3b0f3c2fb5d0)

Firstly, we see that there is empty data. Secondly, there are repeating names, but they have different IDs and segments.

#### a) Checking the names

Let's look at the data for a client named David White, for example.

![image](https://github.com/user-attachments/assets/7fca83a7-b121-4a62-bc3d-e6a099bf0628)

We see that there are many mismatches, both with the segment and the ID. Therefore, the name column is not informative and should be deleted.

![image](https://github.com/user-attachments/assets/25eabfad-992a-4a96-89a9-5f2aa9b30f61)

#### b) Filling the missing values

First, let's see how many empty values there are among the IDs.

![image](https://github.com/user-attachments/assets/eb7defa7-6b07-4bd1-a4e6-b89b039600a5)

Let's fill them with the next values after the maximum.

![image](https://github.com/user-attachments/assets/024f1def-c7da-4f21-abc7-344c039c8c18)

Now let's move on to Segment.

![image](https://github.com/user-attachments/assets/c2a4820a-58bd-480f-bf3e-3d92e8bdf6a5)

Let's take a look at the unique values.

![image](https://github.com/user-attachments/assets/e054d4e4-487b-4e36-a7b5-e460ce5b1d31)

Let's replace the missing values with Undefined.

![image](https://github.com/user-attachments/assets/16054a54-d6ba-4270-a874-3535a60d9d39)

### 8) Numerical data and payment

Let's take a look at some values from these columns.

![image](https://github.com/user-attachments/assets/7146cab2-6fa3-42b0-a3e4-7d8578b3d690)

#### a) Sales і Quantity

First, let's see if there are any values less than 0 in the Sales column.

![image](https://github.com/user-attachments/assets/43a079f5-0151-4dcb-a787-393513b6594e)

Perhaps negative values mean product returns, but for that, the quantity should also be negative.

Let's see how many rows have negative values in both columns.

![image](https://github.com/user-attachments/assets/30ed243c-95cd-43fc-8787-f725321d6c7e)

So these data mean returns, and all others (where only the value of one of the two columns is negative) can be deleted.

![image](https://github.com/user-attachments/assets/00518716-150a-4f49-81f5-24cfd46d3192)

Also, let's check for the missing data.

![image](https://github.com/user-attachments/assets/25acd9de-a953-40b3-afda-019482bc048e)

Therefore, there is no missing data among these columns.

#### b) Discount

Let's look at the unique values. The discount percentage can be in the range from 0 to 100.

![image](https://github.com/user-attachments/assets/2ffc4f83-2e68-47b9-aa84-45acd2939bc6)

So, there are values 110 and -10 here, which most likely just mean 10%, but a recording error was made. Let's replace these data with 10.

![image](https://github.com/user-attachments/assets/bcccde40-636a-443a-9376-25d675517dc1)

And fill Na with zeros.

![image](https://github.com/user-attachments/assets/7a49167f-a144-45d2-94db-cf53e5da259e)
![image](https://github.com/user-attachments/assets/bd37b546-f5cc-404d-abe9-54acc40aed1a)

#### c) Profit і ShippingCost

Let's also look at the unique values.

![image](https://github.com/user-attachments/assets/5ebab924-0f3b-4661-b574-b5628403fd2d)

Also, let's fill Na with zeros.

![image](https://github.com/user-attachments/assets/989f382d-c7ec-4e9e-8fa8-e7ee2d2d885b)
![image](https://github.com/user-attachments/assets/0021fb98-bf19-4a1a-9555-0919370bc265)

And do the same with ShippingCost.

![image](https://github.com/user-attachments/assets/271a22fb-f039-44bc-b5ab-fbdcf672c02b)
![image](https://github.com/user-attachments/assets/c8ff1ca2-4109-4444-8593-e4ee272c2420)

#### d) CustomerRating

Let's see what ratings our customers have.

![image](https://github.com/user-attachments/assets/8a4aa19f-2e10-490f-ab02-4c3180fb00ef)

It's hard to determine the limits of this rating, because the highest value is 100, and the lowest is -1.

To check if these are isolated errors, let's look at how many rows there are with different ratings.

![image](https://github.com/user-attachments/assets/51d4f09a-e9d3-4ff9-8ac9-cc8a5b84ff4b)

We see that these are not isolated errors, so the rating scale here is unclear to us, and therefore this column is not informative and can be deleted.

![image](https://github.com/user-attachments/assets/4627b0b9-dc4c-4e49-890f-83434503e515)

#### e) PaymentMethod

![image](https://github.com/user-attachments/assets/ef6eb1f6-77c3-4675-948f-31aa3429efd6)

Filling the Na values with the Undefined.

![image](https://github.com/user-attachments/assets/1f7f2f18-153d-4b5e-928f-6b25a58d2c55)

## 4. Data visualization and analysis

After cleaning the data, let's save this dataframe to a csv file.

![image](https://github.com/user-attachments/assets/a2b1a71c-ec74-4975-b6dc-001002d1515a)

Now let's use the **describe()** function to view some basic statistical data.

![image](https://github.com/user-attachments/assets/4cbff2ff-62cf-4efd-827b-aa93080eb95c)

Also, for better visual understanding, let's create plots using the **matplotlib** and **seaborn** libraries.

![image](https://github.com/user-attachments/assets/395e9cec-6726-4f04-bb07-63d43427c16a)

Let's look at the plots themselves.

<img src="https://github.com/user-attachments/assets/7b62fc07-0219-43cb-b924-321e0728ac75" width="700">

From the sales graph, it is clear that the distribution is almost uniform, with the exception of values less than zero and close to it. These values represent product returns, so it's not surprising that there are fewer of them.

On the profit distribution graph, a discrete distribution with clear steps is visible, peaking at a value of 0. This means that unprofitable transactions are encountered most often.

The graphs comparing sales with profits and discounts with profits are very strange, as they do not show any correlation.

Therefore, I decided to create another graph that will display the correlations between all these values.

![image](https://github.com/user-attachments/assets/a220378b-4b46-48ad-b735-ac8c70290ce1)

<img src="https://github.com/user-attachments/assets/c5c656c9-b85a-4405-9b3f-0c6b91877702" width="500">

**Conclusion:**
This graph shows that there is no correlation between all the values, which indicates that this data is not real, but artificially created, and there is no point in analyzing it. But despite the impossibility of analyzing this data due to poor data quality, this project serves as a good demonstration of skills in data cleaning and visualising.
