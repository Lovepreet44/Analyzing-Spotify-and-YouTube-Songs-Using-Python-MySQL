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

#Task 8: Calculate the Energy to Liveness ratio for each track and store it in columns 'EnergyLiveness'
def Energy_to_liveness_Ratio():
    df=Irrelevant_Track_name()
    df['EnergyLiveness']=(pd.to_numeric(df['Energy'])/pd.to_numeric(df['Liveness']))
    return df

#Task 9: change the datatype of 'views' to float for further use
def change_the_datatype():
    df=Energy_to_liveness_Ratio()
    df['Views']=pd.to_numeric(df['Views'])
    return df

#Task 10: compare the views and stream columns to infer
# that the song track was more played on which platform, youtube or Spotify.
# Create a column named most_playedon which will have two values.
# Spotify and Youtube,If a song track is most played on youtube then
# the most_played on column will have youtube as the value for that particular song
def compare_the_views():
    df=change_the_datatype()
    df['most_playedon']=np.where(pd.to_numeric(df['Stream'])>pd.to_numeric(df["Views"]),"Spotify","Youtube")
    return df

#Task 11: export the cleaned dataset to CSV to "cleaned_dataset.csv"
def export_the_cleaned_dataset():
    df=compare_the_views()
    df.to_csv("cleaned_dataset.csv",index=False)
    

#TASK 12
#follow the instruction in the Task 13 description and complete the task as per it.

#check if mysql table is created using "cleaned_dataset.csv"
#Use this final dataset and upload it on the provided database for performing analysis in  MySQL
#To run this task click on the terminal and click on the run project
```
# Module 2: Data Analysis using SQL

* In this module, we worked on performing data analysis on the pre-processed data from the previous module and conducting Data Analysis using SQL.
* We generated queries for given problem statements. We performed queries after making connection to phpmyadmin database server.

## Task 1 : Write an SQL query to solve the given problem statement.

* Which is the most viewed song track on youtube?
## Task 2 : Write an SQL query to solve the given problem statement.

* Which Song track is streamed most on Spotify?
## Task 3 : Write an SQL query to solve the given problem statement.

* EnergyLiveness ratio is one of the popular ways to measure the quality of a song, which are the top 5 songs that have the highest energyliveness ratio.
## Task 4 : Write an SQL query to solve the given problem statement.

* Let us assume a situation where an artist named Black Eyed Peas wants to analyze his songs. The artist wants to know which platform is capable of keeping his song track more engaged. To check this he assigns you this task and wants you to report to him where his song tracks are more played on. compare the platforms.
## Task 5 : Write an SQL query to solve the given problem statement.

* Gorillaz wants to know their most liked song on youtube. Report to them with their most liked song along with the Energy and Tempo of the song.

## Task 6 : Write an SQL query to solve the given problem statement.

* Which Album types are more prominent on Spotify?

## Task 7 : Write an SQL query to solve the given problem statement.

* Spotify's most loved song tracks are to be declared soon. Help Spotify choose the top 5 most streamed+youtube viewed song track.
## All the queries to solve the above mentioned problem statements :
``` sql
##code
# Solution: Task 1 : 
SELECT track,Views FROM cleaned_dataset ORDER BY Views DESC LIMIT 1;

##code
# Solution : Task 2 : 
SELECT track,Stream FROM cleaned_dataset WHERE most_playedon="spotify" ORDER BY Stream DESC LIMIT 1;

##code
# Solution : Task 3 : 
SELECT track,EnergyLiveness FROM cleaned_dataset ORDER BY EnergyLiveness DESC LIMIT 5;

##code
# Solution : Task 4 : 
SELECT COUNT(track),most_playedon FROM cleaned_dataset WHERE artist="Black Eyed Peas" GROUP BY most_playedon;

##code
# Solution : Task 5 : 
SELECT track,likes,energy,tempo FROM cleaned_dataset WHERE artist="gorillaz" AND most_playedon="spotify" ORDER BY Likes DESC LIMIT 1;

##code
# Solution : Task 6 : 
SELECT album_type,COUNT(album_type) AS total FROM cleaned_dataset GROUP BY album_type ORDER BY total DESC;

##code
# Solution : Task 7 : 
SELECT track,Views + Stream AS total FROM cleaned_dataset WHERE most_playedon="youtube" GROUP BY track ORDER BY total DESC LIMIT 5;

```


