## Getting and Cleaning Data - Code Book


### Source Data
Data + Description can be found here [UCI HAR Dataset](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones "UCI MAchine Learning Repository")

### Data Set Information
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.

### Attribute Information
For each record in the dataset it is provided: 
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration. 
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

### The dataset includes the following files:

- 'README.txt'

- 'features_info.txt': Shows information about the variables used on the feature vector.

- 'features.txt': List of all features.

- 'activity_labels.txt': Links the class labels with their activity name.

- 'train/X_train.txt': Training set.

- 'train/y_train.txt': Training labels.

- 'test/X_test.txt': Test set.

- 'test/y_test.txt': Test labels.

The following files are available for the train and test data. Their descriptions are equivalent. 

- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 

- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 

- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

### Notes: 

- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file. 

### Processing - Data clean up and transformation

The raw data has been transformed through the following steps:

### 1. Merges the training and the test sets to create one data set
First, the script loads the data from `activity_labels.txt` and `features.txt` files, the train data from `train/subject_train.txt`, `train/X_train.txt` and `train/y_train.txt` files and the test data from `test/subject_test.txt`, `test/X_test.txt` and `test/y_test.txt` files. The fread() function is used to load the X_ files as they are large files.
Then, subject files are merged together as well as X and y files for train and test data sets.
The final merge of subject, X and y tables is made later because several operations are performed on each of them.

### 2. Extracts only the measurements on the mean and standard deviation for each measurement
Here, we filter the X table to select only variables containing `mean(` or `std(` using a regular expression.

### 3. Uses descriptive activity names to name the activities in the data set
The y table contains the id of the activity label of each measurement. So the script matches it with the corresponding name using the table loaded from the `activity_labels.txt` file.

### 4. Appropriately labels the data set with descriptive variable names
During this step, we take the values from `features.txt` with the same regular expression as in step 2 to put the variable names in the columns of X table. The subject and y columns, which are now merged with X, are respectively named `subject` and `activityLabel`.
Special characters are removed from the names and the first letter of `mean` and `std` are uppercased.

### 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
Finally, a summarization is made on the dataset to extract the mean of each subject/activityLabel group.
The final data set is written to a txt file.

### Tidy data set
The tidy data set of dimension 180x68 has the following variables:

- SubjectNum: id of the subject for whom the measurement was made (from 1 to 30)
- Activity: activity performed during the measurement (LAYING, SITTING, STANDING, WALKING, WALKING_DOWNSTAIRS or WALKING_UPSTAIRS)
- The 66 other variables are the mean and std of each measured signal (tBodyAccX, tBodyAccY, tBodyAccZ, tGravityAccX, tGravityAccY, tGravityAccZ, tBodyAccJerkX, tBodyAccJerkY, tBodyAccJerkZ, tBodyGyroX, tBodyGyroY, tBodyGyroZ, tBodyGyroJerkX, tBodyGyroJerkY, tBodyGyroJerkZ, tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag, fBodyAccX, fBodyAccY, fBodyAccZ, fBodyAccJerkX, fBodyAccJerkY, fBodyAccJerkZ, fBodyGyroX, fBodyGyroY, fBodyGyroZ, fBodyAccMag, fBodyBodyAccJerkMag, fBodyBodyGyroMag and fBodyBodyGyroJerkMag)
