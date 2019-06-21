# Machine Learning
Standfort Univesity on Coursera

## What is Machine Learning
Anabling a machine to learn to perform a task without explicitly programming it to do so.

Main machine learning algorithms:
1) Supervised learning
2) Unsupervised learning
3) Reinforcement learning
4) Recommender systems

## Supervised learning
Learning based on a dataset with a right answers given.
Problems:
 - Regression - continue valued output (eg. prices)
   e.g. Given a picture of a person, we have to predict their age on the basis of the given picture
 - Classification - descrete valued output (eg. 0 and 1), different numebr of features
   e.g. Given a patient with a tumor, we have to predict whether the tumor is malignant or benign

### Model Representation
Training set
m - number of training examples
x - inputs (features)
y -output (target variable)
(x,y)- a training examples
x_i, y_i) ith training examples

Training set -> Learning Algorithm -> h (hypothesis) - maps from x to y
h -> e.g. linear function (linear regression with one variable, univariate linear regression), quadratic function
Linear regression -> h = theta_0 + theta_1(x); theta->parameters

### Cost function
Squred error cost function (Mean squared error) (often used for regression)
: minimize sum((h(x)-y)^2)/m -> squared difference between the prediction and the actual output.



Support vector machine 

## Unsupervised learning
Looking for a patterns. There a dataset but without right answers.
 - Clustering - looking for groups of data that has something in common
   e.g. Google sorting news acticles; social network analysis
    - Coctail party algorithm - looking for a structure
   