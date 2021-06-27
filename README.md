

## Portfolio Exercise: Starbucks
<br>

<img src="https://opj.ca/wp-content/uploads/2018/02/New-Starbucks-Logo-1200x969.jpg" width="200" height="200">
<br>
<br>
 

### Table of Contents

1. [Task Description](#description)
1.1. [Background Information](#info)
1.2. [Optimization Strategy](#optim)
1.3. [How To Test Your Strategy?](#test)
2. [Proposed Strategy](#strategy)
2.1. [Imbalanced Training Set](#imbalance)
2.2. [Parameter Tuning](#tuning)
3. [File Description](#files)
4. [Licensing, Authors, and Acknowledgements](#licensing)


## Task Description <a name="description"></a>

### Background Information  <a name="info"></a>

The dataset was originally used as a take-home assignment provided by Starbucks for their job candidates. The data for this exercise consists of about 120,000 data points split in a 2:1 ratio among training and test files. In the experiment simulated by the data, an advertising promotion was tested to see if it would bring more customers to purchase a specific product priced at $10. Since it costs the company 0.15 to send out each promotion, it would be best to limit that promotion only to those that are most receptive to the promotion. Each data point includes one column indicating whether or not an individual was sent a promotion for the product, and one column indicating whether or not that individual eventually purchased that product. Each individual also has seven additional features associated with them, which are provided abstractly as V1-V7.

### Optimization Strategy  <a name="optim"></a>

Your task is to use the training data to understand what patterns in V1-V7 to indicate that a promotion should be provided to a user. Specifically, your goal is to maximize the following metrics:

* **Incremental Response Rate (IRR)** 

IRR depicts how many more customers purchased the product with the promotion, as compared to if they didn't receive the promotion. Mathematically, it's the ratio of the number of purchasers in the promotion group to the total number of customers in the purchasers group (_treatment_) minus the ratio of the number of purchasers in the non-promotional group to the total number of customers in the non-promotional group (_control_).

$$ IRR = \frac{purch_{treat}}{cust_{treat}} - \frac{purch_{ctrl}}{cust_{ctrl}} $$


* **Net Incremental Revenue (NIR)**

NIR depicts how much is made (or lost) by sending out the promotion. Mathematically, this is 10 times the total number of purchasers that received the promotion minus 0.15 times the number of promotions sent out, minus 10 times the number of purchasers who were not given the promotion.

$$ NIR = (10\cdot purch_{treat} - 0.15 \cdot cust_{treat}) - 10 \cdot purch_{ctrl}$$

For a full description of what Starbucks provides to candidates see the [instructions available here](https://drive.google.com/open?id=18klca9Sef1Rs6q8DW4l7o349r8B70qXM).

Below you can find the training data provided.  Explore the data and different optimization strategies.

### How To Test Your Strategy? <a name="test"></a>

When you feel like you have an optimization strategy, complete the `promotion_strategy` function to pass to the `test_results` function.  
From past data, we know there are four possible outomes:

Table of actual promotion vs. predicted promotion customers:  

<table>
<tr><th></th><th colspan = '2'>Actual</th></tr>
<tr><th>Predicted</th><th>Yes</th><th>No</th></tr>
<tr><th>Yes</th><td>I</td><td>II</td></tr>
<tr><th>No</th><td>III</td><td>IV</td></tr>
</table>

The metrics are only being compared for the individuals we predict should obtain the promotion – that is, quadrants I and II.  Since the first set of individuals that receive the promotion (in the training set) receive it randomly, we can expect that quadrants I and II will have approximately equivalent participants.  

Comparing quadrant I to II then gives an idea of how well your promotion strategy will work in the future. 



## 2. Proposed Strategy <a name="strategy"></a>

In this exercise, we use Gradient Boosting Classifier as the classification model to predict whether to provide promotion to an individual or not. 

The variables used in the model include V1 through V7, where V1 and V4-V7 are categorical variables.  


### Imbalanced Training Set <a name="imbalance"></a>

A significant issue in our dataset is imbalance. Among all 80,000+ individuals, only about 1% of them finally purchase the product. This severe imbalanced training set would cause problems in our model training and learning process. To deal with this issue, an up-sampling method and a down-sampling method are considered. 


#### Down-Sampling

In down-sampling, we randomly sample a small subset from the sub-group in which individuals do not purchase the product in the training set, such that the proportion of individuals who purchase would be the same with the proportion of individuals who do not. By doing this, our training set significantly shrink. Lossing information leads to poor perfermance of the trained model.


#### Up-Sampling

In up-sampling, we bootstrap the sub-group in which individuals purchase the product, such that the number of individuals who purchase would be the same with the number of individuals who do not. This process expand our training set by adding duplicate samples. In this project, we use this expaneded training set to train the model.


### Parameter Tuning <a name="tuning"></a>

Generally, parameter tuning could help make the model perform better. This would also be a machine learning pipeline process. However, in our model training process, since the training set is processed (by up-sampling), the parameter tuning process might not be helpful. This is because the processed training set and the test set are no longer homogeneous -- a better performed model in training set might not lead to good performance in testing set. Because of this, we omit the parameter tuning process in this project.



## 3. File Description <a name="files"></a>

### Datasets

There are two raw datasets used for model training and testing: "training.csv" and "Test.csv". 

### Data Processing and Model

"Starbucks.ipynb" is a Jupyter Notebook file showing how the raw training dataset is processed and how the model is trained in this project. 

"test_result.py" is a function used to test the performance of the proposed promotion strategy. This file is provided by Udacity Data Science NanoDegree.

"model_GBC.pkl" is the trained model saved in pickle file format.


## 4. Licensing, Authors, and Acknowledgements <a name="licensing"></a>

Must give credits to: 
- Starbucks, who kindly provides the raw datasets, and 
- Udacity, who provides the nanodegree, related materials, and the project


