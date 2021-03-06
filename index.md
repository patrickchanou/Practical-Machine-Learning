  training <- read.csv("pml-training.csv", na.strings = c("NA", "#DIV/0!", ""))
  testing  <- read.csv("pml-testing.csv",  na.strings = c("NA", "#DIV/0!", ""))
# Reviewing the data needed for the project

  str(training, list.len=15)

# 'data.frame':    19622 obs. of  160 variables:
#  $ X                       : int  1 2 3 4 5 6 7 8 9 10 ...
#  $ user_name               : Factor w/ 6 levels "adelmo","carlitos",..: 2 2 2 2 2 2 2 2 2 2 ...
#  $ raw_timestamp_part_1    : int  1323084231 1323084231 1323084231 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 ...
#  $ raw_timestamp_part_2    : int  788290 808298 820366 120339 196328 304277 368296 440390 484323 484434 ...
#  $ cvtd_timestamp          : Factor w/ 20 levels "02/12/2011 13:32",..: 9 9 9 9 9 9 9 9 9 9 ...
#  $ new_window              : Factor w/ 2 levels "no","yes": 1 1 1 1 1 1 1 1 1 1 ...
#  $ num_window              : int  11 11 11 12 12 12 12 12 12 12 ...
#  $ roll_belt               : num  1.41 1.41 1.42 1.48 1.48 1.45 1.42 1.42 1.43 1.45 ...
#  $ pitch_belt              : num  8.07 8.07 8.07 8.05 8.07 8.06 8.09 8.13 8.16 8.17 ...
#  $ yaw_belt                : num  -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 ...
#  $ total_accel_belt        : int  3 3 3 3 3 3 3 3 3 3 ...
#  $ kurtosis_roll_belt      : num  NA NA NA NA NA NA NA NA NA NA ...
#  $ kurtosis_picth_belt     : num  NA NA NA NA NA NA NA NA NA NA ...
#  $ kurtosis_yaw_belt       : logi  NA NA NA NA NA NA ...
#  $ skewness_roll_belt      : num  NA NA NA NA NA NA NA NA NA NA ..
#   [list output truncated]

    table(training$classe)

# A    B    C    D    E 
# 5580 3797 3422 3216 3607
    prop.table(table(training$user_name, training$classe), 1)
#           
#                    A         B         C         D         E
#   adelmo   0.2993320 0.1993834 0.1927030 0.1323227 0.1762590
#   carlitos 0.2679949 0.2217224 0.1584190 0.1561697 0.1956941
#   charles  0.2542421 0.2106900 0.1524321 0.1815611 0.2010747
#   eurico   0.2817590 0.1928339 0.1592834 0.1895765 0.1765472
#   jeremy   0.3459730 0.1437390 0.1916520 0.1534392 0.1651969
#   pedro    0.2452107 0.1934866 0.1911877 0.1796935 0.1904215
     prop.table(table(training$classe))

#         A         B         C         D         E 
# 0.2843747 0.1935073 0.1743961 0.1638977 0.1838243

# Cleaning the data by removing columns 1 to 6, not required to do the predictions

   training <- training[, 7:160]
   testing  <- testing[, 7:160]

# and removing all data representaed by NA

   is_data  <- apply(!is.na(training), 2, sum) > 19621  # which is the number of observations
   training <- training[, is_data]
   testing  <- testing[, is_data]

   str(training, list.len=15)

