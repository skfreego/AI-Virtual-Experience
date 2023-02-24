# AI-Virtual-Experience
Cognizant | Forage

Gala Groceries is a technology-led grocery store chain based in the USA. They rely heavily on new technologies, such as IoT to give them a competitive edge over other grocery stores. 	
	They pride themselves on providing the best quality, fresh produce from locally sourced suppliers. However, this comes with many challenges to consistently deliver on this objective year-round.
	Gala Groceries approached Cognizant to help them with a supply chain issue. Groceries are highly perishable items. If you overstock, you are wasting money on excessive storage and waste, but if you understock, then you risk losing customers. They want to know how to better stock the items that they sell.
	This is a high-level business problem and will require you to dive into the data in order to formulate some questions and recommendations to the client about what else we need in order to answer that question.
	Once you’re done with your analysis, we need you to summarize your findings and provide some suggestions as to what else we need in order to fulfill their business problem. Please draft an email containing this information to the Data Science team leader to review before we send it to the client.

__________________________________________________________________________________

Task 1 - Exploratory Data Analysis
This notebook will walk you through this task interactively, meaning that once you've imported this notebook into Google Colab, you'll be able to run individual cells of code independantly, and see the results as you go.

This notebooks is designed for users that have an understanding of Python and data analysis. There will be some helper functions and initial setup code provided, but it will be up to you to perform the analysis and to draw insights!

--------------------

Section 1: Setup

	First, we need to mount this notebook to our Google Drive folder, in order to access the CSV data file.
	In order to view, analyse and manipulate the dataset, we must load it into something called a dataframe, which is a way of storing tabulated data in a virtual table. This dataframe will allow us to analyse the data freely. To load it into a dataframe, we will need a package called Pandas. 
		!pip install pandas

	And now we can import this package like so:
		pip install pandas


Section 2 - Data loading

	Now that Google Drive is mounted, you can store the CSV file anywhere in your Drive and update the path variable below to access it within this notebook. Once we've updated the path, let's read this CSV file into a pandas dataframe and see what it looks like
	path = "/content/drive/My Drive/Cognizant Virtual Internship2023/sample_sales_data.csv"
	df = pd.read_csv(path)
	df.drop(columns=["Unnamed: 0"], inplace=True, errors='ignore')
	df.head()

  Using the .head() method allows us to see the top 5 (5 by default) rows within the dataframe. We can use .tail() to see the bottom 5. If you want to see more than 5 rows, simply enter a number into the parentheses, e.g. head(10) or tail(10)


Section 3 - Descriptive statistics

	From just looking at the data, it is hard to get a feeling of what all the columns and rows mean. To gain an understanding of the dataset, let's first look at what columns and datatypes we have:

	df.info()
   The .info() method creates a data description of the dataframe. It tells us:
	#   Column          Non-Null Count  Dtype  
---  ------          --------------  -----  
 0   transaction_id  7829 non-null   object 
 1   timestamp       7829 non-null   object 
 2   product_id      7829 non-null   object 
 3   category        7829 non-null   object 
 4   customer_type   7829 non-null   object 
 5   unit_price      7829 non-null   float64
 6   quantity        7829 non-null   int64  
 7   total           7829 non-null   float64
 8   payment_type    7829 non-null   object

	How many rows (7829)
	How many columns (8)
	Column names
	How many values are non-null for each column
	The types of data contained within each column
	The size of the dataset loaded into memory (~550KB)
	Looking at the output of the .info() method, we can intepret each column as 	follows:

transaction_id = this is a unique ID that is assigned to each transaction
timestamp = this is the datetime at which the transaction was made
product_id = this is an ID that is assigned to the product that was sold. Each product has a unique ID
category = this is the category that the product is contained within
customer_type = this is the type of customer that made the transaction
unit_price = the price that 1 unit of this item sells for
quantity = the number of units sold for this product within this transaction
total = the total amount payable by the customer
payment_type = the payment method used by the customer
It is also interesting to look at the datatypes. We can see that there are 3 different datatypes within this dataset:

object = this column contains categorical values
float64 = this column contains floating point numerical values (i.e. decimal numbers)
int64 = this column contains integer values (whole numbers)
Now let's compute some descriptive statistics of the numeric columns

	df.describe()
   The .describe() method computes some descriptive statistics of the numerical columns, including:

	count = count of how many unique values exist
	mean = mean average value of this column
	std = standard deviation
	min = minimum value
	25% = lower quartile value
	50% = median value
	75% = upper quartile value
	max = maximum value

Section 4 - Visualisation

 	These statistics are useful, but they are better understood using visualisations. For visualisation, we will use the seaborn package, which makes it really easy to create visualisations from a dataframe.
To use them, let's first install and then import it:
	!pip install seaborn

	import seaborn as sns
    To analyse the dataset, below are snippets of code that you can use as helper functions to visualise different columns within the dataset. They include:

plot_continuous_distribution = this is to visualise the distribution of numeric columns (float64)
get_unique_values = this is to show how many unique values are present within a column
plot_categorical_distribution = this is to visualise the distribution of categorical columns (object)
--------------------------------The Code------------------------------------------
def plot_continuous_distribution(data: pd.DataFrame = None, column: str = None, height: int = 8):
  _ = sns.displot(data, x=column, kde=True, height=height, aspect=height/5).set(title=f'Distribution of {column}');

