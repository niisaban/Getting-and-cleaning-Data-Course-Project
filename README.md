# Getting-and-cleaning-Data-Course-Project

This is a project to demonstrate ability to collect, work with, and clean a data set.
The R script,  'run_analysis.R' , does the following:

1. Downloads a file containing a dataset
2. unzips the file and sets a working directory
3. Loads the activity and feature info
4. Loads both the training and test datasets, keeping only those columns which 
   reflect a mean or standard deviation
5. Loads the activity and subject data for each dataset, and merges those 
   columns with the dataset
6. Merges the two datasets
7. Converts the 'activity' and 'subject' columns into factors
8. Creates a tidy dataset that consists of the average (mean) value of each 
   variable for each subject and activity pair.

The end result of a tidy dataset is shown in the file  'tidy.txt'.

