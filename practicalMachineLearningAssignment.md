# Practical Machine Learning Project

## Introduction
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset).

## Preparation
This project is compiled and executed online over RStudio Cloud https://rstudio.cloud/ IDE. You may execute it on any other environment that support R Programming.

## Getting Data
1. Download the training dataset https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv and test dataset https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv.
2. Using RStudio Cloud IDE, upload the same datasets to the server.

## Exploration
Import the datasets
```sh
> training <- read.csv("pml-training.csv", na.strings = c("NA", ""))
> testing = read.csv("pml-testing.csv", na.strings = c("NA", ""))
> dim(training)
[1] 19622   160
> dim(testing)
[1]  20 160
```

There are 19622 records, 160 features in training datasets. 20 records in testing datasets. Let's check the first 10 rows of training data.
```sh
> head(training, n=10)
  X user_name raw_timestamp_part_1 raw_timestamp_part_2   cvtd_timestamp new_window num_window roll_belt
1 1  carlitos           1323084231               788290 05/12/2011 11:23         no         11      1.41
2 2  carlitos           1323084231               808298 05/12/2011 11:23         no         11      1.41
3 3  carlitos           1323084231               820366 05/12/2011 11:23         no         11      1.42
4 4  carlitos           1323084232               120339 05/12/2011 11:23         no         12      1.48
5 5  carlitos           1323084232               196328 05/12/2011 11:23         no         12      1.48
6 6  carlitos           1323084232               304277 05/12/2011 11:23         no         12      1.45
  pitch_belt yaw_belt total_accel_belt kurtosis_roll_belt kurtosis_picth_belt kurtosis_yaw_belt
1       8.07    -94.4                3               <NA>                <NA>              <NA>
2       8.07    -94.4                3               <NA>                <NA>              <NA>
3       8.07    -94.4                3               <NA>                <NA>              <NA>
4       8.05    -94.4                3               <NA>                <NA>              <NA>
5       8.07    -94.4                3               <NA>                <NA>              <NA>
6       8.06    -94.4                3               <NA>                <NA>              <NA>
  skewness_roll_belt skewness_roll_belt.1 skewness_yaw_belt max_roll_belt max_picth_belt max_yaw_belt
1               <NA>                 <NA>              <NA>            NA             NA         <NA>
2               <NA>                 <NA>              <NA>            NA             NA         <NA>
3               <NA>                 <NA>              <NA>            NA             NA         <NA>
4               <NA>                 <NA>              <NA>            NA             NA         <NA>
5               <NA>                 <NA>              <NA>            NA             NA         <NA>
6               <NA>                 <NA>              <NA>            NA             NA         <NA>
  min_roll_belt min_pitch_belt min_yaw_belt amplitude_roll_belt amplitude_pitch_belt amplitude_yaw_belt
1            NA             NA         <NA>                  NA                   NA               <NA>
2            NA             NA         <NA>                  NA                   NA               <NA>
3            NA             NA         <NA>                  NA                   NA               <NA>
4            NA             NA         <NA>                  NA                   NA               <NA>
5            NA             NA         <NA>                  NA                   NA               <NA>
6            NA             NA         <NA>                  NA                   NA               <NA>
  var_total_accel_belt avg_roll_belt stddev_roll_belt var_roll_belt avg_pitch_belt stddev_pitch_belt
1                   NA            NA               NA            NA             NA                NA
2                   NA            NA               NA            NA             NA                NA
3                   NA            NA               NA            NA             NA                NA
4                   NA            NA               NA            NA             NA                NA
5                   NA            NA               NA            NA             NA                NA
6                   NA            NA               NA            NA             NA                NA
  var_pitch_belt avg_yaw_belt stddev_yaw_belt var_yaw_belt gyros_belt_x gyros_belt_y gyros_belt_z accel_belt_x
1             NA           NA              NA           NA         0.00         0.00        -0.02          -21
2             NA           NA              NA           NA         0.02         0.00        -0.02          -22
3             NA           NA              NA           NA         0.00         0.00        -0.02          -20
4             NA           NA              NA           NA         0.02         0.00        -0.03          -22
5             NA           NA              NA           NA         0.02         0.02        -0.02          -21
6             NA           NA              NA           NA         0.02         0.00        -0.02          -21
  accel_belt_y accel_belt_z magnet_belt_x magnet_belt_y magnet_belt_z roll_arm pitch_arm yaw_arm total_accel_arm
1            4           22            -3           599          -313     -128      22.5    -161              34
2            4           22            -7           608          -311     -128      22.5    -161              34
3            5           23            -2           600          -305     -128      22.5    -161              34
4            3           21            -6           604          -310     -128      22.1    -161              34
5            2           24            -6           600          -302     -128      22.1    -161              34
6            4           21             0           603          -312     -128      22.0    -161              34
  var_accel_arm avg_roll_arm stddev_roll_arm var_roll_arm avg_pitch_arm stddev_pitch_arm var_pitch_arm
1            NA           NA              NA           NA            NA               NA            NA
2            NA           NA              NA           NA            NA               NA            NA
3            NA           NA              NA           NA            NA               NA            NA
4            NA           NA              NA           NA            NA               NA            NA
5            NA           NA              NA           NA            NA               NA            NA
6            NA           NA              NA           NA            NA               NA            NA
  avg_yaw_arm stddev_yaw_arm var_yaw_arm gyros_arm_x gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y accel_arm_z
1          NA             NA          NA        0.00        0.00       -0.02        -288         109        -123
2          NA             NA          NA        0.02       -0.02       -0.02        -290         110        -125
3          NA             NA          NA        0.02       -0.02       -0.02        -289         110        -126
4          NA             NA          NA        0.02       -0.03        0.02        -289         111        -123
5          NA             NA          NA        0.00       -0.03        0.00        -289         111        -123
6          NA             NA          NA        0.02       -0.03        0.00        -289         111        -122
  magnet_arm_x magnet_arm_y magnet_arm_z kurtosis_roll_arm kurtosis_picth_arm kurtosis_yaw_arm skewness_roll_arm
1         -368          337          516              <NA>               <NA>             <NA>              <NA>
2         -369          337          513              <NA>               <NA>             <NA>              <NA>
3         -368          344          513              <NA>               <NA>             <NA>              <NA>
4         -372          344          512              <NA>               <NA>             <NA>              <NA>
5         -374          337          506              <NA>               <NA>             <NA>              <NA>
6         -369          342          513              <NA>               <NA>             <NA>              <NA>
  skewness_pitch_arm skewness_yaw_arm max_roll_arm max_picth_arm max_yaw_arm min_roll_arm min_pitch_arm
1               <NA>             <NA>           NA            NA          NA           NA            NA
2               <NA>             <NA>           NA            NA          NA           NA            NA
3               <NA>             <NA>           NA            NA          NA           NA            NA
4               <NA>             <NA>           NA            NA          NA           NA            NA
5               <NA>             <NA>           NA            NA          NA           NA            NA
6               <NA>             <NA>           NA            NA          NA           NA            NA
  min_yaw_arm amplitude_roll_arm amplitude_pitch_arm amplitude_yaw_arm roll_dumbbell pitch_dumbbell yaw_dumbbell
1          NA                 NA                  NA                NA      13.05217      -70.49400    -84.87394
2          NA                 NA                  NA                NA      13.13074      -70.63751    -84.71065
3          NA                 NA                  NA                NA      12.85075      -70.27812    -85.14078
4          NA                 NA                  NA                NA      13.43120      -70.39379    -84.87363
5          NA                 NA                  NA                NA      13.37872      -70.42856    -84.85306
6          NA                 NA                  NA                NA      13.38246      -70.81759    -84.46500
  kurtosis_roll_dumbbell kurtosis_picth_dumbbell kurtosis_yaw_dumbbell skewness_roll_dumbbell
1                   <NA>                    <NA>                  <NA>                   <NA>
2                   <NA>                    <NA>                  <NA>                   <NA>
3                   <NA>                    <NA>                  <NA>                   <NA>
4                   <NA>                    <NA>                  <NA>                   <NA>
5                   <NA>                    <NA>                  <NA>                   <NA>
6                   <NA>                    <NA>                  <NA>                   <NA>
  skewness_pitch_dumbbell skewness_yaw_dumbbell max_roll_dumbbell max_picth_dumbbell max_yaw_dumbbell
1                    <NA>                  <NA>                NA                 NA             <NA>
2                    <NA>                  <NA>                NA                 NA             <NA>
3                    <NA>                  <NA>                NA                 NA             <NA>
4                    <NA>                  <NA>                NA                 NA             <NA>
5                    <NA>                  <NA>                NA                 NA             <NA>
6                    <NA>                  <NA>                NA                 NA             <NA>
  min_roll_dumbbell min_pitch_dumbbell min_yaw_dumbbell amplitude_roll_dumbbell amplitude_pitch_dumbbell
1                NA                 NA             <NA>                      NA                       NA
2                NA                 NA             <NA>                      NA                       NA
3                NA                 NA             <NA>                      NA                       NA
4                NA                 NA             <NA>                      NA                       NA
5                NA                 NA             <NA>                      NA                       NA
6                NA                 NA             <NA>                      NA                       NA
  amplitude_yaw_dumbbell total_accel_dumbbell var_accel_dumbbell avg_roll_dumbbell stddev_roll_dumbbell
1                   <NA>                   37                 NA                NA                   NA
2                   <NA>                   37                 NA                NA                   NA
3                   <NA>                   37                 NA                NA                   NA
4                   <NA>                   37                 NA                NA                   NA
5                   <NA>                   37                 NA                NA                   NA
6                   <NA>                   37                 NA                NA                   NA
  var_roll_dumbbell avg_pitch_dumbbell stddev_pitch_dumbbell var_pitch_dumbbell avg_yaw_dumbbell
1                NA                 NA                    NA                 NA               NA
2                NA                 NA                    NA                 NA               NA
3                NA                 NA                    NA                 NA               NA
4                NA                 NA                    NA                 NA               NA
5                NA                 NA                    NA                 NA               NA
6                NA                 NA                    NA                 NA               NA
  stddev_yaw_dumbbell var_yaw_dumbbell gyros_dumbbell_x gyros_dumbbell_y gyros_dumbbell_z accel_dumbbell_x
1                  NA               NA                0            -0.02             0.00             -234
2                  NA               NA                0            -0.02             0.00             -233
3                  NA               NA                0            -0.02             0.00             -232
4                  NA               NA                0            -0.02            -0.02             -232
5                  NA               NA                0            -0.02             0.00             -233
6                  NA               NA                0            -0.02             0.00             -234
  accel_dumbbell_y accel_dumbbell_z magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z roll_forearm
1               47             -271              -559               293               -65         28.4
2               47             -269              -555               296               -64         28.3
3               46             -270              -561               298               -63         28.3
4               48             -269              -552               303               -60         28.1
5               48             -270              -554               292               -68         28.0
6               48             -269              -558               294               -66         27.9
  pitch_forearm yaw_forearm kurtosis_roll_forearm kurtosis_picth_forearm kurtosis_yaw_forearm
1         -63.9        -153                  <NA>                   <NA>                 <NA>
2         -63.9        -153                  <NA>                   <NA>                 <NA>
3         -63.9        -152                  <NA>                   <NA>                 <NA>
4         -63.9        -152                  <NA>                   <NA>                 <NA>
5         -63.9        -152                  <NA>                   <NA>                 <NA>
6         -63.9        -152                  <NA>                   <NA>                 <NA>
  skewness_roll_forearm skewness_pitch_forearm skewness_yaw_forearm max_roll_forearm max_picth_forearm
1                  <NA>                   <NA>                 <NA>               NA                NA
2                  <NA>                   <NA>                 <NA>               NA                NA
3                  <NA>                   <NA>                 <NA>               NA                NA
4                  <NA>                   <NA>                 <NA>               NA                NA
5                  <NA>                   <NA>                 <NA>               NA                NA
6                  <NA>                   <NA>                 <NA>               NA                NA
  max_yaw_forearm min_roll_forearm min_pitch_forearm min_yaw_forearm amplitude_roll_forearm
1            <NA>               NA                NA            <NA>                     NA
2            <NA>               NA                NA            <NA>                     NA
3            <NA>               NA                NA            <NA>                     NA
4            <NA>               NA                NA            <NA>                     NA
5            <NA>               NA                NA            <NA>                     NA
6            <NA>               NA                NA            <NA>                     NA
  amplitude_pitch_forearm amplitude_yaw_forearm total_accel_forearm var_accel_forearm avg_roll_forearm
1                      NA                  <NA>                  36                NA               NA
2                      NA                  <NA>                  36                NA               NA
3                      NA                  <NA>                  36                NA               NA
4                      NA                  <NA>                  36                NA               NA
5                      NA                  <NA>                  36                NA               NA
6                      NA                  <NA>                  36                NA               NA
  stddev_roll_forearm var_roll_forearm avg_pitch_forearm stddev_pitch_forearm var_pitch_forearm avg_yaw_forearm
1                  NA               NA                NA                   NA                NA              NA
2                  NA               NA                NA                   NA                NA              NA
3                  NA               NA                NA                   NA                NA              NA
4                  NA               NA                NA                   NA                NA              NA
5                  NA               NA                NA                   NA                NA              NA
6                  NA               NA                NA                   NA                NA              NA
  stddev_yaw_forearm var_yaw_forearm gyros_forearm_x gyros_forearm_y gyros_forearm_z accel_forearm_x
1                 NA              NA            0.03            0.00           -0.02             192
2                 NA              NA            0.02            0.00           -0.02             192
3                 NA              NA            0.03           -0.02            0.00             196
4                 NA              NA            0.02           -0.02            0.00             189
5                 NA              NA            0.02            0.00           -0.02             189
6                 NA              NA            0.02           -0.02           -0.03             193
  accel_forearm_y accel_forearm_z magnet_forearm_x magnet_forearm_y magnet_forearm_z classe
1             203            -215              -17              654              476      A
2             203            -216              -18              661              473      A
3             204            -213              -18              658              469      A
4             206            -214              -16              658              469      A
5             206            -214              -17              655              473      A
6             203            -215               -9              660              478      A
 [ reached 'max' / getOption("max.print") -- omitted 4 rows ]
```