# 'data.frame':    19622 obs. of  160 variables:
#  $ X                       : int  1 2 3 4 5 6 7 8 9 10 ...
#  $ user_name               : Factor w/ 6 levels "adelmo","carlitos",..: 2 2 2 2 2 2 2 2 2 2 ...
#  $ raw_timestamp_part_1    : int  1323084231 1323084231 1323084231 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 ...
#  $ raw_timestamp_part_2    : int  788290 808298 820366 120339 196328 304277 368296 440390 484323 484434 ...
#  $ cvtd_timestamp          : Factor w/ 20 levels "02/12/2011 13:32",..: 9 9 9 9 9 9 9 9 9 9 ...
#  $ new_window              : Factor w/ 2 levels "no","yes": 1 1 1 1 1 1 1 1 1 1 ...
#  $ num_window              : int  11 11 11 12 12 12 12 12 12 12 ...
#  $ roll_belt               : num  1.41 1.41 1.42 1.48 1.48 1.45 1.42 1.42 1.43 1.45 ...
#  $ pitch_belt              : num  8.07 8.07 8.07 8.05 8.07 8.06 8.09 8.13 8.16 8.17 ...
#  $ yaw_belt                : num  -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 ...
#  $ total_accel_belt        : int  3 3 3 3 3 3 3 3 3 3 ...
#  $ kurtosis_roll_belt      : num  NA NA NA NA NA NA NA NA NA NA ...
#  $ kurtosis_picth_belt     : num  NA NA NA NA NA NA NA NA NA NA ...
#  $ kurtosis_yaw_belt       : logi  NA NA NA NA NA NA ...
#  $ skewness_roll_belt      : num  NA NA NA NA NA NA NA NA NA NA ...
#   [list output truncated]

   table(training$classe)

# 
# A    B    C    D    E 
# 5580 3797 3422 3216 3607
   prop.table(table(training$user_name, training$classe), 1)
#           
#                    A         B         C         D         E
#   adelmo   0.2993320 0.1993834 0.1927030 0.1323227 0.1762590
#   carlitos 0.2679949 0.2217224 0.1584190 0.1561697 0.1956941
#   charles  0.2542421 0.2106900 0.1524321 0.1815611 0.2010747
#   eurico   0.2817590 0.1928339 0.1592834 0.1895765 0.1765472
#   jeremy   0.3459730 0.1437390 0.1916520 0.1534392 0.1651969
#   pedro    0.2452107 0.1934866 0.1911877 0.1796935 0.1904215
     prop.table(table(training$classe))
 
#         A         B         C         D         E 
# 0.2843747 0.1935073 0.1743961 0.1638977 0.1838243

# Cleaning the data by removing columns 1 to 6, not required to do the predictions

 training <- training[, 7:160]
 testing  <- testing[, 7:160]

# and removing all data representaed by NA

 is_data  <- apply(!is.na(training), 2, sum) > 19621  # which is the number of observations
 training <- training[, is_data]
 testing  <- testing[, is_data]

# Let's split the training set into two as to validate the predictions.
# We subsample 60% of the set for training purposes and 40%  for testing, evaluation and accuracy measurement.

 library(caret)

# Loading required package: lattice
# Loading required package: ggplot2

 set.seed(3141592)
 inTrain <- createDataPartition(y=training$classe, p=0.60, list=FALSE)
 train1  <- training[inTrain,]
 train2  <- training[-inTrain,]
 dim(train1)

# [1] 11776    54

 dim(train2)

# [1] 7846   54

# Install the caret package 
# train1 is the training data set (it contains 11776 observations, or about 60% of the entire training data set
# train2 is the testing data set (it contains 7846 observations, or about 40% of the entire training data set).
# The dataset train2 will never be used 


 nzv_cols <- nearZeroVar(train1)
 if(length(nzv_cols) > 0) {
  train1 <- train1[, -nzv_cols]
  train2 <- train2[, -nzv_cols]
 }
 dim(train1)

# [1] 11776    54

dim(train2)

# [1] 7846   54
# This step didn’t do anything as the earlier removal of NA was sufficient to clean the data. We are satisfied that we 
# now have 53 clean covariates to build a model for classe (which is the 54th column of the data set).


# Data Manipulation
# Becasue we have 53 covariates, we will assess their relative importance using the output of a quick Random
# Install the randomForest package

 set.seed(3141592)
 fitModel <- randomForest(classe~., data=train1, importance=TRUE, ntree=100)
 varImpPlot(fitModel)
 
