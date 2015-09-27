Raw file is in zip format and located in "http://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip".

The attached r code, "run_analysis.r" assumes that raw file is downloaded, unzipped and saved in working directory. The data itself is "UCI HAR Dataset" directory.

The data is acceloremeter measurements of 30 subjects. Subject names are not known but they have id's changing from 1 to 30.
9 of the subcject are labeled as test group and rest of them as train group. Train and test group results are in different directories under "UCI HAR Dataset". 

Subjects are measured when they are performing 6 different activities. Activity id's and names are stored in "activity_labels.txt"

Accelorometer makes 561 different measurements. Name of these measurerements are stored in "features.txt". 

Measurements of accelometers are stored in "y_train.txt" and "y_test.txt" files. There are 561 columns in these files. Each column in these files correspond to one of 561 features. Each row is unique measurement of an activity and a subject. Subject id of each measurement is stored in "x_test.txt" and "x_train.txt" files. There are exactly same rows in x_test.txt and y_test.txt Subject id in the first row of x_text.txt corresponds to first measurement in y_test.txt... In general, subject id in the nth row of x_test.txt corresponds to nth measurement in y_test.txt. Likewise, subject id in the nth row of x_train.txt corresponds to nth measurement in y_train.txt.
           


#download file to working directory
#download.file(fileUrl, destfile = ".getdata-projectfiles-UCI HAR Dataset.zip", mode="wb")

# unzip the data to working directory
# the data is saved to 'UCI HAR Dataset' folder
#unzip(".getdata-projectfiles-UCI HAR Dataset.zip", exdir = ".")

#read the activity names from the activity_labels.txt
activity_labels <- read.table(file="./UCI HAR Dataset/activity_labels.txt")
           
#read the feature names from the features.txt
features <- read.table(file="./UCI HAR Dataset/features.txt")

#read results for test group from the X_test.txt
result.test <- read.table(file="./UCI HAR Dataset/test/X_test.txt")

#read acticity_labels for test group from the Y_test.txt
activity.test <- read.table(file="./UCI HAR Dataset/test/Y_test.txt")

#read subject id for test group from the subject_test.txt
subject.test <- read.table(file="./UCI HAR Dataset/test/subject_test.txt")

#read results for train group from the X_train.txt
result.train <- read.table(file="./UCI HAR Dataset/train/X_train.txt")

#read acticity_labels for train group from the Y_train.txt
activity.train <- read.table(file="./UCI HAR Dataset/train/Y_train.txt")

#read subject id for train group from the subject_train.txt
subject.train <- read.table(file="./UCI HAR Dataset/train/subject_train.txt")

#Update feature names to make them more readible
features$V2 <- gsub(pattern = "()",replace = "",features$V2, fixed = TRUE)
features$V2 <- gsub(pattern = ",",replace = "_and_",features$V2, fixed = TRUE)
features$V2 <- gsub(pattern = "-",replace = "_",features$V2, fixed = TRUE)

#Change the columns names of result.train and result.test datasets
#Columns correspond to feature names
names(result.test) <- features$V2
names(result.train) <- features$V2

#subcject and activity id's will be added to the result data both for 
#train and test set separately
result.test$activity <- activity.test$V1
result.test$subject <- subject.test$V1
result.train$activity <- activity.train$V1
result.train$subject <- subject.train$V1

#train and data sets will be merged
result <- rbind(result.test,result.train)

#rename the activity id's to activity lables
result$activity <- activity_labels$V2[result$activity]

#Find feature names about mean or std
mean_or_std <- unique(c(grep("mean",features$V2),grep("std",features$V2)))

#A subset of result with features mean or std and activity & subject names 
mean_or_std <- c(mean_or_std,c(562,563))
resultms <- result[,mean_or_std]

#sqldf package will be used to find the mean of resultms for each activity and subject
#SQL query is in the form of "select avg(colname1),avg(colname2)... from data.frame group by .."
#So the query is concatenated
q <- paste(names(resultms)[1:79], collapse = '),avg(')
qq <- paste("select subject, activity, avg(", q[1], ") from resultms group by subject, activity order by subject, activity")

library(sqldf)
#run the query
step5 <- sqldf(qq)

#save the query in a text file
write.table(step5,file = "./step5.txt",row.names = FALSE, sep = ",")


