#Merges the training and the test sets to create one data set.
#Set working directory
setwd("./getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset")
#Merge.test.files
test.subject_test<-read.table("./test/subject_test.txt")
test.X_test<-read.table("./test/X_test.txt")
features<-read.table("features.txt")
features.names<-as.character(as.vector(features[,2]))
colnames(test.X_test)<-features.names
test.y_test<-read.table("./test/y_test.txt")
test<-cbind(test.subject_test, test.y_test, test.X_test)
#Merge.train.files
train.subject_test<-read.table("./train/subject_train.txt")
train.X_train<-read.table("./train/X_train.txt")
colnames(train.X_train)<-features.names
train.y_train<-read.table("./train/y_train.txt")
train<-cbind(train.subject_test, train.y_train, train.X_train)
#Merge.train.n.test.objects
Data<-rbind(test, train)

#Extracts only the measurements on the mean and standard deviation for each measurement.
#grep.mean
mean.names<-grep("mean", names(Data))
#grep.standard.deviation
std.names<-grep("std", names(Data))
#grep.subject.n.activity
V1<-grep("V1$", names(Data))
#merge.needed.column.names
mean.std.names<-c(V1,mean.names,std.names)
#extract.needed.data
filtertData<-Data[,mean.std.names]

#Uses descriptive activity names to name the activities in the data set
activity_labels<-read.table("./activity_labels.txt")
filtertData$V1.1 <- factor(filtertData$V1.1,
                    levels = c(1:6),
                    labels = activity_labels[,2])

#Appropriately labels the data set with descriptive variable names.
filtertData<-rename(filtertData, c("V1.1"="activity"))
filtertData<-rename(filtertData, c("V1"="subject"))

names(filtertData)<-gsub("^t", "Time.", names(filtertData))
names(filtertData)<-gsub("^f", "Freq.", names(filtertData))

#From the data set in step 4, creates a second, 
#independent tidy data set with the average of each variable for each activity and each subject.

task<-group_by(filtertData, subject, activity)
summarise(task, )
