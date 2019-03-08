---
title: "Practical Machine Learning  Assignment"
author: "Anju Mandal"
date: "8 March 2019"
output: 
  html_document: 
    keep_md: yes
---

###Summary
####  Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. 
### Goal of the Assignment 
#### The goal of the assignment will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. The goal of this project is to predict the manner in which they did the execise using the "classe" variable in the training set. All other variables can be used as predictor.
####Use the prediction model to predict 20 different test cases.

###Loading the Data

```r
  trainingSet <- read.csv("./pml-training.csv",header=T,sep=",",na.strings=c("NA",""))
  testSet <- read.csv("./pml-testing.csv",header=T,sep=",",na.strings=c("NA",""))
  dim(trainingSet)
```

```
## [1] 19622   160
```

```r
  dim(testSet)
```

```
## [1]  20 160
```

```r
  names(trainingSet)
```

```
##   [1] "X"                        "user_name"               
##   [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
##   [5] "cvtd_timestamp"           "new_window"              
##   [7] "num_window"               "roll_belt"               
##   [9] "pitch_belt"               "yaw_belt"                
##  [11] "total_accel_belt"         "kurtosis_roll_belt"      
##  [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
##  [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
##  [17] "skewness_yaw_belt"        "max_roll_belt"           
##  [19] "max_picth_belt"           "max_yaw_belt"            
##  [21] "min_roll_belt"            "min_pitch_belt"          
##  [23] "min_yaw_belt"             "amplitude_roll_belt"     
##  [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
##  [27] "var_total_accel_belt"     "avg_roll_belt"           
##  [29] "stddev_roll_belt"         "var_roll_belt"           
##  [31] "avg_pitch_belt"           "stddev_pitch_belt"       
##  [33] "var_pitch_belt"           "avg_yaw_belt"            
##  [35] "stddev_yaw_belt"          "var_yaw_belt"            
##  [37] "gyros_belt_x"             "gyros_belt_y"            
##  [39] "gyros_belt_z"             "accel_belt_x"            
##  [41] "accel_belt_y"             "accel_belt_z"            
##  [43] "magnet_belt_x"            "magnet_belt_y"           
##  [45] "magnet_belt_z"            "roll_arm"                
##  [47] "pitch_arm"                "yaw_arm"                 
##  [49] "total_accel_arm"          "var_accel_arm"           
##  [51] "avg_roll_arm"             "stddev_roll_arm"         
##  [53] "var_roll_arm"             "avg_pitch_arm"           
##  [55] "stddev_pitch_arm"         "var_pitch_arm"           
##  [57] "avg_yaw_arm"              "stddev_yaw_arm"          
##  [59] "var_yaw_arm"              "gyros_arm_x"             
##  [61] "gyros_arm_y"              "gyros_arm_z"             
##  [63] "accel_arm_x"              "accel_arm_y"             
##  [65] "accel_arm_z"              "magnet_arm_x"            
##  [67] "magnet_arm_y"             "magnet_arm_z"            
##  [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
##  [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
##  [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
##  [75] "max_roll_arm"             "max_picth_arm"           
##  [77] "max_yaw_arm"              "min_roll_arm"            
##  [79] "min_pitch_arm"            "min_yaw_arm"             
##  [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
##  [83] "amplitude_yaw_arm"        "roll_dumbbell"           
##  [85] "pitch_dumbbell"           "yaw_dumbbell"            
##  [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
##  [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
##  [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
##  [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
##  [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
##  [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
##  [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
## [101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
## [103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
## [105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
## [107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
## [109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
## [111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
## [113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
## [115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
## [117] "accel_dumbbell_y"         "accel_dumbbell_z"        
## [119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
## [121] "magnet_dumbbell_z"        "roll_forearm"            
## [123] "pitch_forearm"            "yaw_forearm"             
## [125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
## [127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
## [129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
## [131] "max_roll_forearm"         "max_picth_forearm"       
## [133] "max_yaw_forearm"          "min_roll_forearm"        
## [135] "min_pitch_forearm"        "min_yaw_forearm"         
## [137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
## [139] "amplitude_yaw_forearm"    "total_accel_forearm"     
## [141] "var_accel_forearm"        "avg_roll_forearm"        
## [143] "stddev_roll_forearm"      "var_roll_forearm"        
## [145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
## [147] "var_pitch_forearm"        "avg_yaw_forearm"         
## [149] "stddev_yaw_forearm"       "var_yaw_forearm"         
## [151] "gyros_forearm_x"          "gyros_forearm_y"         
## [153] "gyros_forearm_z"          "accel_forearm_x"         
## [155] "accel_forearm_y"          "accel_forearm_z"         
## [157] "magnet_forearm_x"         "magnet_forearm_y"        
## [159] "magnet_forearm_z"         "classe"
```

