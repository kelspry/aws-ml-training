##Data Collection

*Machine Learning Cycle* represents the iterative process of ML:

![ML Cycle](./images/ml_cycle.png)

Goal: Understand the problem at hand, understand the parts of our input data, map our data into AWS

###Data Collection Concepts

#### ML - questions to ask

1. What type of generalisation are we seeking? What type of answer are we looking to get to?
2. Do we really need ML?
3. How will my ML generalisation be consumed?
4. What do we have to work with?
5. How can I tell if the generalisation is working?

#### Data

![Data](./images/good_bad_data.png)

General rule of thumb - you should have at least 10 times as many data points as the total number of features.

Where does data come from?

- No matter how you get your data, you are aiming to build a single data repository.

###Data Terminology

*Datasets* - input data = training/testing data (in ML)
*Column/Attribute* - feature of the data
*Row/Observations* - different data points in the dataset
*Structured Data* - defined schema - schema is info needed to interpret the data e.g. attribute names and their 
assigned data types 
*Unstructured Data* - no defined schema or structural properties. This makes up the majority of data collected e.g. 
images/pdf's/videos etc
*Semi-structured Data* - too unstructured for relational data, but has some organisational structure e.g. 
No-SQL data, data usually in csv/json/xml format.

![StructuredUnstructured](./images/structured_unstructured.png)

*Database* - single repository for applications or other services that are transactional.
Whenever we write to a transactional database, if there is an error, the changes are rolled back. 

*Data Warehouse* - collect data from many different sources and in many different formats and congregate them together 
in a data warehouse. Traditional, before the data lands into the Data Warehouse there is some processing/cleaning of 
the data before it lands in the Data Warehouse. This is so we can run analytics/use BI tools etc from the data in the 
Warehouse. Warehouses are used to stores large volumes of data that we can analyse more easily.

*Data Lake* - Store mass amounts of unstructured data that are dumped into a Data Lake (single repo). Traditionally 
there is no processing done before putting data into a Data Lake. Data Lakes are often used to store historical data or
data we are not sure what to do with it yet etc. 