There are missing values and features not important to train the model. Let's move on to clean up the data.

## Data Cleaning
Let's remove missing values and unneccesary features from 1st to 7th column, left 53 features.

```sh
> training<- training[,colSums(is.na(training)) == 0]
> testing<- testing[,colSums(is.na(testing)) == 0]
> training <- training[, -c(1:7)]
> testing <- testing[, -c(1:7)]
> dim(training)
[1] 19622    53
> dim(testing)
[1] 20 53
```

## Data Split
Split training data to 70% for training and 30% for validation.

```sh
> library(caret)
Loading required package: lattice
Loading required package: ggplot2
> set.seed(3433) 
> inTrain <- createDataPartition(training$classe, p = 0.7, list = FALSE)
> train_data <- training[inTrain, ]
> validation_data <- training[-inTrain, ]
```

## Training
Let's try the famous Random Forest algorithm to solve this classification problem.

```sh
> library(randomForest)
randomForest 4.6-14
Type rfNews() to see new features/changes/bug fixes.

Attaching package: ‘randomForest’

The following object is masked from ‘package:ggplot2’:

    margin

> rf <- randomForest(classe ~ ., data=train_data)
> rf

Call:
 randomForest(formula = classe ~ ., data = train_data) 
               Type of random forest: classification
                     Number of trees: 500
No. of variables tried at each split: 7

        OOB estimate of  error rate: 0.54%
Confusion matrix:
     A    B    C    D    E class.error
A 3899    4    1    0    2 0.001792115
B   13 2643    2    0    0 0.005643341
C    0   20 2372    4    0 0.010016694
D    0    0   22 2228    2 0.010657194
E    0    0    1    3 2521 0.001584158
```

