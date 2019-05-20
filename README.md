# DATASET IS UNDER REVIEW AND WILL BECOME AVAILABLE SOON

# CogBeacon: A Multi-Modal Dataset for Modeling Cognitive Fatigue
CogBeacon is a multi-modal dataset designed to target the effects of cognitive fatigue in human performance. The dataset consists of 76 sessions collected from 19 male and female users performing different versions of the Wisconsin Card Sorting Test (WCST); a popular cognitive test in experimental and clinical psychology designed to assess cognitive flexibility, reasoning and specific aspects of cognitive functioning. During each session we record and fully annotate, user's EEG functionality, facial keypoints, real-time self-reports on cognitive fatigue, as well as detailed information of the performance metrics achieved during the cognitive task (success rate, response time, number of errors etc.). Along with the dataset we provide a baseline Machine Learning analysis towards predicting cognitive fatigue and our multi-modal implementation of the WCST, to allow other researches expand or modify the functionalities of the CogBeacon data-collection framework. To our knowledge, this is the first multi-modal dataset specifically designed to assess cognitive fatigue.



## CogBeacon Data Collection Platform can be found @ [The WCST Interface](https://github.com/MikeMpapa/CogBeacon-WCST_interface/)

## Dataset Details
As mentioned above, the dataset consists of 4 folders; EEG, face_keypoints, fatigue_self_report, and user_performance. Below, we explain the contents of each folder and the way the data is organized.
Please refer to our paper **CogBeacon: A Multi-Modal Dataset and Data-Collection Platform for Modeling Cognitive Fatigue (insert link to publication)**, for more details about the dataset and data collection methodology.
### 1. EEG Data:
**Filename structure:** There are 76 sessions and data from each session is stored in a separate folder. This folder is named "user_\<userID\> _ \<StimuliType\> _ \<GameMode\>". For example, the folder name "user_0_v_m" implies the folder belongs to user with ID 0 using the visual stimuli and that data were collected by a modified version of the WCST task (V1 or V2). If the user_id is a single integer and the game mode is characterised by the letter "m" it means that data were recorder using the V1 WCST version. If user_id is followed by the letter "b" and the game mode is characterised as "m" it means that the folder contains data captured using V2 version of the WCST. Finally if game mode is characterised by the letter "o" it means that data collection was based on replicating the rules of the original WCST. Bellow are explained the values of the different flags in detail:

- UserID: 

  * if ID --> data were collected in the first day of data collection and IF GameMode = "m" WCST=V1

  * if IDb --> data were collected in the second day of data collection and IF GameMode = "m" WCST=V2

- StimuliType: "v"=visual, "t"=textual, "a"=auditory

- GameMode: "o"=simulation of the original WCST test, "m"=modified version of the WCST

Each of these folders consists of individual files collected during each round, ie a single answer provided by the user. The file name encoding is as follows: \<roundID_under_the_same_rule\> _ \<roundID\>. For example file 3_17 containes the data captured during the 17th round of the game, which was the 3rd round under the same decision rule.

