# DATASET IS UNDER REVIEW AND WILL BECOME AVAILABLE SOON

# CogBeacon: A Multi-Modal Dataset for Modeling Cognitive Fatigue
CogBeacon is a multi-modal dataset designed to target the effects of cognitive fatigue in human performance. The dataset consists of 76 sessions collected from 19 male and female users performing different versions of the Wisconsin Card Sorting Test (WCST); a popular cognitive test in experimental and clinical psychology designed to assess cognitive flexibility, reasoning and specific aspects of cognitive functioning. During each session we record and fully annotate, user's EEG functionality, facial keypoints, real-time self-reports on cognitive fatigue, as well as detailed information of the performance metrics achieved during the cognitive task (success rate, response time, number of errors etc.). Along with the dataset we provide a baseline Machine Learning analysis towards predicting cognitive fatigue and our multi-modal implementation of the WCST, to allow other researches expand or modify the functionalities of the CogBeacon data-collection framework. To our knowledge, this is the first multi-modal dataset specifically designed to assess cognitive fatigue.



## CogBeacon Data Collection Platform can be found @ [The WCST Interface](https://github.com/MikeMpapa/CogBeacon-WCST_interface/)

## Dataset Details
As mentioned above, the dataset consists of 4 folders; EEG, face_keypoints, fatigue_self_report, and user_performance. Below, we explain the contents of each folder and the way the data is organized.
Please refer to our paper **CogBeacon: A Multi-Modal Dataset and Data-Collection Platform for Modeling Cognitive Fatigue (insert link to publication)**, for more details about the dataset and data collection methodology.
### 1. EEG Data:
**Filename structure:** As mentioned above, there are 76 sessions and data from each session is stored in a separate folder. This folder is named "user_user-id_version-type". For example, the folder name "user_0_v_m" implies the folder belongs to user with ID 0 and version type "v_m". Each of these folders consists of individual files for each round. These files are named "0_1", indicating that the files belong to rule type "0" and round "1". **(requires revision)**

The EEG Data was recorded using the [Muse EEG headset](https://choosemuse.com/). The headset has four electrodes, two over the prefrontal lobe and two behind the ears. The data set consists of:
* **Raw EEG :** at a sampling frequency of 220 Hz
* **Absolute Frequency Bands (A):** gamma 32-100 Hz (*&gamma;*), beta 13-32 Hz (*&beta;*), alpha 8-13 Hz (*&alpha;*), theta 4-8 Hz (*&theta;*) and delta 0.5-4Hz (*&delta;*) at sampling frequency of 10 Hz. The absolute band power for a given frequency range is the logarithm of the sum of the Power Spectral Density of the EEG data over that frequency range.
  <p align="center"><img src="https://github.com/MikeMpapa/CogBeacon-MultiModal_Dataset_for_Cognitive_Fatigue/blob/master/Absolute%20Frequency%20Band.png"></p>
* **Relative Frequency Bands (R):** &gamma;, &beta;, &alpha;, &theta; and &delta; at sampling frequency of 10 Hz. The relative band powers are calculated by dividing the absolute linear-scale power in one band over the sum of the absolute linear-scale powers in all bands. 
  <p align="center"><img src="https://github.com/MikeMpapa/CogBeacon-MultiModal_Dataset_for_Cognitive_Fatigue/blob/master/Relative%20Frequency%20Band.png"></p>
* **Session Score for each Frequency band (S):** A value computed by comparing the current value of a band power to its history in sampling frequency of 10 Hz.
* **Signal Quality Indicator:** An integer value from 1 (optimal quality) to 4 (very bad quality).

### 2. Facial:
To capture behavioral changes during the task, we also recorded variations in the movement of the face, capturing a set of 68 facial keypoints and four corners for bounding box, with a webcam placed on top of the screen, at a frame rate of **insert rate**. To identify facial keypoints, we deployed the method presented by [Kazemi et. al.](http://openaccess.thecvf.com/content_cvpr_2014/papers/Kazemi_One_Millisecond_Face_2014_CVPR_paper.pdf) that uses a Regression Tree approach and can be applied in a real-time. 

**Filename structure:** This data can be found in a folder named face_keypoints. Similar to the EEG dataset, there are 76 sessions and data from each session is stored in a separate folder. The name of these folders follow the same naming as explained above. The facial keypoints and the corners of the bounding box are stored in a numpy (.npz) file and are named "0_1_1.npz", indicating that the files belong to the rule type "0", round "1", and frame "1".

### 3. Fatigue self report:
During each session, participants were told to report when they were having trouble to keep up with the task by pressing a button placed in front of them. The button could be pressed at any time during a game as many times as the participants felt appropriate.

**Filename structure:** This data can be found in the folder named fatigue_self_report. The self reports are stored as csv files for each session and the name of these files follow the same structures as the eeg session folders and facial keypoints session folders. Each record in the csv indicate the level of fatigue as reported by the users during each round of the session.

### 4. User Performance:
For every round of every session, the system logs a set of metrics and scores related to user performance with respect to the task. For each session, user performance metrics are stored as a csv and each file follows he same naming convention as the other type of data, with an **additional metric** at the end of the filename. Each record of the csv consists of the following metrics:
* **Round Number:** An integer value indicating the current round.
* **Question number:** An integer value indicating the current question number under a specific rule type.
* **Level:** An integer value indicating the current level of the test. It indicates number of possible choices offered by the system: 2,3,4 or 5.
* **Score:** An indicative round-based user score computed as:
  <p align="center"><img src="https://github.com/MikeMpapa/CogBeacon-MultiModal_Dataset_for_Cognitive_Fatigue/blob/master/Round%20Based%20Score.png"></p>
* **Stimuli:** The type of the correct stimuli: color, shape or number.
* **Stimuli Type:** The value of the correct stimuli:
  * If color: green, yellow, blue, red or magenta.
  * If shape: triangle, star, cross, circle or heart.
  * If number: one, two, three, four or five.
* **Response:** A binary flag that indicating if user response was correct in a given round.
* **Persistence:** 
* **Time:** User’s response time at every round **in seconds**.
* **Correct:** The total number of correct answers.
* **NON-PER Errors:** The cumulative number of non-perseverative errors until the current round. Non-perseverative errors are the errors recorded when the user tries to figure out the new rule after a rule change. Given that there were three possible decision rules in total (based on color or shape or number), a user is supposed to figure out the correct rule no later than the third round after a rule change. Any error that occurred before the third round is considered as non-perseverative error.
* **PER Errorsn:** The cumulative number of perseverative errors until the current round. Perseverative errors are when the user continues to apply the wrong rule despite the informative feedback provided by the system.
### Source code + ML analysis:
The code we used for ML analysis and the respective data, with the appropriate data splits for cross validation, in our papaer **CogBeacon: A Multi-Modal Dataset and Data-Collection Platform for Modeling Cognitive Fatigue** is available at (insert link).
<link to: datasets used for ML analysis (balanced+unbalanced)>
<+analysis code: feature extraction +modeling, dataset creation>