## Evaluation
Now, using the training model to evaluate against validation_data.
```sh
> pred = predict(rf, newdata=validation_data)
> (cm <- confusionMatrix(validation_data$classe, pred))
Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1671    3    0    0    0
         B    6 1128    5    0    0
         C    0    6 1019    1    0
         D    0    0    7  955    2
         E    0    0    2    1 1079

Overall Statistics
                                          
               Accuracy : 0.9944          
                 95% CI : (0.9921, 0.9961)
    No Information Rate : 0.285           
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.9929          
                                          
 Mcnemar's Test P-Value : NA              

Statistics by Class:

                     Class: A Class: B Class: C Class: D Class: E
Sensitivity            0.9964   0.9921   0.9864   0.9979   0.9981
Specificity            0.9993   0.9977   0.9986   0.9982   0.9994
Pos Pred Value         0.9982   0.9903   0.9932   0.9907   0.9972
Neg Pred Value         0.9986   0.9981   0.9971   0.9996   0.9996
Prevalence             0.2850   0.1932   0.1755   0.1626   0.1837
Detection Rate         0.2839   0.1917   0.1732   0.1623   0.1833
Detection Prevalence   0.2845   0.1935   0.1743   0.1638   0.1839
Balanced Accuracy      0.9979   0.9949   0.9925   0.9980   0.9988
> (rf_accuracy <- cm$overall[1])
 Accuracy 
0.9943925 
```

The overall accuracy of 99% is good! Also Cross Validation is skipped, since OBB estimate of error rate is low at 0.54%. OOB is a very straightforward way to estimate the test error of a bagged model, without the need to perform cross-validation. Random Forest out-of-box tuning already generate good accuracy shown that it's really a good algorithm to be considered when we want to use decision tree approach to solve classification problem.

## Prediction for the testing data
Lastly, we use the model to predict for the given testing datasets.
```sh
> (pred_testing <- predict(rf, testing))
 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
 B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
Levels: A B C D E
```
### End

