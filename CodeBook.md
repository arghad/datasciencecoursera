Cook Book for Run Analysis for the course "Getting and Cleaning Data"
========================================================
```{r Code}
    ## 0. Preparation of data before start merging
    # +++++++++++++++++++++++++++++++++++++++++++++++
    ## Read the testing data set from test dirctory    
    # Read the measurements of test data set
    X_test <- read.table(paste(directory, "/test/X_test.txt", sep=""), sep="")
    
    # Read the activities of test data set
    Y_test <- read.table(paste(directory, "/test/Y_test.txt", sep=""), sep="")
    
    # Read the subject of test data set
    subject_test <- read.table(paste(directory, "/test/subject_test.txt", sep=""), sep="")
    
    # merge the data into one test data set
    test_set <- cbind(X_test, Y_test, subject_test)
    
    # +++++++++++++++++++++++++++++++++++++++++++++++++
    ## Read the training data set from test dirctory    
    # Read the measurements of train data set
    X_train <- read.table(paste(directory, "/train/X_train.txt", sep=""), sep="")
    
    # Read the activities of train data set
    Y_train <- read.table(paste(directory, "/train/Y_train.txt", sep=""), sep="")
    
    # Read the subject of train data set
    subject_train <- read.table(paste(directory, "/train/subject_train.txt", sep=""), sep="")
    
    # merge the data into one train data set
    train_set <- cbind(X_train, Y_train, subject_train)
    
    
    # ***********************************************
    ## 1. Merges the training and the test sets to create one data set
    merged_set <- rbind(test_set, train_set)
    
    
    # ***********************************************
    ## 2. Extracts only the measurements on the mean and standard deviation for each measurement
    featuresNames <- read.table(paste(directory, "/features.txt", sep=""), sep="")
    featuresNames <- featuresNames[,"V2"]
    columnNames <- c(as.character(featuresNames), "Activity", "Subject")
    colnames(merged_set) <- columnNames
    merged_set_updated <- merged_set[,columnNames[grep("mean[()]|std[()]|Activity|Subject", columnNames)]]
    
    # ***********************************************
    ## 3. Uses descriptive activity names to name the activities in the data set
    activity_names <- c("WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS","SITTING", "STANDING","LAYING")    
    
    # ***********************************************
    ## 4. Appropriately labels the data set with descriptive activity names
    merged_set_updated$activity_name <- activity_names[merged_set_updated$Activity]
    
    
    # ***********************************************
    ## 5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.
    dataMelt <- melt(merged_set_updated,id=c("activity_name","Activity","Subject"),measure.vars=columnNames[grep("mean[()]|std[()]", columnNames)])
    
    second_tidy_data <- aggregate( formula = dataMelt$value~dataMelt$activity_name+dataMelt$Subject+dataMelt$variable, 
               data = dataMelt$variable,
               FUN = mean )
    
    write.table(second_tidy_data, "second_tidy_data.txt", sep=",")
		
	```