1) making run_analysis.R script, downloading dataset -> folder called UCI HAR Dataset

2) data -> variables

features <- features.txt
activities <- activity_labels.txt
subject_test <- test/subject_test.txt
x_test <- test/X_test.txt
y_test <- test/y_test.txt
subject_train <- test/subject_train.txt
x_train <- test/X_train.txt
y_train <- test/y_train.txt


####################################################################script#################################

features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
> activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
> subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
> x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
> y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
> subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
> x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
> y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")
> 
> 
> 
> X <- rbind(x_train, x_test)
> Y <- rbind(y_train, y_test)
> Subject <- rbind(subject_train, subject_test)
> Merged_Data <- cbind(Subject, Y, X)
> TidyData <- Merged_Data %>% select(subject, code, contains("mean"), contains("std"))
> TidyData$code <- activities[TidyData$code, 2]
> 
> 
> names(TidyData)[2] = "activity"
> names(TidyData)<-gsub("Acc", "Accelerometer", names(TidyData))
> names(TidyData)<-gsub("Gyro", "Gyroscope", names(TidyData))
> 
> names(TidyData)<-gsub("BodyBody", "Body", names(TidyData))
> names(TidyData)<-gsub("Mag", "Magnitude", names(TidyData))
> names(TidyData)<-gsub("^t", "Time", names(TidyData))
> names(TidyData)<-gsub("^f", "Frequency", names(TidyData))
> names(TidyData)<-gsub("tBody", "TimeBody", names(TidyData))
> names(TidyData)<-gsub("-mean()", "Mean", names(TidyData), ignore.case = TRUE)
> names(TidyData)<-gsub("-std()", "STD", names(TidyData), ignore.case = TRUE)
> names(TidyData)<-gsub("-freq()", "Frequency", names(TidyData), ignore.case = TRUE)
> names(TidyData)<-gsub("angle", "Angle", names(TidyData))
> names(TidyData)<-gsub("gravity", "Gravity", names(TidyData))
> FinalData <- TidyData %>%
+   group_by(subject, activity) %>%
+   summarise_all(funs(mean))
>list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
This warning is displayed once every 8 hours.
Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated. 
> write.table(FinalData, "FinalData.txt", row.name=FALSE)
> str(FinalData)
> FinalData
