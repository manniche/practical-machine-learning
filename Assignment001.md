Programming Assignment 1 for "Practical Macine Learning"
========================================================

The goal of your project is to predict the manner in which they did the exercise. This is the "classe" variable in the training set. You may use any of the other variables to predict with. You should 

 1. create a report describing how you built your model, 
 2. how you used cross validation, 
 3. what you think the expected out of sample error is, and 
 4. why you made the choices you did. 
 
You will also use your prediction model to predict 20 different test cases.


```r
library(caret)
data = read.csv("C:\\Temp\\pml-training.csv",na.strings=c("NA",""))
head(data)
```

```
##   X user_name raw_timestamp_part_1 raw_timestamp_part_2   cvtd_timestamp
## 1 1  carlitos           1323084231               788290 05/12/2011 11:23
## 2 2  carlitos           1323084231               808298 05/12/2011 11:23
## 3 3  carlitos           1323084231               820366 05/12/2011 11:23
## 4 4  carlitos           1323084232               120339 05/12/2011 11:23
## 5 5  carlitos           1323084232               196328 05/12/2011 11:23
## 6 6  carlitos           1323084232               304277 05/12/2011 11:23
##   new_window num_window roll_belt pitch_belt yaw_belt total_accel_belt
## 1         no         11      1.41       8.07    -94.4                3
## 2         no         11      1.41       8.07    -94.4                3
## 3         no         11      1.42       8.07    -94.4                3
## 4         no         12      1.48       8.05    -94.4                3
## 5         no         12      1.48       8.07    -94.4                3
## 6         no         12      1.45       8.06    -94.4                3
##   kurtosis_roll_belt kurtosis_picth_belt kurtosis_yaw_belt
## 1               <NA>                <NA>              <NA>
## 2               <NA>                <NA>              <NA>
## 3               <NA>                <NA>              <NA>
## 4               <NA>                <NA>              <NA>
## 5               <NA>                <NA>              <NA>
## 6               <NA>                <NA>              <NA>
##   skewness_roll_belt skewness_roll_belt.1 skewness_yaw_belt max_roll_belt
## 1               <NA>                 <NA>              <NA>            NA
## 2               <NA>                 <NA>              <NA>            NA
## 3               <NA>                 <NA>              <NA>            NA
## 4               <NA>                 <NA>              <NA>            NA
## 5               <NA>                 <NA>              <NA>            NA
## 6               <NA>                 <NA>              <NA>            NA
##   max_picth_belt max_yaw_belt min_roll_belt min_pitch_belt min_yaw_belt
## 1             NA         <NA>            NA             NA         <NA>
## 2             NA         <NA>            NA             NA         <NA>
## 3             NA         <NA>            NA             NA         <NA>
## 4             NA         <NA>            NA             NA         <NA>
## 5             NA         <NA>            NA             NA         <NA>
## 6             NA         <NA>            NA             NA         <NA>
##   amplitude_roll_belt amplitude_pitch_belt amplitude_yaw_belt
## 1                  NA                   NA               <NA>
## 2                  NA                   NA               <NA>
## 3                  NA                   NA               <NA>
## 4                  NA                   NA               <NA>
## 5                  NA                   NA               <NA>
## 6                  NA                   NA               <NA>
##   var_total_accel_belt avg_roll_belt stddev_roll_belt var_roll_belt
## 1                   NA            NA               NA            NA
## 2                   NA            NA               NA            NA
## 3                   NA            NA               NA            NA
## 4                   NA            NA               NA            NA
## 5                   NA            NA               NA            NA
## 6                   NA            NA               NA            NA
##   avg_pitch_belt stddev_pitch_belt var_pitch_belt avg_yaw_belt
## 1             NA                NA             NA           NA
## 2             NA                NA             NA           NA
## 3             NA                NA             NA           NA
## 4             NA                NA             NA           NA
## 5             NA                NA             NA           NA
## 6             NA                NA             NA           NA
##   stddev_yaw_belt var_yaw_belt gyros_belt_x gyros_belt_y gyros_belt_z
## 1              NA           NA         0.00         0.00        -0.02
## 2              NA           NA         0.02         0.00        -0.02
## 3              NA           NA         0.00         0.00        -0.02
## 4              NA           NA         0.02         0.00        -0.03
## 5              NA           NA         0.02         0.02        -0.02
## 6              NA           NA         0.02         0.00        -0.02
##   accel_belt_x accel_belt_y accel_belt_z magnet_belt_x magnet_belt_y
## 1          -21            4           22            -3           599
## 2          -22            4           22            -7           608
## 3          -20            5           23            -2           600
## 4          -22            3           21            -6           604
## 5          -21            2           24            -6           600
## 6          -21            4           21             0           603
##   magnet_belt_z roll_arm pitch_arm yaw_arm total_accel_arm var_accel_arm
## 1          -313     -128      22.5    -161              34            NA
## 2          -311     -128      22.5    -161              34            NA
## 3          -305     -128      22.5    -161              34            NA
## 4          -310     -128      22.1    -161              34            NA
## 5          -302     -128      22.1    -161              34            NA
## 6          -312     -128      22.0    -161              34            NA
##   avg_roll_arm stddev_roll_arm var_roll_arm avg_pitch_arm stddev_pitch_arm
## 1           NA              NA           NA            NA               NA
## 2           NA              NA           NA            NA               NA
## 3           NA              NA           NA            NA               NA
## 4           NA              NA           NA            NA               NA
## 5           NA              NA           NA            NA               NA
## 6           NA              NA           NA            NA               NA
##   var_pitch_arm avg_yaw_arm stddev_yaw_arm var_yaw_arm gyros_arm_x
## 1            NA          NA             NA          NA        0.00
## 2            NA          NA             NA          NA        0.02
## 3            NA          NA             NA          NA        0.02
## 4            NA          NA             NA          NA        0.02
## 5            NA          NA             NA          NA        0.00
## 6            NA          NA             NA          NA        0.02
##   gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y accel_arm_z magnet_arm_x
## 1        0.00       -0.02        -288         109        -123         -368
## 2       -0.02       -0.02        -290         110        -125         -369
## 3       -0.02       -0.02        -289         110        -126         -368
## 4       -0.03        0.02        -289         111        -123         -372
## 5       -0.03        0.00        -289         111        -123         -374
## 6       -0.03        0.00        -289         111        -122         -369
##   magnet_arm_y magnet_arm_z kurtosis_roll_arm kurtosis_picth_arm
## 1          337          516              <NA>               <NA>
## 2          337          513              <NA>               <NA>
## 3          344          513              <NA>               <NA>
## 4          344          512              <NA>               <NA>
## 5          337          506              <NA>               <NA>
## 6          342          513              <NA>               <NA>
##   kurtosis_yaw_arm skewness_roll_arm skewness_pitch_arm skewness_yaw_arm
## 1             <NA>              <NA>               <NA>             <NA>
## 2             <NA>              <NA>               <NA>             <NA>
## 3             <NA>              <NA>               <NA>             <NA>
## 4             <NA>              <NA>               <NA>             <NA>
## 5             <NA>              <NA>               <NA>             <NA>
## 6             <NA>              <NA>               <NA>             <NA>
##   max_roll_arm max_picth_arm max_yaw_arm min_roll_arm min_pitch_arm
## 1           NA            NA          NA           NA            NA
## 2           NA            NA          NA           NA            NA
## 3           NA            NA          NA           NA            NA
## 4           NA            NA          NA           NA            NA
## 5           NA            NA          NA           NA            NA
## 6           NA            NA          NA           NA            NA
##   min_yaw_arm amplitude_roll_arm amplitude_pitch_arm amplitude_yaw_arm
## 1          NA                 NA                  NA                NA
## 2          NA                 NA                  NA                NA
## 3          NA                 NA                  NA                NA
## 4          NA                 NA                  NA                NA
## 5          NA                 NA                  NA                NA
## 6          NA                 NA                  NA                NA
##   roll_dumbbell pitch_dumbbell yaw_dumbbell kurtosis_roll_dumbbell
## 1         13.05         -70.49       -84.87                   <NA>
## 2         13.13         -70.64       -84.71                   <NA>
## 3         12.85         -70.28       -85.14                   <NA>
## 4         13.43         -70.39       -84.87                   <NA>
## 5         13.38         -70.43       -84.85                   <NA>
## 6         13.38         -70.82       -84.47                   <NA>
##   kurtosis_picth_dumbbell kurtosis_yaw_dumbbell skewness_roll_dumbbell
## 1                    <NA>                  <NA>                   <NA>
## 2                    <NA>                  <NA>                   <NA>
## 3                    <NA>                  <NA>                   <NA>
## 4                    <NA>                  <NA>                   <NA>
## 5                    <NA>                  <NA>                   <NA>
## 6                    <NA>                  <NA>                   <NA>
##   skewness_pitch_dumbbell skewness_yaw_dumbbell max_roll_dumbbell
## 1                    <NA>                  <NA>                NA
## 2                    <NA>                  <NA>                NA
## 3                    <NA>                  <NA>                NA
## 4                    <NA>                  <NA>                NA
## 5                    <NA>                  <NA>                NA
## 6                    <NA>                  <NA>                NA
##   max_picth_dumbbell max_yaw_dumbbell min_roll_dumbbell min_pitch_dumbbell
## 1                 NA             <NA>                NA                 NA
## 2                 NA             <NA>                NA                 NA
## 3                 NA             <NA>                NA                 NA
## 4                 NA             <NA>                NA                 NA
## 5                 NA             <NA>                NA                 NA
## 6                 NA             <NA>                NA                 NA
##   min_yaw_dumbbell amplitude_roll_dumbbell amplitude_pitch_dumbbell
## 1             <NA>                      NA                       NA
## 2             <NA>                      NA                       NA
## 3             <NA>                      NA                       NA
## 4             <NA>                      NA                       NA
## 5             <NA>                      NA                       NA
## 6             <NA>                      NA                       NA
##   amplitude_yaw_dumbbell total_accel_dumbbell var_accel_dumbbell
## 1                   <NA>                   37                 NA
## 2                   <NA>                   37                 NA
## 3                   <NA>                   37                 NA
## 4                   <NA>                   37                 NA
## 5                   <NA>                   37                 NA
## 6                   <NA>                   37                 NA
##   avg_roll_dumbbell stddev_roll_dumbbell var_roll_dumbbell
## 1                NA                   NA                NA
## 2                NA                   NA                NA
## 3                NA                   NA                NA
## 4                NA                   NA                NA
## 5                NA                   NA                NA
## 6                NA                   NA                NA
##   avg_pitch_dumbbell stddev_pitch_dumbbell var_pitch_dumbbell
## 1                 NA                    NA                 NA
## 2                 NA                    NA                 NA
## 3                 NA                    NA                 NA
## 4                 NA                    NA                 NA
## 5                 NA                    NA                 NA
## 6                 NA                    NA                 NA
##   avg_yaw_dumbbell stddev_yaw_dumbbell var_yaw_dumbbell gyros_dumbbell_x
## 1               NA                  NA               NA                0
## 2               NA                  NA               NA                0
## 3               NA                  NA               NA                0
## 4               NA                  NA               NA                0
## 5               NA                  NA               NA                0
## 6               NA                  NA               NA                0
##   gyros_dumbbell_y gyros_dumbbell_z accel_dumbbell_x accel_dumbbell_y
## 1            -0.02             0.00             -234               47
## 2            -0.02             0.00             -233               47
## 3            -0.02             0.00             -232               46
## 4            -0.02            -0.02             -232               48
## 5            -0.02             0.00             -233               48
## 6            -0.02             0.00             -234               48
##   accel_dumbbell_z magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z
## 1             -271              -559               293               -65
## 2             -269              -555               296               -64
## 3             -270              -561               298               -63
## 4             -269              -552               303               -60
## 5             -270              -554               292               -68
## 6             -269              -558               294               -66
##   roll_forearm pitch_forearm yaw_forearm kurtosis_roll_forearm
## 1         28.4         -63.9        -153                  <NA>
## 2         28.3         -63.9        -153                  <NA>
## 3         28.3         -63.9        -152                  <NA>
## 4         28.1         -63.9        -152                  <NA>
## 5         28.0         -63.9        -152                  <NA>
## 6         27.9         -63.9        -152                  <NA>
##   kurtosis_picth_forearm kurtosis_yaw_forearm skewness_roll_forearm
## 1                   <NA>                 <NA>                  <NA>
## 2                   <NA>                 <NA>                  <NA>
## 3                   <NA>                 <NA>                  <NA>
## 4                   <NA>                 <NA>                  <NA>
## 5                   <NA>                 <NA>                  <NA>
## 6                   <NA>                 <NA>                  <NA>
##   skewness_pitch_forearm skewness_yaw_forearm max_roll_forearm
## 1                   <NA>                 <NA>               NA
## 2                   <NA>                 <NA>               NA
## 3                   <NA>                 <NA>               NA
## 4                   <NA>                 <NA>               NA
## 5                   <NA>                 <NA>               NA
## 6                   <NA>                 <NA>               NA
##   max_picth_forearm max_yaw_forearm min_roll_forearm min_pitch_forearm
## 1                NA            <NA>               NA                NA
## 2                NA            <NA>               NA                NA
## 3                NA            <NA>               NA                NA
## 4                NA            <NA>               NA                NA
## 5                NA            <NA>               NA                NA
## 6                NA            <NA>               NA                NA
##   min_yaw_forearm amplitude_roll_forearm amplitude_pitch_forearm
## 1            <NA>                     NA                      NA
## 2            <NA>                     NA                      NA
## 3            <NA>                     NA                      NA
## 4            <NA>                     NA                      NA
## 5            <NA>                     NA                      NA
## 6            <NA>                     NA                      NA
##   amplitude_yaw_forearm total_accel_forearm var_accel_forearm
## 1                  <NA>                  36                NA
## 2                  <NA>                  36                NA
## 3                  <NA>                  36                NA
## 4                  <NA>                  36                NA
## 5                  <NA>                  36                NA
## 6                  <NA>                  36                NA
##   avg_roll_forearm stddev_roll_forearm var_roll_forearm avg_pitch_forearm
## 1               NA                  NA               NA                NA
## 2               NA                  NA               NA                NA
## 3               NA                  NA               NA                NA
## 4               NA                  NA               NA                NA
## 5               NA                  NA               NA                NA
## 6               NA                  NA               NA                NA
##   stddev_pitch_forearm var_pitch_forearm avg_yaw_forearm
## 1                   NA                NA              NA
## 2                   NA                NA              NA
## 3                   NA                NA              NA
## 4                   NA                NA              NA
## 5                   NA                NA              NA
## 6                   NA                NA              NA
##   stddev_yaw_forearm var_yaw_forearm gyros_forearm_x gyros_forearm_y
## 1                 NA              NA            0.03            0.00
## 2                 NA              NA            0.02            0.00
## 3                 NA              NA            0.03           -0.02
## 4                 NA              NA            0.02           -0.02
## 5                 NA              NA            0.02            0.00
## 6                 NA              NA            0.02           -0.02
##   gyros_forearm_z accel_forearm_x accel_forearm_y accel_forearm_z
## 1           -0.02             192             203            -215
## 2           -0.02             192             203            -216
## 3            0.00             196             204            -213
## 4            0.00             189             206            -214
## 5           -0.02             189             206            -214
## 6           -0.03             193             203            -215
##   magnet_forearm_x magnet_forearm_y magnet_forearm_z classe
## 1              -17              654              476      A
## 2              -18              661              473      A
## 3              -18              658              469      A
## 4              -16              658              469      A
## 5              -17              655              473      A
## 6               -9              660              478      A
```