###Data Partition
####Creating training data and validation data sets 

```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.5.2
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```r
library(lattice)
library(ggplot2)
library(rpart)
```

```
## Warning: package 'rpart' was built under R version 3.5.2
```

```r
library(randomForest)
```

```
## Warning: package 'randomForest' was built under R version 3.5.2
```

```
## randomForest 4.6-14
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

```r
library(ElemStatLearn)
```

```
## Warning: package 'ElemStatLearn' was built under R version 3.5.2
```

```r
library(corrplot)
```

```
## Warning: package 'corrplot' was built under R version 3.5.2
```

```
## corrplot 0.84 loaded
```

```r
library(e1071)
```

```
## Warning: package 'e1071' was built under R version 3.5.2
```

```r
set.seed(12345)
trainingSet <- trainingSet[,-1] # Remove the first column that represents a ID Row
inTrain = createDataPartition(trainingSet$classe, p=0.60, list=F)
training = trainingSet[inTrain,]
validating = trainingSet[-inTrain,]
```

###Data Cleaning
#### Data set must first be checked on possibility of columns without data to ensure models are applied properly.

####Remove all the columns that having less than 60% of data filled


```r
sum((colSums(!is.na(training[,-ncol(training)])) < 0.6*nrow(training)))
```

```
## [1] 100
```

```r
KeepCols <- c((colSums(!is.na(training[,-ncol(training)])) >= 0.6*nrow(training)))
training   <-  training[,KeepCols]
validating <- validating[,KeepCols]
```

###Modeling
#### In random forests, there is no need for cross-validation or a separate test set to get an unbiased estimate of the test set error. It is estimated internally, during the execution. Therefore, using the training of the model (Random Forest) for the training data set.


```r
library(randomForest)
md_rf <- randomForest(classe~.,data=training)
md_rf
```

```
## 
## Call:
##  randomForest(formula = classe ~ ., data = training) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 7
## 
##         OOB estimate of  error rate: 0.16%
## Confusion matrix:
##      A    B    C    D    E  class.error
## A 3347    1    0    0    0 0.0002986858
## B    2 2277    0    0    0 0.0008775779
## C    0    2 2051    1    0 0.0014605648
## D    0    0    8 1920    2 0.0051813472
## E    0    0    0    3 2162 0.0013856813
```

###Model Evaluation
####Verification of the variable importance measures as produced by random Forest is as follows:

```r
importance(md_rf)
```

