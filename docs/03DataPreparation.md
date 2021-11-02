# Data Preparation

There is cleaning required before the data can be consumed for ML.

First step is understanding the data.

Data Preparation is the process of transforming a dataset using different techniques to prepare it for model training 
and testing.

Ensure the relevant features are used for the problem at hand. 

The data preparation process is usually the most time consuming part of ML work. 

Example:

![DataPrepExample](../images/data_prep_example.png)

AWS tools to remember for data prep:
1. SageMaker & Jupyter Notebooks (more adhoc cleaning jobs)
2. ETL jobs in AWS Glue (more reusable cleaning jobs that can be rerun as required/on a schedule)

## Categorical Encoding

- Turing proved that anything can be encoded using 1's and 0's (Turing Machine).

- Categorical encoding is the process of manipulating categorical variables when ML algorithms expect numerical values
as inputs.

Note:

categorical variable == categorical feature == discrete feature

When should we do this encoding?

(Note, certain model types require numerical values)

![WhenToEncode](../images/when_to_encode.png)

Categorical Encoding Examples:

![CategoricalVariableExamples](../images/categorical_variable_examples.png)

Nominal variables - order does not matter
Ordinal variables - order does matter

In depth example:

![CategoricalEncodingFullExample](../images/categorical_encoding_full_example.png)

Loan_Approved - simple binary encoding:

![LoanApproved](../images/loan_approved.png)

Pool_Size - define a map, choosing sizes for small, large medium. You may need to tweak these to get the
required output:

![PoolSize](../images/pool_size.png)

Num_Home_Type - can't just encode these as 1, 2, 3 because these are nominal values where order doesn't matter. 
By doing 1, 2 and 3, the algorithm will think that one is 'better' than another:

![HomeType1](../images/home_type_1.png)
 
Option instead is:

![HomeType2](../images/home_type_2.png)

One-hot Encoding - transforms nominal categorical features and creates new binary columns for each observation:
    
One-hot encoding can lead to a problem if there are many, many variables (for obvious reasons). Then the question is, 
should we one-hot or not?

* Can instead use techniques like grouping by similarity, which could create fewer overall categories before encoding.
* Can also map rare values to "other", which can help reduce the overall number of new columns created by one-hot 
encoding.
    
### Categorical Encoding Summary

![CategoricalEncodingSummary](../images/categorical_encoding_summary.png)

## Text Feature Engineering

- Feature engineering generally means transforming attributes within our data to make them more useful within our model 
for the problem at hand.

- Text Feature Engineering means transforming text within out data so ML algorithms can better analyse it (splitting text
into bite sized pieces).

Simple example case for text feature engineering:

![SimpleTextFeatureEngineeringExample](../images/simple_text_feature_engineering_example.png)

Values are tokenised from one raw text string to bite sized pieces.

### Bag-of-Words

- Tokenises raw text and creates a statistical representation of the text. 
- Breaks up text by white space into single words 

Example:

![BagOfWords](../images/bag_of_words.png)

### N-Gram

- An extension of Bag-of-Words which produces groups of words of n size.
- Breaks up text by white space into groups of words.

Note, N-gram, size = 1, gives the same result as Bag-of-Words.

Note, when we apply N-gram, it also produces groups less than it's size, so N-gram, size = 2 will produce tokens of 
size 2 and 1.

Example where N-gram, size = 2:

![NGram](../images/n_gram.png)

Technique could be used in a spam filter for example to find phrases such a 'Click here' or 'You're a Winner'.

![NGramNumberGrams](../images/n_gram_number_grams.png)

### Orthogonal Sparse Bigram (OSB)

- Creates groups of words of size n and outputs every pair of words that include the first word.
- Creates groups of words that always include the first word. 

Breaking down OSB:

![OSBWordsMeaning](../images/osb_words_meaning.png)

Note - OSB is not better or worse than N-Gram, it's just a different technique to use; one may work better than another
in certain situations.

Example:

![OSB1](../images/osb_example_1.png)

![OSB2](../images/osb_example_2.png)

### Term Frequency - Inverse Document Frequency (tf-idf)

- Represents how important a word or words are to a given set of text by providing appropriate weights
to terms that are common and less common in the text. 
- Shows us the popularity of a word or words in text data by making common words like 'the' or 'and' less important.

