Code Book

1. Downloading and loading data
		Downloads the UCI HAR zip file if it doesn't exist
		Reads the activity labels to activityLabels
		Reads the column names of data (a.k.a. features) to Features
		Reads the test data.frame to ActivityTest 
		Reads the trainning data.frame to ActivityTrain

2. Manipulating data

Merges test data and trainning data to a new data set, Data
Indentifies the mean and std columns (plus Activity and Subject) to subFeatures
Extracts a new data.frame (called Data2) with only those columns from subFeatures.

