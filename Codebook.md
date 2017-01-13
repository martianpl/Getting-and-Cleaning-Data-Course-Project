Load the file into data

if(!file.exists(“./data”)){dir.create(“./data”)} fileUrl <- “https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip” download.file(fileUrl,destfile=“./data/Dataset.zip”)

unzip(zipfile=“./data/Dataset.zip”,exdir=“./data”) DF <- file.path(“./data” , “UCI HAR Dataset”) files<-list.files(DF, recursive=TRUE)

Read the acitivty files

ActivityTest <- read.table(file.path(DF, “test” , “Y_test.txt” ),header = FALSE) ActivityTrain <- read.table(file.path(DF, “train”, “Y_train.txt”),header = FALSE)

Read Subject files

SubjectTrain <- read.table(file.path(DF, “train”, “subject_train.txt”),header = FALSE) SubjectTest <- read.table(file.path(DF, “test” , “subject_test.txt”),header = FALSE)

Read features file

FeaturesTest <- read.table(file.path(DF, “test” , “X_test.txt” ),header = FALSE) FeaturesTrain <- read.table(file.path(DF, “train”, “X_train.txt”),header = FALSE)

Merge the data

#1. Concatenate

Subject <- rbind(SubjectTrain, SubjectTest) Activity<- rbind(ActivityTrain, ActivityTest) Features<- rbind(FeaturesTrain, FeaturesTest)

#2. Rename the headers

names(Subject)<-c(“subject”) names(Activity)<- c(“activity”) FeaturesNames <- read.table(file.path(DF, “features.txt”),head=FALSE) names(Features)<- FeaturesNames$V2

#3. Merge columns

Combine <- cbind(Subject, Activity) Data <- cbind(Features, Combine)

Extract only Mean and SD

#1. Subset name of features subFeaturesNames<-FeaturesNames$V2[grep(“mean 

|std 

”, FeaturesNames$V2)]

#2. Subset data frames by names of selected features selectedNames<-c(as.character(subFeaturesNames), “subject”, “activity” ) Data<-subset(Data,select=selectedNames)

Read descriptive activities

activityLabels <- read.table(file.path(DF, “activity_labels.txt”),header = FALSE)

#replace descriptive variable names with labels

names(Data)<-gsub(“t”, “time”, names(Data)) names(Data)<-gsub(“f”, “frequency”, names(Data)) names(Data)<-gsub(“Acc”, “Accelerometer”, names(Data)) names(Data)<-gsub(“Gyro”, “Gyroscope”, names(Data)) names(Data)<-gsub(“Mag”, “Magnitude”, names(Data)) names(Data)<-gsub(“BodyBody”, “Body”, names(Data))

Create second independant tidy data set

library(plyr); Data2<-aggregate(. ~subject + activity, Data, mean) Data2<-Data2[order(Data2$subject,Data2$activity),] write.table(Data2, file = “tidydata.txt”,row.name=FALSE)

Write table

write.table(Data2, file=“tidyData.txt”, row.name=FALSE, sep = “\t”)
