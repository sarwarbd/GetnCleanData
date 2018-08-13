# GetnCleanData
Get and Clean Data Project assignment



## Download and unzip data
download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip","D:/Training/Dataset.zip")

unzip("D:/Training/Dataset.zip", list=TRUE)

dir.create("D:/Training/Dataset")

unzip("D:/Training/Dataset.zip", exdir="D:/Training/Dataset")

## Load dimension data
activity_label <- read.table("D:/Training/Dataset/UCI HAR Dataset/activity_labels.txt")

feature <- read.table("D:/Training/Dataset/UCI HAR Dataset/features.txt")

## Load train data
train_x <- read.table("D:/Training/Dataset/UCI HAR Dataset/train/X_train.txt")

train_y <- read.table("D:/Training/Dataset/UCI HAR Dataset/train/y_train.txt")

train_sub <- read.table("D:/Training/Dataset/UCI HAR Dataset/train/subject_train.txt")

## Assign column names
colnames(train_x) <- feature$V2

## Assign column names
train_x <- cbind(train_sub,train_x)

## Add subs info and activity info and update column name
train_x <- cbind(train_sub,train_x)

colnames(train_x)[1] <- "sub_id"

train_x <- cbind(train_x,train_y)

colnames(train_x)[563] <- "activity"

train_x <- merge(train_x,activity_label, by.x="activity", by.y="V1")

colnames(train_x)[564] <- "activity_label"


## Load Test data
test_x <- read.table("D:/Training/Dataset/UCI HAR Dataset/test/X_test.txt")

test_y <- read.table("D:/Training/Dataset/UCI HAR Dataset/test/y_test.txt")

test_sub <- read.table("D:/Training/Dataset/UCI HAR Dataset/test/subject_test.txt")

colnames(test_x) <- feature$V2

test_x <- cbind(test_sub,test_x)

colnames(test_x)[1] <- "sub_id"

test_x <- cbind(test_x,test_y)

colnames(test_x)[563] <- "activity"

test_x <- merge(test_x,activity_label, by.x="activity", by.y="V1")

colnames(test_x)[564] <- "activity_label"

## Combine Training & Test data
full_set <- rbind(train_x,test_x)

write.csv(full_set,file="D:/Training/dataset-full.csv")


## Extract columns for mean & SD
col_mean_sd <-  colnames(full_set)[grepl("mean\\()",colnames(full_set)) | grepl("std\\()",colnames(full_set)) ]

mean_sd_set <- full_set [, col_mean_sd] 

set_id <- full_set [, c("sub_id","activity","activity_label")]

## Take mean per subs per activity

cdata <- aggregate(cbind(mean_sd_set,set_id)[col_mean_sd], by = set_id[c("sub_id","activity","activity_label")], FUN = mean)

write.csv(cdata,file="D:/Training/dataset-summary.csv")

