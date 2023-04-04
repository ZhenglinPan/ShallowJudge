# ShallowJudge
*@Author: Zhenglin Pan*
*@Date: March, 2023*

![ShallowJudge](https://github.com/ZhenglinPan/ShallowJudge/blob/main/others/Cover_ShallowJudge.jpg)

This is a final course project for ECE 720 X50 Winter 2023, University of Alberta.

**Note: This project is not done yet.**

Overview of the ShallowJudge
![ShallowJudge](https://github.com/ZhenglinPan/ShallowJudge/blob/main/others/Structure.png)

1. Firstly, we obtained "benchmarks" for each layer and each class, which are representative feature maps that indicate the most frequent patterns of a class at a certain network hierarchy. For all subsequent validation or testing samples, their feature maps on various levels are compared to the respective benchmark, and the more identical a feature map is to the benchmark, the more likely it belongs to this class. This is accessed quantitatively by "similarity."

2. Secondly, through kernel density estimation plots, we observed a distinct difference between positive and negative samples ("negative samples" here refers to those misclassified in this multi-class scenario) on the distribution of similarity. Negative samples tend to have a smaller similarity with the class benchmark, proving our fundamental assumption that positive and negative samples are subject to different distributions. Meanwhile, we noticed that the distributions are more contrastive on some layers, measured by the Bhattacharyya distance. In other words, some layers divide the positive and the negative well, and they are nominated as Judge Layers.

3. We then used the Bayesian method to convert a sample's similarity with the benchmark into the probability that the sample is negative, given its class is known. However, since the class is unknown, we estimated the probability of a sample's class. To do so, a state-abstraction process, i.e., trace analysis, is implemented. By using the distance function, we can understand how the state of a feature map changes in the network's layers. These traces are structural flow patterns that imply the class information in the early stage of the network. On state traces, we applied the Bayesian method, which provided us with a reasonable conjecture on a sample's class.

4. With class information found out in (3.), we proceeded to find probabilistic thresholds for traces. For instance, if a sample's trace is '4-4-4', which means the state of the feature maps gained by 3 judge layers are all 4 (i.e., all feature maps are closest to class 4's benchmarks), but the similarities are abnormally small (according to KDE, a negative sample tends to have a lower similarity in distribution), it suggests that this sample is of greater possibility to be regarded as a tricky sample, consequently, a higher threshold will be given to this sample.

5. Finally, with different thresholds (positively related to the possibility of being a negative sample) on different samples, ideally, higher on the bug-triggering tricky sample, we picked tricky samples out by random selection (rolling a dice) and ascertained they are tricky samples. Since tricky samples have a higher threshold, they are generally more likely to be picked. Later experiments on the validation set and test set had proven this.