def get_unique_values(data, column):
  num_unique_values = len(data[column].unique())
  value_counts = data[column].value_counts()
  print(f"Column: {column} has {num_unique_values} unique values\n")
  print(value_counts)

def plot_categorical_distribution(data: pd.DataFrame = None, column: str = None, height: int = 8, aspect: int = 2):
  _ = sns.catplot(data=data, x=column, kind='count', height=height, aspect=aspect).set(title=f'Distribution of {column}');
---------------------------------------------------------------------------------

// kde = True
	Kernel Density Estimation (KDE) is a way to estimate the probability density function of a continuous random variable. It is used for non-parametric analysis. Setting the hist flag to False in distplot will yield the kernel density estimation plot

// displot
	A Distplot or distribution plot, depicts the variation in the data distribution. Seaborn Distplot represents the overall distribution of continuous data variables. The Seaborn module along with the Matplotlib module is used to depict the distplot with different variations in it.

// catplot
	Catplot can handle 8 different plots currently available in Seaborn. catplot function can do all these types of plots and one can specify the type of plot one needs with the kind parameter. So it is kind of one stop for every plot you will require for bivariate analysis.

// get_unique_values(df, 'transaction_id')
	transaction_id is a unique ID column for each transaction. Since each row represents a unique transaction, this means that we have 7829 unique transaction IDs. Therefore, this column is not useful to visualise.

// get_unique_values(df, 'product_id')
	Similarly, product_id is an ID column, however it is unique based on the product that was sold within the transaction. From this computation, we can see that we have 300 unique product IDs, hence 300 unique products within the dataset. This is not worth visualising, but it certainly interesting to know. From the output of the helper function, we can see that the product most frequently was sold within this dataset was ecac012c-1dec-41d4-9ebd-56fb7166f6d9, sold 114 times during the week. Whereas the product least sold was ec0bb9b5-45e3-4de8-963d-e92aa91a201e sold just 3 times

// get_unique_values(df, 'category')
	There are 22 unique values for category, with fruit and vegetables being the 2 most frequently purchased product categories and spices and herbs being the least. Let's visualise this too.


// get_unique_values(fd, 'customer_type')
	There are 5 unique values for customer_type, and they seem to be evenly distributed. Let's visualise this:
--------------------------------------------------
Section 5 - Correlations
	 Correlations measure how each numeric column is linearly related to each other. It is measured between -1 and 1. If a correlation between 2 columns is close to -1, it shows that there is a negative correlation, that is, as 1 increases, the other decreases. If a correlation between 2 columns is close to 1, it shows that they are positively correlated, that is, as 1 increases, so does the other. Therefore, correlations do not infer that one column causes the other, but it gives us an indication as to how the columns are linearly related.

## Task 2
Your work on the previous task was very helpful to propel this project forward with the client. Based on your recommendations, they want to focus on the following problem statement:

“Can we accurately predict the stock levels of products based on sales data and sensor data on an hourly basis in order to more intelligently procure products from our suppliers?” 

The client has agreed to share more data in the form of sensor data. They use sensors to measure temperature storage facilities where products are stored in the warehouse, and they also use stock levels within the refrigerators and freezers in store. 

It is your task to look at the data model diagram that has been provided by the Data Engineering team and to decide on what data you’re going to use from the data available. In addition, we need you to create a strategic plan as to how you’ll use this data to complete the work to answer the problem statement. 

You can summarize your choices and plan of work in a PowerPoint presentation. This PowerPoint will be sent to the Data Science team leader and the client for a review. Make sure to keep it concise (ideally 1 slide) and business-friendly. 


# Task 3 

It is now time to get started with some machine learning!

The client has provided 3 datasets, it is now your job to combine, transform and model these datasets in a suitable way to answer the problem statement that the business has requested. 

Most importantly, once the modeling process is complete, we need you to communicate your work and analysis in the form of a single PowerPoint slide, so that we can present the results back to the business. The key here is to use business-friendly language and to explain your results in a way that the business will understand. For example, ensure that when you’re summarizing the performance of the results you don’t use technical metrics, but rather convert it into numbers that they’ll understand. 

### Task 4

# ------- BEFORE STARTING - SOME BASIC TIPS
# You can add a comment within a Python file by using a hashtag '#'
# Anything that comes after the hashtag on the same line, will be considered
# a comment and won't be executed as code by the Python interpreter.

# --- 1) IMPORTING PACKAGES
# The first thing you should always do in a Python file is to import any
# packages that you will need within the file. This should always go at the top
# of the file

# --- 2) DEFINE GLOBAL CONSTANTS
# Constants are variables that should remain the same througout the entire running
# of the module. You should define these after the imports at the top of the file.
# You should give global constants a name and ensure that they are in all upper
# case, such as: UPPER_CASE

# --- 3) ALGORITHM CODE
# Next, we should write our code that will be executed when a model needs to be 
# trained. There are many ways to structure this code and it is your choice 
# how you wish to do this. The code in the 'module_helper.py' file will break
# the code down into independent functions, which is 1 option. 
# Include your algorithm code in this section below:
 
# --- 4) MAIN FUNCTION
# Your algorithm code should contain modular code that can be run independently.
# You may want to include a final function that ties everything together, to allow
# the entire pipeline of loading the data and training the algorithm to be run all
# at once
