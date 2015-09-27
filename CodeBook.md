Raw file is in zip format and located in "http://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip".

The attached r code, "run_analysis.r" assumes that raw file is downloaded, unzipped and saved in working directory. The data itself is "UCI HAR Dataset" directory.

The data is acceloremeter measurements of 30 subjects. Subject names are not known but they have id's changing from 1 to 30.
9 of the subcject are labeled as test group and rest of them as train group. Train and test group results are in different directories under "UCI HAR Dataset". 

Subjects are measured when they are performing 6 different activities. Activity id's and names are stored in "activity_labels.txt"

Accelorometer makes 561 different measurements. Name of these measurerements are stored in "features.txt". 

Measurements of accelometers are stored in "x_train.txt" and "x_test.txt" files. There are 561 columns in these files. Each column in these files correspond to one of 561 features. Each row is unique measurement of an activity and a subject. 

Feature id of each measurement is stored in "y_test.txt" and "y_train.txt" files. There are exactly same rows in x_test.txt and y_test.txt. Feature id in the first row of y_text.txt corresponds to first measurement in x_test.txt... In general, feature id in the nth row of y_test.txt corresponds to nth measurement in x_test.txt. Likewise, feature id in the nth row of y_train.txt corresponds to nth measurement in x_train.txt.

Subject id of each measurement is stored in "subject_test.txt" and "subject_train.txt" files. There are exactly same rows in subject_test.txt and x_test.txt. Subject  id in the first row of subject_text.txt corresponds to first measurement in x_test.txt... In general, subject id in the nth row of subject_test.txt corresponds to nth measurement in x_test.txt. Likewise, subject id in the nth row of subject_train.txt corresponds to nth measurement in x_train.txt.

Inertial signals are ignored in this study.

1) read the activity names from the activity_labels.txt. Activity id 's will be replaced with activity names.
2)read the feature names from the features.txt. Column names of each result set will ve replaced by feature names.
3)read results for test group from the X_test.txt
4)read acticity_labels for test group from the Y_test.txt
5)read subject id for test group from the subject_test.txt
6)read results for train group from the X_train.txt
7)read acticity_labels for train group from the Y_train.txt
8)read subject id for train group from the subject_train.txt
9)Update feature names to make them more readible (replace parantheses, commas and minuses)
10)Change the columns names of result.train and result.test datasets to feature names
11)Addsubcject and activity id to result data both for train and test set separately
12)merge train and data sets
13)rename the activity id's to activity lables
14)Find feature names about mean or std and retrieve a subset of only those columns
15)sqldf package will be used to find the mean of resultms for each activity and subject. SQL query is in the form of "select avg(colname1),avg(colname2)... from data.frame group by ..". So the query is concatenated
16)step5 is retrieved by sql query. It gives the average of selected measures (measures about mean and standard deviation) for aeach activity and subject. 
17)results are saved to step5.txt in working directory


