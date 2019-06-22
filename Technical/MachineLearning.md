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

J(theta_0, theta_1) = 1/(2*m) * sum(h(x_i)-y_i)^2
Goal: minimize J

If J == 0, then the hypothesis perfectly describes the system.

Support vector machine 

### Gradient descent
1. Start with some theta_0, theta_1
2. Keep changing theta_0 and theta_1 to reduce J(theta_0, theta_1) until we hopefully we end up in minimum.

Found minimum can be local and depends on the initialisation of the parameters.
Cost function should decrese after EVERY iteration.

alpha - learning rate, determined how big steps we make in the descent direction.
If alpha is very smalle the gradient descent can be slow, if it is too big we can overshoot the minimum and the algorithm can fail to converge or even diverge. As we appraoch minimum, the diveritive will become smaller so there is no need to descrese the alpha - it can be fixed.
theta_j = theta_j - alpha* delta/delta(theta_j){J(theta_0, theta_1) for j=0,1
theta_0 and theta_1 have to be updated simultaneously.

Batch Gradient Descent - each step uses all the trainint examples
For linear regression gradient descent always converge to the global minimum, because the cost function is convex.

### Multiple features
n - number of features
m - number of examples
x_i - vector of features i =1,...,m
x_i_j - jth element of the vector x_i, j=1,...,n

Linear Regression
we add addition x_0 which is always equal to 1. Then:
h(x) = Theta^T * x
where Theta and x are vectors of size n+1

Gradient Descent
Update rule:
theta_j = theta_j - alpha*1/m*sum(h(x_i) - y_i)*x_i_j

Feature Scaling
Scale feature so that they are on similar scale - faster diveragance for the gradient descent
np. x = (feature-average_feature_value)/range_feature_values(or std deviation);
Normaly we want to have features between -1 and 1.

Learning Rate
If J is not decresing, chose smaller alpha
If J goes up and down and so on, chose smaller alpha
if convergence is very small, change alpha to be larger.
Tip: try a range of alpha np. 0.001, 0.003, 0.01, 0.03, 0.1, 0.3 ....

We don't need to use the feature straigth, basing on our knowledge on a problem we can create new features (np. having lenght and width, we can use area)

### Polynomial regression


## Unsupervised learning
Looking for a patterns. There a dataset but without right answers.
 - Clustering - looking for groups of data that has something in common
   e.g. Google sorting news acticles; social network analysis
    - Coctail party algorithm - looking for a structure
   
