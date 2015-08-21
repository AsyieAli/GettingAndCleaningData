##Getting and Cleaning Data Coursera Course Project.
###The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

##Read Dataset Online.

###onlineFile <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

##Download the file

###download.file(onlineFile, "./UCI-HAR-dataset.zip", method="auto")

##Unzip and create directory automatically

###unzip("./UCI-HAR-dataset.zip")

##QUESTION 1: Merges the training and the test sets to create one data set.

##Read features column

###features <- read.table("./UCI HAR Dataset/features.txt")

##Read test.x

###test.x <- read.table("./UCI HAR Dataset/test/X_test.txt", col.names=features[,2])

##Read train.x

###train.x <- read.table("./UCI HAR Dataset/train/X_train.txt", col.names=features[,2]) X <- rbind(test.x, train.x)

##QUESTION 2: Extracts only the measurements on the mean and standard deviation for each measurement.

##Filter features which has column mean and std

###features <- features[grep("(mean|std)\(", features[,2]),]

##Add both to mean_and_std

###mean_and_std <- X[,features[,1]]

##QUESTION 3: Uses descriptive activity names to name the activities in the data set

##Read test.y

###test.y <- read.table("./UCI HAR Dataset/test/y_test.txt", col.names = c('activity'))

##Read train.y

###train.y <- read.table("./UCI HAR Dataset/train/y_train.txt", col.names = c('activity'))

##Bind to y

###y <- rbind(test.y, train.y)

##Read labels

###labels <- read.table("./UCI HAR Dataset/activity_labels.txt")

##Map activity to code

###for (i in 1:nrow(labels)) { code <- as.numeric(labels[i, 1]) name <- as.character(labels[i, 2]) y[y$activity == code, ] <- name }

##QUESTION 4: Appropriately labels the data set with descriptive variable names.

##Read labels.x

###labels.x <- cbind(y, X)

##Bind to labels.mean_and_std

###labels.mean_and_std <- cbind(y, mean_and_std)

##QUESTION 5: From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

##Test.subject

###test.subject <- read.table("./UCI HAR Dataset/test/subject_test.txt", col.names = c('subject'))

##Train.subject

###train.subject <- read.table("./UCI HAR Dataset/train/subject_train.txt", col.names = c('subject'))

##Bind to subject

###subject <- rbind(test.subject, train.subject)

##Get average

###averages <- aggregate(X, by = list(activity = y[,1], subject = subject[,1]), mean) if (file.exists("./result.txt")){ file.remove("./result.txt") }

##Write to text

###write.csv(averages, file="./result.txt", row.names=FALSE)