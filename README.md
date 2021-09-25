## Spam Account Detection
### Data Description
Fakes and spammers are a major problem on all social media platforms, including Instagram. [kaggler](https://www.kaggle.com/free4ever1/instagram-fake-spammer-genuine-accounts) has personally identified the spammer/fake accounts included in this dataset after carefully examining each instance and as such the dataset has high level of accuracy. The dataset has been collected using a crawler from 15-19, March 2019. The response variable is fake, and is coded 0=genuine and 1=spam. There are 11 predictors in the data:

+ profile picture, user has profile picture, or not (coded 0=no picture, 1=has picture)
+ nums length user, this is the ratio of number of numerical chars in username to its length
+ full name words, full name in word tokens
+ nums length full name, the ratio of the number of numerical characters in full name to its length
+ name username, are username and full name literally the same (coded 0=not same, 1=same)
+ description length, bio length in characters
+ external url, has external url or not (coded 0=no external url, 1=has external url)
+ private or not, is the account set to  private viewer ship (coded 0=not private, 1=private)
+ posts, number of posts
+ followers, number of followers
+ follows, number of accounts followed

### Split Data
Before building these models, we will split the data into one set that will be used to develop models, preprocess the predictors, and explore relationships among the predictors and the response (the training set) and another that will be the final arbiter of the predictor set/model combination performance (the test set). To partition the data, the splitting of the orignal data set will be done in a stratified manner by making random splits in each of the outcome classes. This will keep the proportion of fake accounts approximately the same. In the splitting, 82% of the data were allocated to the training set. 10-fold cross-validation were used to tune the models

### Prepprocess
The predictors were transformed using yeoJohnson transformations, centered and scaled, searched for near zero variance predictors, and searched for a predictor space with kendal correlation less than .80.

### Modeling
While often overlooked, the metric used to assess the effectiveness of a model to predict the outcome is very important and can influence the conclusions. The metrics select to evaluate model performance depend on the response variable. The outcome being a binary categorical variable. Accuracy, sensitivity, specificity, kappa, and area under the curve were used to evaluate each model's performance. \

The models tried on this data include logistic regression, linear discriminant analysis, regularized discriminant analysis, flexible discriminant analysis, k-nearest neighbors, single-layer neural network, and C5 boosted trees.

### Logistic Model
This models was created usinhg the penalized version (elastic net) that conducts feature selection during model training. 10 values of the ridge and lasso penalty combinations were examined, using a space filling design.

### LDA Model
This models was created using the penalized version that conducts feature selection during model training. 10 values of the ridge penalty was investigated, using a space filling design.

### RDA Model
This model was created using a space filling design with 10 combination values for fraction common variance that toggles between LDA and QDA by putting a penalty on the covariance matrix $\lambda \Sigma_l + (1-\lambda)\Sigma$ when $\lambda$ is zero we get LDA and when $\lambda$ is 1 we get QDA. For $\lambda$ values between 0 and 1 we get something between LDA and QDA. Fraction identity buts a penalty on the pooled covariance matrix and allows the matrix to morph from its observed value to one where the predictors are assumed to be indepenent. Tuning an RDA model over these parameters enables the training data to decide the most appropriate  assumptions for the model.

### FDA Model
This model, first-degree and second-degree MARS hinge functions were used and the number of retained terms was varied from 2 to 12.

### KNN Model
this model, k varied from 1 to 10.

### NNet Model
Models were fit with hidden units ranging from 2 to 10 and 10 weight decay values determined using a space filling design.

### C5.0 Boosted Model
This model was evaluated with tree-based models, up to 100 iterations of boosting and tuned over sample_size and min_n.

### Training Results
The model results are shown below. The plot show the confidence interval of the resamples over the different metrics. The linear models did just as well as the non-linear models. The best non-linear model is the neural network and the best linear model is the logistic regression. To see if these two models are statistically different a t-test can be performed. I did not do this in this analysis.

![](https://github.com/ModelBehavior/Shawn_Portfolio/blob/main/images/Screen%20Shot%202021-09-25%20at%2012.59.51%20PM.png)

# Results
The best two models were logistic regression and the single-layer neural network. We can see our models dropped in accuracy quite a bit on the testing data, and the logistic regression predicted one more class correctly than the single-layer neural network. The logistic regression model misclassified 12 observations, while the single-layer neural network misclassified 13. Given the simplicity of the logistic regression model, it would be chosen as the overall model. The logistic regression has a false positive rate of 10.77%. And a true positive rate of 91.8%. Meaning when the model predicts genuine, it is right 91.8% of the time, and when the model predicts spam, it is wrong 10.77% of the time.

![](https://github.com/ModelBehavior/Shawn_Portfolio/blob/main/images/Screen%20Shot%202021-09-25%20at%201.02.23%20PM.png)
