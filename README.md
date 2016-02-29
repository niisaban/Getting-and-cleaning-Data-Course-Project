# Getting-and-cleaning-Data-Course-Project
A project to demonstrate ability to collect, work with, and clean a data set.
## Download the file and put the file in the data folder
dataset_url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(dataset_url, "Dataset.zip")

##Unzip the file
unzip("Dataset.zip", exdir = "Dataset")
##set working directory
setwd("C:/Users/Abdulrahman/Documents/Coursera/Getting and Cleaning Data/Dataset")

##unzipped files are in the folder UCI HAR Dataset. Get the list of files
files <- list.files("UCI HAR Dataset", recursive = TRUE)
files

## The README.txt file has detailed information on the dataset. The Inertial  Signals files will not be used.
##Files that will be used are: 
## 'test/X_test.txt', 'test/y_test.txt', 
## 'train/subject_train.txt', 'train/X_train.txt', 'train/y_train.txt'

##Read data from targeted files into the variables
## Read Activity files
dataActivityTest <- read.table(file.path("UCI HAR Dataset", "test", "Y_test.txt"), header = FALSE)
dataActivityTrain <- read.table(file.path("UCI HAR Dataset", "train", "Y_train.txt"), header = FALSE)
## Read Subject files
dataSubjectTrain <- read.table(file.path("UCI HAR Dataset", "train", "subject_train.txt"), header = FALSE)
dataSubjectTest <- read.table(file.path("UCI HAR Dataset", "test", "subject_test.txt"), header = FALSE)
## Read Features files
dataFeaturesTest <- read.table(file.path("UCI HAR Dataset", "test", "X_test.txt"), header = FALSE)
dataFeaturesTrain <- read.table(file.path("UCI HAR Dataset", "train", "X_train.txt"), header = FALSE)

## Look at the properties of the above variables
str(dataActivityTest)
str(dataActivityTrain)
str(dataSubjectTrain)
str(dataSubjectTest)
str(dataFeaturesTest)
str(dataFeaturesTrain)

## Merge the training and the test sets to create one data set
## 1. concatenate the data tables by rows
dataSubject <- rbind(dataSubjectTrain, dataSubjectTest)
dataActivity <- rbind(dataActivityTrain, dataActivityTest)
dataFeatures <- rbind(dataFeaturesTrain, dataFeaturesTest)
## 2. set names to variables
names(dataSubject) <- c("Subject")
names(dataActivity) <- c("Activity")
dataFeaturesNames <- read.table(file.path("UCI HAR Dataset", "features.txt"), header = FALSE)
names(dataFeatures) <- dataFeaturesNames$V2
## 3. Merge coloumns to obtain the data frame Data for all data
dataAll <- cbind(dataSubject, dataActivity)
Data <- cbind(dataFeatures, dataAll)

## Extract only the measurements on the mean and standard deviation for each measurement
## 1. Subset Name of the Features by measurements on the mean and standard deviation
## The above can be achieved by using Names of Features with "mean()" or "std()"
subdataFeaturesNames <- dataFeaturesNames$V2[grep("mean\\(\\) | std\\(\\)", dataFeaturesNames$V2)]
## 2. Subset the data frame (Data) by selected names of Features
selectedNames <- c(as.character(subdataFeaturesNames), "subject", "activity")
Data <- subset(Data, "select" == selectedNames)
str(Data)

## Name the activities in the data set with descriptive activity names()
## Read descriptive activity names from "activity_labels.txt"
activityLabels <- read.table(file.path("UCI HAR Dataset", "activity_labels.txt"), header = FALSE)
## Use descriptive activity names to factorize variables (activity) in the data frame (Data)
## Check
head(Data$activity, 30)
## Label the data set (names of features) with descriptive variable names
## replace prefix t by time
## replace Acc by Accelerometer
## replace Gyro by Gyroscope
## replace prefix f by frequency
## replace Mag by Magnitude
## replace BodyBody by Body
names(Data) <- gsub("^t", "time", names(Data))
names(Data) <- gsub("^f", "frequency", names(Data))
names(Data) <- gsub("^Acc", "Accelerometer", names(Data))
names(Data) <- gsub("^Gyro", "Gyroscope", names(Data))
names(Data) <- gsub("^Mag", "Magnitude", names(Data))
names(Data) <- gsub("^BodyBody", "Body", names(Data))
## Print the result to the console to check
names(Data)

## Create a second, independent tidy data set with the average of each activity and each subject
library(dplyr)
Data2 <- aggregate(Data, list(subject, activity), fun = mean)
Data2 <- Data2[order(Data2$subject, Data2$activity),]
write.table(Data2, file = "tidydata.txt", row.name = FALSE)