The `head` command shows us that we can expect many NA values in the data set, so the first task should be to sanitize the data.


```r
data.nas <- apply(data,2,function(x) {sum(is.na(x))})
training.data <- data[,which(data.nas == 0)]
```

Second, there are some variables that we should not be using as predictors directly: 'user_name', timestamps and measurement windows should all be excluded from the predictors training set.


```r
training.data.clean <- training.data[,8:dim(training.data)[2]]
```

Then, we construct datasets for cross-validation, separated on the `classe` variable.


```r
idx <- createDataPartition(y = training.data.clean$classe, p=0.2,list=FALSE)
training.data.fifth <- training.data.clean[idx, ]
training.data.cv <- training.data.clean[-idx, ]
```


```r
training.model.fitted <- train(as.factor(training.data.fifth$classe)~ .,data=training.data.fifth,  preProcess=c("pca"),  method="rf",prox=TRUE, trControl = trainControl(method = "cv", number = 4, allowParallel=F))
```

Then see how the model does against the cross-validation set


```r
predictions = predict(training.model.fitted, training.data.cv[-60])
table(predictions,training.data.cv$classe)
```

```
##            
## predictions    A    B    C    D    E
##           A 4297  177   42   50   33
##           B   45 2688  120   13   44
##           C   48  128 2450  185   82
##           D   50   19   79 2285   53
##           E   24   25   46   39 2673
```

```r
accuracy_training = sum(predictions == training.data.cv$classe)/length(predictions)
accuracy_training
```

```
## [1] 0.917
```

And lets see how the model does on the test data


```r
raw.testing.data <- read.csv("C:\\Temp\\pml-testing.csv")
answers  <- predict(training.model.fitted,raw.testing.data);
table(answers)
```

```
## answers
## A B C D E 
## 8 7 2 1 2
```

```r
answers_text = cbind(raw.testing.data$problem_id,as.character(answers))
answers_text
```

```
##       [,1] [,2]
##  [1,] "1"  "B" 
##  [2,] "2"  "A" 
##  [3,] "3"  "C" 
##  [4,] "4"  "A" 
##  [5,] "5"  "A" 
##  [6,] "6"  "B" 
##  [7,] "7"  "D" 
##  [8,] "8"  "B" 
##  [9,] "9"  "A" 
## [10,] "10" "A" 
## [11,] "11" "A" 
## [12,] "12" "C" 
## [13,] "13" "B" 
## [14,] "14" "A" 
## [15,] "15" "E" 
## [16,] "16" "E" 
## [17,] "17" "A" 
## [18,] "18" "B" 
## [19,] "19" "B" 
## [20,] "20" "B"
```

