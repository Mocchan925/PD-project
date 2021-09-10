# PD疾病预测模型构建及变量筛选方法 
### _We have analyzed the features differentially expressed in PD and control groups, now we want to build a disease prediction model for PD diagnosis and select variables with the greatest impact. There are many kind of methods are able to acheive this goal. Herein, I summarized the methods I used for model building and variables selection._
***

## 目录 
### In total, there are x# method. All analysis were performed via R software
* [Stepwise regression 逐步回归]()
* [BIC]()
* [LASSO]()
* [PCA]()
* [Best selection model 最优子集回归]()
  * [AIC]()
  * [BIC]()

## Step regression 
_In statistics, stepwise regression is a method of fitting regression models in which the choice of predictive variables is carried out by an automatic procedure_ 
```R
glm<- glm(Group ~., data=df, family = binomial) #all significant features run for glm
step(glm) #select the smallest AIC model, has 16 variables left

glm_step<- glm(formula = Group.x ~ CCL19 + CX3CL1 + FGF.23 + IL.17C + IL10 + 
                  CDH6 + CLEC10A + CPA2 + BST2 + CCL27 + DPEP2 + NEFL + PMVK + 
                  PTPN1 + TBCB + WWP2, family = binomial, data = df)                  
#ROC
pre<- predict(glm_step, type ='response')
step_roc<- roc(df$Group, pre)
plot(step_roc, print.auc=T, auc.polygon = T, grid = c(0.1,0.2),
     grid.col = c("green", "red"), max.auc.polygon = T,
     auc.polygon.col = "skyblue", print.thres = T, main = "step Model")
```
![step](https://user-images.githubusercontent.com/68887901/132807671-44a602fe-1343-41a4-a989-e9f3001a6227.png)  
**The result looks perfect, but we have to consider if overfitting, and 16 features are still too many, thus step might not be a good method to build model**

## BIC Bayes Information Criteria
_In statistics, the Bayesian information criterion (BIC) or Schwarz information criterion (also SIC, SBC, SBIC) is a criterion for model selection among a finite set of models; the model with the lowest BIC is preferred._  

_Also, the penalty term is larger in BIC than AIC (select variables more strictly)_  

```R
#before using glm, dataset separate into <train> and <test> 
library(caTools)
set.seed(1)
index <-  sample.split(df$Group,SplitRatio = 0.3)
train <- subset(df, index == F)
test <- subset(df, index == T)

library(BMA)
glm_BIC<- bic.glm(Group ~., data=df, glm.family = binomial)
summary(glm_BIC) #model 5 is the best, only 5 features left  

{Call:
 bic.glm.formula(f = Group ~ ., data = train, glm.family = binomial)


   12  models were selected
 Best  5  models (cumulative posterior probability =  0.8109 ): 

           p!=0    EV       SD       model 1    model 2    model 3    model 4    model 5  
Intercept  100     1.06746  0.31679     1.1449     1.0629     0.9716     1.0460     0.9363
CCL19.x     41.4   0.47716  0.62256      .         1.0719     1.2484      .         1.0894
CX3CL1.x    92.2  -1.88301  0.83991    -2.0387    -2.5066    -1.6751    -1.8786    -1.6741
CXCL5.x     19.4   0.10950  0.24310      .          .         0.5444     0.5551      .    
CXCL6.x     51.4   0.48479  0.52562     0.9543      .          .          .         0.7534
IL.17C.x     2.8  -0.02915  0.18311      .          .          .          .          .    
IL10.x       0.0   0.00000  0.00000      .          .          .          .          .    
CDH6.x       2.3  -0.06563  0.45742      .          .          .          .          .    
CLEC10A.x   76.7   1.30351  0.85940     1.7757     1.5026      .         1.7312      .    
CLEC1B.x     2.8   0.01342  0.08820      .          .          .          .          .    
CPM.x        0.0   0.00000  0.00000      .          .          .          .          .    
N.CDase.x    0.0   0.00000  0.00000      .          .          .          .          .    
NTRK3.x      2.6  -0.06330  0.41145      .          .          .          .          .    
BST2.x       0.0   0.00000  0.00000      .          .          .          .          .    
CCL27.x      0.0   0.00000  0.00000      .          .          .          .          .    
DPEP2.x      0.0   0.00000  0.00000      .          .          .          .          .    
EIF4B.x      0.0   0.00000  0.00000      .          .          .          .          .    
GGT5.x       0.0   0.00000  0.00000      .          .          .          .          .    
GPNMB.x      0.0   0.00000  0.00000      .          .          .          .          .    
IL32.x       0.0   0.00000  0.00000      .          .          .          .          .    
KIF1BP.x     3.6   0.02413  0.13607      .          .          .          .          .    
NAA10.x      2.2   0.01032  0.07518      .          .          .          .          .    
NEFL.x     100.0  -1.40523  0.39002    -1.4206    -1.3560    -1.5144    -1.3336    -1.4224
PMVK.x       0.0   0.00000  0.00000      .          .          .          .          .    
PSG1.x       0.0   0.00000  0.00000      .          .          .          .          .    
PSME1.x      0.0   0.00000  0.00000      .          .          .          .          .    
PTPN1.x      0.0   0.00000  0.00000      .          .          .          .          .    
SNCG.x       0.0   0.00000  0.00000      .          .          .          .          .    
TBCB.x       0.0   0.00000  0.00000      .          .          .          .          .    
WWP2.x       0.0   0.00000  0.00000      .          .          .          .          .    
                                                                                          
nVar                                      4          4          4          4          4   
BIC                                  -422.2902  -420.6889  -419.2752  -418.9571  -418.5368
post prob                               0.403      0.181      0.089      0.076      0.062 
}
```  
### Therefore, I choose model 5 as the glm model, lets apply it for test data  
```R 
glm_BIC.test<- glm(Group~ CCL19 + CX3CL1+ CXCL6+ NEFL, data = test, family = binomial)

pre2<- predict(glm_BIC.test, type ='response')
BIC_roc<- roc(test$Group, pre2)
plot(BIC_roc, print.auc=T, auc.polygon = T, grid = c(0.1,0.2),
     grid.col = c("green", "red"), max.auc.polygon = T,
     auc.polygon.col = "skyblue", print.thres = T, main = "BIC Model")
```  
![BIC](https://user-images.githubusercontent.com/68887901/132810246-886382f9-0830-4b40-84d8-a14e8b00c3b3.png)  
**Test data looks good, BIC method is work well for olink data**  

## LASSO regression  
_In statistics and machine learning, lasso ( least absolute shrinkage and selection operator; also Lasso or LASSO) is a regression analysis method that performs both variable selection and regularization in order to enhance the prediction accuracy and interpretability of the resulting statistical model._  
_There are many artical and blog has a good explanation to LASSO, here I recommend this two:_  
 https://blog.csdn.net/orchidzouqr/article/details/53582801  
 https://zhuanlan.zhihu.com/p/360655441

``` r
library(glmnet) #lasso require to split the independent variables and dependent variables to x and y
x<- as.matrix(train[,2:32])
y<- as.matrix(train[,1])
fit <- glmnet(x,y, family = "binomial", nlambda = 100, alpha = 1)#alpha=1 is LASSO regression，=0 is ridge regression 岭回归

```