Breaking down tf-idf:

![TFIDFWordsMeaning](../images/tf_idf_words_meaning.png)

Example:

![TFIDFExample](../images/tf_idf_example.png)

**Vectorised Tf-idf (could come up in exam):**

Syntax - (number of documents, number of unique n-grams)

e.g. (2,7) for above example where the number of documents is 2 and the number of unique n-grams is 7 

(See resource for how to vectorise)

### Use cases for these text feature engineering transformations

![WhichTransformation](../images/which_transformation.png)

### Remove punctuation and lowercase transformation

- These are straight forward transformations, however are often very useful to do upfront before we do any other
type of text transformation.

Examples:

![RemovePunctuation](../images/remove_punctuation.png)

Note - punctuation inside of words considered important, so in most cases this isn't removed (e.g. apostrophes and 
hyphens)

![Lowercase](../images/lowercase.png)

### Cartesian Product Transformation

- Creates a new feature from the combination of two or more text or categorical values. 
- Combining sets of words together.

Example:

![CartesianProductExample](../images/cartesian_product_example.png)

If a buyer buys one of these books, this could be used to create more suggestions about future books they could buy
i.e. targeted marketing.

### Feature Engineering Dates

- Translating dates into useful information 

Examples of useful information we can get:

![DatesUsefulInfo](../images/dates_useful_info.png)

Example of date_feature_engineering:

![DatesEngineeringExample](../images/dates_engineering_example.png)

With this information extracted, we can then determine more important correlations/information about our
information. 

## Summary of Text Feature Engineering Methods Discussed

(There are many other transformations out there, but some of the main ones have been discussed)

![TextFeatureEngineeringSummary](../images/text_feature_engineering_summary.png)

## Numeric Feature Engineering

- Transforming numeric values within our data so ML algorithms can better analyse them

### Feature Scaling

