# EL-GY-9163: Machine Learning for Cyber-security
# Lab 3 Report
**Net ID**: sn3533 <br>
**Name**: Sai Goutham Nelanuthula


# Overview
You must do the project individually. In this HW, you will design a backdoor detector for 
BadNets trained on the YouTube Face dataset using the pruning defence discussed in 
class. Your detector will take the following as input:
1. B, a backdoored neural network classifier with N classes
2. Dvalid, a validation dataset of clean, labelled images
1. Output the correct class if the test input is clean. The correct class will be in [1, N].
2. Output class N+1 if the input is backdoored.
You will design G using the pruning defence that we discussed in class. That is, you will prune 
the last pooling layer of BadNet B (the layer just before the FC layers) by removing one 
channel at a time from that layer. Channels should be removed in decreasing order of average 
activation values over the entire validation set. Every time you prune a channel, you will 
measure the new validation accuracy of the new pruned badnet. You will stop pruning once the 
validation accuracy drops at least X% below the original accuracy. This will be your new 
network B'.
Now, your goodnet G works as follows. For each test input, you will run it through both B and 
B'. If the classification outputs are the same, i.e., class i, you will output class i. If they differ you 
will output N+1. Evaluate this defence on:
1. A BadNet, B1, (“sunglasses backdoor”) on YouTube Face for which we have already
told you what the backdoor looks like. That is, we give you the validation data and
also test data with examples of clean and backdoored inputs.

# Description
The primary idea involves pruning the conv_3 layer in a convolutional neural network (CNN) based on the average activation over the validation dataset. The objective is to save the model at specific accuracy drops. We do this as follows,
1. Train and Validate the Model: Train the CNN model on the training dataset and assess its performance on the validation dataset.
2. Monitor Accuracy: Keep a close eye on the validation accuracy during training. Once the accuracy drops by at least 2%, 4%, or 10%, take the necessary actions.
3. Prune Convolutional Layer: When the accuracy drop criteria are met, initiate the pruning process for the conv_3 layer. This likely involves removing specific filters or neurons from conv_3.
4. Save the Model: After pruning, save the model with a distinctive name corresponding to the accuracy drop. Adhere to the naming convention:
  For a 2% accuracy drop: _**model_X=2.h5**_<br>
  For a 4% accuracy drop: _**model_X=4.h5**_<br>
  For a 10% accuracy drop: _**model_X=10.h5**_.<br>

The initial images without any poisoning look as below: <br>
![image](https://github.com/gauthamsai0071/MlCyber/assets/64630509/3704ed28-89c5-44ec-9314-1136b2c7c7f6)

After poisoning the data, we added sunglasses to the data: <br>
![image](https://github.com/gauthamsai0071/MlCyber/assets/64630509/1f788895-2604-4a2d-b2ca-d8fe8ec1dcc9)
<br>

After pruning the model with validation data set, the scatter plot of clean accuracy and attack success rate looks as below:
<br>
![image](https://github.com/gauthamsai0071/MlCyber/assets/64630509/8d14d4f3-4fec-45b3-be44-7676f2d145fb)

The prune defense is ineffective here since the attack success rate does not fall significantly. Because accuracy is jeopardized far too much, the attack success rate is adequate but not remarkable. The attack mechanism, according to my theory, is a prune immune attack, and the pruned model is kept with the poisoned data. <br>

The combined model with 2% repair exhibited a clean test accuracy of 95.90%, but it had a 100.00% attack rate, indicating susceptibility to adversarial attacks.
<br>

For the combined model with 4% repair, the clean test accuracy was 92.29%, and the attack rate slightly decreased to 99.98%.
<br>

The combined model with 10% repair showed a clean test accuracy of 84.54%, and the attack rate further decreased to 77.21%. This implies a trade-off between clean accuracy and resilience to adversarial attacks, with higher repair percentages providing increased robustness at the expense of reduced accuracy on clean data.
<br>

The attack success rate when the accuracy drops at least 30% is 6.954187234779596%.

To design a goodNet, we needed to combine two models together which were the BadNet and the repaired model. The program is available in the <code>sn3533_MlCyber_BackdoorsAttacks.ipynb</code> file.

Instruction to run the code :
  - Make sure that there is enough memory available for the execution (Ideally, Google Colab Pro subscription would do).
  - All the required data and models are situated in the data directory, make sure your correctly place them in your file directory, and adjust the paths accordingly.<br>
  


The below figure depicts the accuracy of combined drop models<br>

![image](https://github.com/gauthamsai0071/MlCyber/assets/64630509/6e52c475-bd28-4773-98e3-0dabd224ded4)


The below figure depicts the attack rate for combined drop models<br>

![image](https://github.com/gauthamsai0071/MlCyber/assets/64630509/194f1d8c-fc54-49f4-8489-16715e646b7e)