The EEG Data was recorded using the [Muse EEG headset](https://choosemuse.com/). The headset has four electrodes, two over the prefrontal lobe and two behind the ears. The data set consists of:
* **Raw EEG :** at a sampling frequency of 220 Hz
* **Absolute Frequency Bands (A):** gamma 32-100 Hz (*&gamma;*), beta 13-32 Hz (*&beta;*), alpha 8-13 Hz (*&alpha;*), theta 4-8 Hz (*&theta;*) and delta 0.5-4Hz (*&delta;*) at sampling frequency of 10 Hz. The absolute band power for a given frequency range is the logarithm of the sum of the Power Spectral Density of the EEG data over that frequency range.
  <p align="center"><img src="https://github.com/MikeMpapa/CogBeacon-MultiModal_Dataset_for_Cognitive_Fatigue/blob/master/Absolute%20Frequency%20Band.png"></p>
  where f_low and f_high are the minimum and maximum frequencies of frequency band x and G is the FFT of the raw EEG signal g.
* **Relative Frequency Bands (R):** &gamma;, &beta;, &alpha;, &theta; and &delta; at sampling frequency of 10 Hz. The relative band powers are calculated by dividing the absolute linear-scale power in one band over the sum of the absolute linear-scale powers in all bands. 
  <p align="center"><img src="https://github.com/MikeMpapa/CogBeacon-MultiModal_Dataset_for_Cognitive_Fatigue/blob/master/Relative%20Frequency%20Band.png"></p>
  where x is one of the five frequency bands.
* **Session Score for each Frequency band (S):** A value computed by comparing the current value f a band power to its history in sampling frequency of 10 Hz. This value is mapped to a score etween 0 and 1 using a linear function that returns 0 if the current value is equal to or below the 10th percentile of the distribution of band powers, and returns 1 if it’s equal to or above the 90th percentile. Linear scoring between 0 and 1 is done for any value between these two percentiles.
* **Signal Quality Indicator:** An integer value from 1 (optimal quality) to 4 (very bad quality).

### 2. Facial Keypoints:
To capture behavioral changes during the task, we also recorded variations in the movement of the face, capturing a set of 68 facial keypoints and four corners for bounding box, with a webcam placed on top of the screen, at a frame rate of 2 FPS. To identify facial keypoints, we deployed the method presented by [Kazemi et. al.](http://openaccess.thecvf.com/content_cvpr_2014/papers/Kazemi_One_Millisecond_Face_2014_CVPR_paper.pdf) that uses a Regression Tree approach and can be applied in a real-time. 

**Filename structure:** This data can be found in a folder named face_keypoints. Similar to the EEG dataset, there are 76 sessions and data from each session is stored in a separate folder. The name of these folders follow the same naming as explained above. The last flag in the filename indicates the frameID. Thus, each filename has a name structure as : \<round_under_the_same_rule\> _ \<roundID\> _ \<frameID\>. The facial keypoints and the corners of the bounding box are stored in a numpy (.npz). For example file named "1_1_3.npz", indicates that it contains the keypoints captured in the third frame of the session, which bellonged in the 1st round of the game which was also the 1st round under the same decision rule.

### 3. Fatigue self report:
During each session, participants were told to report when they were having trouble to keep up with the task by pressing a button placed in front of them. The button could be pressed at any time during a game as many times as the participants felt appropriate.

**Filename structure:** This data can be found in the folder named fatigue_self_report. The self reports are stored as csv files for each session and the name of these files follow the same structures as the eeg session folders and facial keypoints session folders. Each record in the csv indicates how the total number of times a user has pressed the button at this specific point of the game. Each row of the csv corresponds to a game round. For example an a value of "3" in row 50 means that in that until the 50th round the user had pressed the button 3 times.

### 4. User Performance:
For every round of every session, the system logs a set of metrics and scores related to user performance with respect to the task. For each session, user performance metrics are stored as a csv and each file follows he same naming convention as the other type of data, with an additional metric at the end of the filename indicating the total score of the user at the end of the game computed by summing the individual user scores at each round. The formula for estimating user scores at each round can be found bellow. Each record of the csv consists of the following metrics:
* **Round Number:** An integer value indicating the current round.
* **Question number:** An integer value indicating the current question number under a specific rule type.
* **Level:** An integer value indicating the current level of the test. It indicates number of possible choices offered by the system: 2,3,4 or 5.
* **Score:** An indicative round-based user score computed as:
  <p align="center"><img src="https://github.com/MikeMpapa/CogBeacon-MultiModal_Dataset_for_Cognitive_Fatigue/blob/master/Round%20Based%20Score.png" width="350" height=80></p>
* **Stimuli:** The type of the correct stimuli: color, shape or number.
* **Stimuli Type:** The value of the correct stimuli:
  * If color: green, yellow, blue, red or magenta.
  * If shape: triangle, star, cross, circle or heart.
  * If number: one, two, three, four or five.
* **Response:** A binary flag that indicating if user response was correct in a given round.
* **Time:** User’s response time at every round **in seconds**.
* **Correct:** The total number of correct answers.
* **NON-PER Errors:** The cumulative number of non-perseverative errors until the current round. Non-perseverative errors are the errors recorded when the user tries to figure out the new rule after a rule change. Given that there were three possible decision rules in total (based on color or shape or number), a user is supposed to figure out the correct rule no later than the third round after a rule change. Any error that occurred before the third round is considered as non-perseverative error.
* **PER Errors:** The cumulative number of perseverative errors until the current round. Perseverative errors are when the user continues to apply the wrong rule despite the informative feedback provided by the system.
### Source code + ML analysis:
The code we used for ML analysis and the respective data, with the appropriate data splits for cross validation, in our papaer **CogBeacon: A Multi-Modal Dataset and Data-Collection Platform for Modeling Cognitive Fatigue** is available at (insert link).
<link to: datasets used for ML analysis (balanced+unbalanced) + analysis code: feature extraction +modeling, dataset creation>


### Confidentiality & Data Sharing

Our team has received permission by the Institutional Review Board (IRB) of the University of Texas at Arlington (UTA) in order to conduct these experiments and share the attached data. CogBeacons protocol’s ID is 2019-0253 and its title is “CogBeacon: Towards detecting cognitive fatigue”. Main contributors of this study are  Mr.Michalis Papakostas (PI, michalis.papakostas@mavs.uta.edu), Mr. Akilesh Rajavenkatanarayanan (CO-PI, akilesh.rajavenkatanarayanan@mavs.uta.edu), and  Dr. Fillia Makedon (Faculty Advisor,  makedon@uta.edu).  For additional information or questions about the confidentiality or data sharing protocol please feel free to contact directly the IRB office at UTA (erahelpdesk@uta.edu) or to the project personel. Available in the repo is a copy of the consent form that the participants had to sign.