- Changes numeric values so all values are on the same scale:

    * **Normalisation** (note, in the context of the exam: scaling = feature scaling = normalisation):
    
         Example: we want to apply Normalisation to the 'Price' feature. The way normalisation works is that is assigns
         the minimum value to 0 and the maximum value to 1 and then it scales the other values in between.
         
         ![ScalingExampleOverall](../images/scaling_example_overall.png)
         
         ![PriceScaleBeforeNormalisation](../images/price_scale_before_normalisation.png)
         
         ![PriceScaleAfterNormalisation](../images/price_scale_after_normalisation.png)
         
         Calculation explained (don't need to know this specifically for the exam):
         
         ![NormalisationCalculation](../images/normalisation_calculation.png)
         
         This is the most common and easiest method for scaling values. The problem with normalisation is that OUTLIERS
         CAN THROW OFF NORMALISATION!! 
     
    * **Standardisation**:
        
        This works by setting the average value to 0 and then uses the z-score for the remainder of the values.
        
        Using same Price example again:
        
        ![PriceScaleAfterStandardisation](../images/price_scale_after_standardisation.png)
        
        The z-score takes into account the price average and the price standard deviation and smooths out the values
        so that outliers are not as much of an issue
        
        Calculation explained (don't need to know this specifically for the exam):
        
        ![StandardisationCalculation](../images/standardisation_calculation.png)
        
Feature Scaling Summary:

![FeatureScalingSummary](../images/feature_scaling_summary.png)
    
### Binning

- Changes numeric values into groups or buckets of similar values
    * Quantile Binning aims to assign the same number of features to each bin
    
We can use binning when we know that a numeric feature within our dataset doesn't have a linear role with our prediction
or our target attribute i.e. if we know that the increasing/decreasing of the numeric feature doesn't have any 
correlation with our target attribute, then that feature is a great candidate for binning. 

Often a person's age is a good candidate for binning e.g. we don't care so much about whether somebody is 51 or 52, 
but we care more about the life stage of a person's age. 

Can then use the bins as categorical variables.

Example:

![BinningExample](../images/binning_example.png)

The drawback with setting a fixed number of bins is that we may have irregular bins/the bins are not uniform
with the number of data points. We are looking for more even distribution between the values that are in the bins.

To overcome this, we can apply a technique called Quantile Binning:

Same Example once we have applied Quantile Binning:

![QuantileBinningExample](../images/quantile_binning_example.png)

Note - after we have done this, we can assign a categorical variable to each bin, for example: 

'Youth', 'Young Adult', 'Adult'

... and then we can use other transformation techniques to hopefully find better correlation with our target attribute

Example whole process explained:

![QuantileBinningProcess](../images/quantile_binning_process.png)

Binning Summary:

![BinningSummary](../images/binning_summary.png)

### Numeric Feature Engineering Summary

![NumericFeatureEngineeringSummary](../images/numeric_feature_engineering_summary.png)

## Other Feature Engineering

* **Image Feature Engineering** - can be done to extract useful information from images before using them with ML
algorithms. A lot of these techniques are out of the scope of the exam, but worth covering to give a more complete idea
of feature engineering.

Simple example problem:

Is this character a number - humans could look at the picture and know that it is the number 3, the way we can feature
engineer this image is by breaking the image up into a grid and then translating the values in the grid that are black 
to 1 and that are white to 0. 

![ImageFeatureEngineeringExample](../images/image_feature_engineering_example.png)

This grid of 1's and 0's can then be compared against other known images of numbers to work out if the image is of a 
number (e.g. the MNIST dataset).

* **Audio Feature Engineering** - can be done to extract useful information from sounds and audio before using them with ML
algorithms. 

Example simple audio stream - can be sampled at different points in time to give a data set of amplitude values
that actually represent the audio stream in numeric format.

![AudioFeatureEngineeringExample](../images/audio_feature_engineering_example.png)

### Dataset Formats

Amazon allows 2 different modes: File (most traditional), and Pipe (provides faster start times for our training jobs/
our algorithms and provides better throughput).

![DatasetFormats](../images/dataset_formats.png)

Notes - recordIO-protobuf is a file type. A tensor is a mutli-dimensional array.

Example python code to create a recordIO-protobuf (will go into this more in detail in the algorithms chapter
and in the hands on labs):

![RecordIOProtobufPython](../images/recordio_protobuf_python.png)

## Handling Missing Values

Missing values in your datasets can interfere with analysis and model predictions.

Missing data can be represented in many different ways:

- null
- NaN
- NA
- None
- etc...

Handling missing values is an important data preparation step.

It is important to understand why values are missing in order to make a decision about how to handle them. 

Missingness Mechanisms - why is data missing:

![MissingnessMechanisms](../images/missingness_mechanisms.png)

Techniques to handle missing values:

![HandleMissingValues](../images/handle_missing_values.png)

Note - replacing data is known as **data imputation**.

May get questions on the exam asking which technique to use to handle missing values.

## Feature Selection 

- Selecting the most relevant features from your data to prevent over-complicating the analysis, resolving potential
inaccuracies, and removes irrelevant features or repeated information.
- Feature selection is an intuitive step humans take to reduce the number of features.

Example:

![ExampleFeatureSelection](../images/example_feature_selection.png)

### Principal Component Analysis (PCA)

 - An unsupervised learning algorithm that reduces the number of features while still retaining as much information
 as possible.
 - This can be extremely helpful if we have 100's of features
 
### Feature Selection Use Cases

![FeatureSelectionUseCases](../images/feature_selection_use_cases.png)

## AWS Data Preparation Helper Tools

![DataPrepTools](../images/data_prep_tools.png)

### AWS Glue

![AWSGlueOverview](../images/aws_glue_overview.png)

- Fully managed ETL service that allows you to run Python or Scala code on your datasets to allow you to 
transform them. 
- It allows you to create jobs that transform your data and you can then run them either on demand or on 
a defined schedule or even when another AWS service is triggered e.g. when an object lands on S3.
- Before we can set up any data preparation jobs in AWS Glue, we first need to let AWS Glue know what our data source
actually looks like. This involves setting up a Crawler (explained in detail in 'Data Collection' notes).
- What's great about the Python and Scala code is that AWS Glue allows you to either upload your code manually, 
or it generates the code for you and allows you to edit it directly within the console - this gives a great starting 
point for transforming your data.
- AWS Glue consists of 2 key parts:
    1. Data Catalogue
    2. ETL
    
![AWSGlueFeatures](../images/aws_glue_features.png)

- Within the Data Catalogue there are 2 key things:
    1. Databases
    2. Crawlers - scan input data source and infer schema etc - then creates tables within the databases
- ETL jobs can then be set up once these database tables are established - when creating a job, we have the choice
to choose the type of job we want to create e.g. a Spark job - this is a fully managed cluster of Apache Spark servers 
that AWS Glue spins up in the background and allows us to run our transformation code on. *Note - Spark is the default 
job type.* We then have the choice or choosing between Python and Scala as our ETL language. Once we select an ETL 
language, then we select how we want the job to run:
    1. Allow AWS Glue to generate the script for us - if you select Python, it automatically generate PySpark code
    (example transformation jobs you can choose in image below)
    2. Provide it with a script of our own 
    3. Start a brand new script from scratch
    
![AWSGlueJobsPySpark](../images/aws_glue_jobs_pyspark.png)

- Rather than choosing Spark as the job type, you can also choose Python shell, which allows you to write 
traditional python scripts to run on your datasets - see below for supported python libraries (Note, in this case, AWS Glue
then will not generate scripts for us).

![AWSGluePythonShell](../images/aws_glue_python_shell.png)

- AWS Glue also allows us to create Apache Zeppeling and Jupyter notebooks to do transformations. These are not 
specifically hooked into jobs, but allow us to do ad-hoc/simple transformations. If you create a notebook within AWS
Glue, it's actually hosted on the SageMaker service.
- Least amount of effort for ETL jobs in terms of the infrastructure you need to set up (AWS Glue is fully managed).

## AWS SageMaker

- SageMaker is must more than a data preparation too 
- It also allows you to build, train, and deploy ML models
- Here, we will just touch on the data preparation service it offers
- SageMaker allows you to create Jupyter Notebooks that are directly integrated within the SageMaker service. You can 
spin up notebook instances that are on a fully managed server, all within SageMaker.

- AWS Glue vs AWS SageMaker:

![AWSGlueSageMaker](../images/aws_glue_sagemaker.png)

## Elastic Map Reduce (EMR)

- Included here as a data preparation tool because the entire ETL/data preparation process could be done within the EMR 
ecosystem, however other services within AWS make the job much easier e.g. can integrate EMR and SageMaker together -
if we already have ETL processes/Spark jobs that are already running within our EMR cluster - we can directly integrate 
the SageMaker SDK for Apache Spark within our EMR cluster, so we can run all our SageMaker and Spark jobs together 
within the EMR cluster.
- Note, EMR is not fully managed, so EMR is not the easiest option for ETL, as we would have to managed all the clusters
ourselves within EMR.
- Reminder - EMR is a fully managed Hadoop cluster ecosystem that runs on multiple EC2 instances. EMR allows you to pick
and choose different frameworks that you want to include within the cluster. This allows you to run distributed
workloads over many EC2 instances if you have lots and lots of data. We could therefore choose frameworks that allow us 
to run our transformation jobs on a distributed system e.g. 

![EMRFrameworks](../images/emr_frameworks.png)

## Amazon Athena

- Allows you to run SQL queries on S3 data
- Athena is a serverless platform that is fully managed by AWS
- As long as you have your Data Catalogue set up within AWS Glue, you can then query it using Athena
- This means you can do any SQL transformations on your data to prepare it for your ML 

## AWS Data Pipeline

- This allows you to process and move data between different AWS compute services e.g. 

![DataPipelineOverview](../images/data_pipeline_overview.png)

- In most cases, Data Pipeline wouldn't be used for an ETL job, however if for some reason you didn't want to
use Python or Scala and instead you wanted to use Java/Javascript etc, you could always have the data pipeline
transform/prepare the data for you
- When creating a Data Pipeline, you can choose between the several built-in templates that AWS offers, to migrate your
data from one service to another:

## Which Data Preparation service should I use?

In most cases, AWS Glue will be your go-to service. 

![WhichDataPrepService](../images/which_data_prep_service.png)

## Exam Tips

![DataPreparationExamsTips1](../images/data_preparation_exam_tips_1.png)

![DataPreparationExamsTips2](../images/data_preparation_exam_tips_2.png)

![DataPreparationExamsTips3](../images/data_preparation_exam_tips_3.png)

![DataPreparationExamsTips4](../images/data_preparation_exam_tips_4.png)

![DataPreparationExamsTips5](../images/data_preparation_exam_tips_5.png)






 






