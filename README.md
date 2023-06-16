# Analyzing-Spotify-and-YouTube-Songs-Using-Python-MySQL
Data science project - Analyzing Spotify and YouTube Songs Using Python &amp; MySQL

## Project Description

This project helped us understand how a real-world database is analyzed using SQL, how to get maximum available insights from the dataset, pre-process the data using Python for better upcoming performance, how a structured query language helps us retrieve useful information from the database

The Project will consist of 2 modules:

Module 1: Data Pre-processing data using Python

Module 2: Analyzing data using SQL

### Data Source

Data was provided by HiCounselor.
![spotify](https://github.com/lovepreetdhalla/Analyzing-Spotify-and-YouTube-Songs-Using-Python-MySQL/assets/15218972/6fa47c3f-e314-4911-91ec-04f6ab0e91e2)
# Data Analysis Concepts Utilized In This Project

Data Preprocessing Using Python Programming and connecting to Database :

* Droping unnecessary columns <br />
* Droping null values<br />
* Droping duplicate values<br />
* Data type conversion<br />
* Renaming the column<br />
* Droping records which starts with special characters<br />
* Creating new column on the basis of some conditions<br />
* Saving newly cleaned dataset into csv file format<br />
* Converting dataframe into SQL Table, Uploading Table into phpmyadmin server and solving queries.<br />

 Using SQL Queries To Solve Problem Statements :

* Aggregating the data<br />
* Grouping the data<br />
* Ordering the data<br />
* Using addition of two columns to show as a calculated column<br />
# Module 1: Data Preprocessing using python:

Data Pre-processing is one of the important steps in data analytics because data that is not processed can lead to different unwanted results when the data will be used for further applications. This task includes sub-tasks such as handling null values, deletion or transformation of irrelevant values, datatype transformation, removing duplicates, etc<br />

* Task 1 : Column removal: Streamlining data for analysis (Eliminate unnecessary columns from the dataset for our analysis by removing Url_spotify, Uri, Key, Url_youtube, and Description).<br />
* Task 2 : Null Value Analysis :Assesing data completeness and column-wise null sum (Examine the dataset for the presence of null values and calculate the total count of null values for each column, providing insights into the data's completeness and potential data quality issues).<br />
* Task 3 : Null Value Handling (Manage null values in data for improved data quality and analysis and return dataframe).<br />
* Task 4 : Duplicate Check And First Value Retention : Data deduplication for accuracy(Identify and eliminate duplicate records in the dataset while retaining the first occurrence of each unique value. This ensures data integrity by removing redundant information and maintaining the original data structure).<br />
* Task 5 : Millisecond to Minute Conversion : Enhancing Time Duration Clarity(Convert the duration in milliseconds to minutes, facilitating a clearer comprehension and representation of time intervals in a more user-friendly format).<br />
* Task 6 : Column Renaming : Enhancing clarity with duration in minutes(Change the name of the modified column to "Duration_min" to accurately reflect the conversion from milliseconds to minutes, providing a more descriptive and meaningful representation of the data).<br />
* Task 7 : Exclusion of irrelevant tracks : Filtering out '?' prefix track names(Eliminate track names that are deemed irrelevant and begin with the "?" character, ensuring the dataset only includes relevant and meaningful track information for further analysis or processing).<br />
* Task 8 : Energy to Liveness Ration Calculation : Analysing the relationship and storing results(Compute the Energy to Liveness ratio for each track, quantifying the relationship between energy and liveliness attributes. The resulting ratios are then stored in a column named 'EnergyLiveness' for further analysis or interpretation).<br />
* Task 9 : Data Type Conversion : Transforming 'Views' to float for enhanced usability(Modify the data type of the 'views' column to float, enabling numerical operations and facilitating its utilization in subsequent analysis or calculations requiring floating-point values.<br />
* Task 10 : Platform Dominance Analysis : Identifying most played platform and creating 'Most_Playedon' column(Analyze the 'views' and 'stream' columns to determine the dominant platform (YouTube or Spotify) on which a song track was most played. Create a new column called 'most_playedon' with values 'Spotify' or 'YouTube' indicating the platform with the highest play count for each song track).<br />
* Task 11 : Data Export and Download : Saving and accessing 'Cleaned_dataset.csv'(Export the data to a CSV file named "cleaned_dataset.csv" and enable downloading by providing a clickable file name, allowing users to access and retrieve the file with ease).<br />
* Task 12 : Module 1 completion : Creating MySQL Table from exported 'Cleaned_dataset.csv'(Create a MySQL table named "cleaned_dataset" by utilizing the exported file, "cleaned_dataset.csv". Follow these steps: 1. Download the CSV file. 2. Create the table using the CSV file, either through an online editor or by executing SQL commands. 3. Click "Run Test" to conclude Module 1).<br />
# All python codes for Data-Preprocessing and converting to new csv file.
``` py
import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings("ignore")



#Task 1: Remove columns that are not needed in our analysis.
# Remove Url_spotify, Uri, Key, Url_youtube, Description
def Remove_columns():
    df = pd.read_csv('Spotify_Youtuben.csv')
    df.drop(columns=['Url_spotify','Uri','Key','Url_youtube','Description'],axis=1,inplace=True)
    return df

#Task 2: Check for the null values
def no_of_null_values():
    df=Remove_columns()
    df=df.isna().sum()
    return df

#Task 3: Handle the null values replace int value with 0 and other values with NA
def Handle_Null_values():
    df=Remove_columns()
    for i in df.columns:
        if df[i].dtypes=="float64":
            df[i].fillna(0,inplace=True)
        if df[i].dtypes=="object":
            df[i].fillna("NA",inplace=True)
    return df

#Task 4: CHECK FOR DUPLICATES AND REMOVE THEM KEEPING THE FIRST VALUE
def drop_the_duplicates():
    df=Handle_Null_values()
    df.drop_duplicates(keep="first",inplace=True)
    return df

#Task 5: CONVERT millisecond duration to minute for a better understanding
def convert_milisecond_to_Minute():
    df=drop_the_duplicates()
    df['Duration_ms']=pd.to_numeric(df['Duration_ms'])
    df['Duration_ms']=(df['Duration_ms']/(60000.0))
    return df

#Task 6: Rename the modified column to Duration_min
def rename_modified_column():
    df=convert_milisecond_to_Minute()
    df.rename(columns={"Duration_ms":"Duration_min"},inplace=True)
    return df

#Task 7: Remove irrelevant 'Track' name that starts with ?
def Irrelevant_Track_name():
    df=rename_modified_column()
    df=df[~df['Track'].str.startswith('?')]
    return df