# Using the graphs above, we select the top 10 variables that we’ll use for model building. If the 


 correl = cor(train1[,c("yaw_belt","roll_belt","num_window","pitch_belt","magnet_dumbbell_z","magnet_dumbbell_y","pitch_forearm","accel_dumbbell_y","roll_arm","roll_forearm")])
 diag(correl) <- 0
 which(abs(correl)>0.75, arr.ind=TRUE)

#           row col
# roll_belt   2   1
# yaw_belt    1   2

# We coould face challenges with roll_belt and yaw_belt because of  correlation (above 75%) with each other:

 cor(train1$roll_belt, train1$yaw_belt)

# [1] 0.8152349


 qplot(roll_belt, magnet_dumbbell_y, colour=classe, data=train1)

# This graph suggests that we could probably categorize the data into groups based on roll_belt values.

# Install the rpart.plot package

 library(rpart.plot)

# Loading required package: rpart

 fitModel <- rpart(classe~., data=train1, method="class")
 prp(fitModel)
 
# Because of Random Forest algorithm,  we will not investigate tree classifiers


# Modeling
# Creating our model.
# Using a Random Forest algorithm, using the train() function from the caret package.
# Using 9 variables out of the 53 as model parameters. ch_belt, 

 set.seed(3141592)
 fitModel <- train(classe~roll_belt+num_window+pitch_belt+magnet_dumbbell_y+magnet_dumbbell_z+pitch_forearm+accel_dumbbell_y+roll_arm+roll_forearm,
                  data=train1,
                  method="rf",
                  trControl=trainControl(method="cv",number=2),
                  prox=TRUE,
                  verbose=TRUE,
                  allowParallel=TRUE)



 saveRDS(fitModel, "modelRF.Rds")

# We will  use the tree later

 fitModel <- readRDS("modelRF.Rds")


# How accurate is our model?
# caret’s confusionMatrix() function can be used on on train2 (the test set) to get an idea of the accuracy:

 predictions <- predict(fitModel, newdata=train2)
 confusionMat <- confusionMatrix(predictions, train2$classe)
 confusionMat

# Confusion Matrix and Statistics
# 
#           Reference
# Prediction    A    B    C    D    E
#          A 2231    2    0    0    0
#          B    1 1512    0    0    1
#          C    0    3 1368    6    1
#          D    0    1    0 1280    3
#          E    0    0    0    0 1437
# 
# Overall Statistics
#                                           
#                Accuracy : 0.9977          
#                  95% CI : (0.9964, 0.9986)
#     No Information Rate : 0.2845          
#     P-Value [Acc > NIR] : < 2.2e-16       
#                                           
#                   Kappa : 0.9971          
#  Mcnemar's Test P-Value : NA              
# 
# Statistics by Class:
# 
#                      Class: A Class: B Class: C Class: D Class: E
# Sensitivity            0.9996   0.9960   1.0000   0.9953   0.9965
# Specificity            0.9996   0.9997   0.9985   0.9994   1.0000
# Pos Pred Value         0.9991   0.9987   0.9927   0.9969   1.0000
# Neg Pred Value         0.9998   0.9991   1.0000   0.9991   0.9992
# Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
# Detection Rate         0.2843   0.1927   0.1744   0.1631   0.1832
# Detection Prevalence   0.2846   0.1930   0.1756   0.1637   0.1832
# Balanced Accuracy      0.9996   0.9979   0.9992   0.9974   0.9983

# An accuracy of 99.77% is magnificent 

# Estimating the out-of-sample error rate


 missClass = function(values, predicted) {
  sum(predicted != values) / length(values)
 }
 OOS_errRate = missClass(train2$classe, predictions)
 OOS_errRate

# [1] 0.002294163
# Our out-of-sample error rate is 0.23%.