```
##                      MeanDecreaseGini
## user_name                 100.8162157
## raw_timestamp_part_1      958.6118343
## raw_timestamp_part_2       10.2881748
## cvtd_timestamp           1371.6699625
## new_window                  0.1053097
## num_window                567.2182048
## roll_belt                 525.2605612
## pitch_belt                320.0654326
## yaw_belt                  349.1436458
## total_accel_belt          120.5532066
## gyros_belt_x               37.9104844
## gyros_belt_y               53.6794427
## gyros_belt_z              131.5540490
## accel_belt_x               57.8293070
## accel_belt_y               70.6818755
## accel_belt_z              187.5771409
## magnet_belt_x             105.0210890
## magnet_belt_y             196.0891149
## magnet_belt_z             182.1040734
## roll_arm                  120.0264534
## pitch_arm                  52.2963834
## yaw_arm                    80.5103902
## total_accel_arm            29.6745029
## gyros_arm_x                40.9909478
## gyros_arm_y                37.9648763
## gyros_arm_z                17.7528897
## accel_arm_x                94.5192706
## accel_arm_y                49.1525339
## accel_arm_z                40.7702017
## magnet_arm_x               83.6274502
## magnet_arm_y               74.4099454
## magnet_arm_z               55.4323942
## roll_dumbbell             189.3322920
## pitch_dumbbell             79.7279584
## yaw_dumbbell              107.6218076
## total_accel_dumbbell      119.6201310
## gyros_dumbbell_x           38.0307721
## gyros_dumbbell_y           90.3398451
## gyros_dumbbell_z           24.5299045
## accel_dumbbell_x          126.2919248
## accel_dumbbell_y          186.3219976
## accel_dumbbell_z          131.4645707
## magnet_dumbbell_x         228.8924569
## magnet_dumbbell_y         312.2371997
## magnet_dumbbell_z         296.2695125
## roll_forearm              239.6666018
## pitch_forearm             293.9674097
## yaw_forearm                53.8422674
## total_accel_forearm        38.1252480
## gyros_forearm_x            26.7054716
## gyros_forearm_y            38.5633264
## gyros_forearm_z            28.0449131
## accel_forearm_x           143.1409155
## accel_forearm_y            44.9892100
## accel_forearm_z            97.9494515
## magnet_forearm_x           78.5690383
## magnet_forearm_y           75.2136960
## magnet_forearm_z           96.5482375
```

#### Using confusion matrix to check the results

```r
confusionMatrix(predict(md_rf,newdata=validating[,-ncol(validating)]),validating$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2231    2    0    0    0
##          B    1 1516    2    0    0
##          C    0    0 1366    3    0
##          D    0    0    0 1282    2
##          E    0    0    0    1 1440
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9986          
##                  95% CI : (0.9975, 0.9993)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9982          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9996   0.9987   0.9985   0.9969   0.9986
## Specificity            0.9996   0.9995   0.9995   0.9997   0.9998
## Pos Pred Value         0.9991   0.9980   0.9978   0.9984   0.9993
## Neg Pred Value         0.9998   0.9997   0.9997   0.9994   0.9997
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2843   0.1932   0.1741   0.1634   0.1835
## Detection Prevalence   0.2846   0.1936   0.1745   0.1637   0.1837
## Balanced Accuracy      0.9996   0.9991   0.9990   0.9983   0.9992
```

####checking the accuracy of the model

```r
accuracy<-c(as.numeric(predict(md_rf,newdata=validating[,-ncol(validating)])==validating$classe))
accuracy<-sum(accuracy)*100/nrow(validating)
accuracy
```

```
## [1] 99.8598
```

####Accuracy of the model tested on validating set is 99.85% and the sample error is 0.15%. Thus Random Forest Model is the best fit in this case.

###Applying the Model to the Test data 
###  Testing loaded earlier needs to cleaned in similar way as training data 


```r
testSet <- testSet[,-1] # Remove the first column that represents a ID Row
testSet <- testSet[ ,KeepCols] # Keep the same columns of testing dataset
testSet <- testSet[,-ncol(testSet)] # Remove the problem ID
# Coerce testing dataset to same class and structure of training dataset 
testing <- rbind(training[100, -59] , testSet) 
# Apply the ID Row to row.names and 100 for dummy row from testing dataset 
row.names(testing) <- c(100, 1:20)
#Prediction with the Testing Dataset
predictions <- predict(md_rf,newdata=testing[-1,])
predictions
```

```
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
## Levels: A B C D E
```
