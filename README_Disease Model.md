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
#### 若真实模型不在模型集合中，AIC可选择最优模型（即所选模型的泛化误差最小）。因此当研究的目标是预测性的，即希望预测精度高时，可考虑AIC；而当研究的目标是描述性的，即希望刻画自变量和因变量的真实结构时，则可考虑BIC

## LASSO regression  
_In statistics and machine learning, lasso ( least absolute shrinkage and selection operator; also Lasso or LASSO) is a regression analysis method that performs both variable selection and regularization in order to enhance the prediction accuracy and interpretability of the resulting statistical model._  
_LASSO 回归的特点是在拟合广义线性模型的同时进行变量筛选（variable selection）和复杂度调整（regularization)_  
_There are many artical and blog has a good explanation to LASSO, here I recommend this two:_  
 https://blog.csdn.net/orchidzouqr/article/details/53582801  
 https://zhuanlan.zhihu.com/p/360655441

``` r
library(glmnet) #lasso require to split the independent variables and dependent variables to x and y
x<- as.matrix(train[,2:32])
y<- as.matrix(train[,1])
fit <- glmnet(x,y, family = "binomial", nlambda = 100, alpha = 1)#alpha=1 is LASSO regression，=0 is ridge regression 岭回归
plot(fit, xvar = "lambda", label = T)
```
![lasso1](https://user-images.githubusercontent.com/68887901/132811617-c28b2e3b-b26a-48ee-9f98-2cc6129e88fa.png)  
随着lambdas增加, 变量不断减少，部分变量系数变为0（等于没有这个变量）
```R
cvfit = cv.glmnet(x,y, family = "binomial")
plot(cvfit)
```
![lasso2](https://user-images.githubusercontent.com/68887901/132812002-4341d11c-f31b-43db-beec-d3ea2895981b.png)  
这张图我们关注的重点是那两条虚线，分别是最小λ值和最小值一个标准误差的λ值. 把虚线放在上面那张图来看，可以知道lambda值所对应的最简单模型（或最佳）中还剩余多少个变量。  
```R
cvfit$lambda.min#求出最小值 (we don't need this value in most of time)
cvfit$lambda.1se#求出最小值一个标准误的λ值, 在lambda.min一个标准差范围内得到的最简单模型的那一个lambda值。
  #因为lambda值达到一定大小之后，继续增加模型自变量个数及缩小lambda值，并不能显著提高模型性能，lambda.lse给出的就是一个具备优良性能但是自变量个数最少的模型
[1] 0.06730087

l.coef2<-coef(cvfit$glmnet.fit,s=0.01933131,exact = F)
l.coef2
32 x 1 sparse Matrix of class "dgCMatrix"
                       s1
(Intercept)  4.295839e-01
CCL19        2.034575e-01
CX3CL1      -7.262295e-01
CXCL5        2.637342e-02
CXCL6        2.268519e-01
FGF.23       1.106212e-02
IL.17C      -1.459430e-01
IL10         .           
CDH6        -4.792538e-08
CLEC10A      2.541831e-01
CLEC1B       .           
CPA2         3.155745e-01
CPM          .           
N.CDase      .           
NTRK3        .           
BST2         .           
CCL27        .           
DPEP2       -3.184393e-02
EIF4B        .           
GGT5         .           
GPNMB        .           
IL32         .           
KIF1BP       9.000807e-03
NAA10        .           
NEFL        -5.525597e-01
PMVK         .           
PSG1         .           
PSME1        .           
PTPN1        .           
SNCG         .           
TBCB         .           
WWP2         .  
```
### There are still too many (12) significant variables after lasso regression. Though, we can try to use other method to filter more (like using BIC/step again or PCA), its time-consuming and I perfer move to next one. (But LASSO regression works pretty good in Metaomics data (positive mode), so it's largely depends on what type of data it is.)  

## PCA Principal component analysis  
_适用于变量很多，数据维度很高的时候。筛选贡献度最高的变量；但是新变量不依赖于因变量，也许不适合在有疾病组对照组的情况初期使用_
```R
library(factoextra)
library(vegan)
train.pca<-prcomp(train[,-1],scale=TRUE)
#summary(train.pca)#eigenvalue
train.pca$rotation#eigenvectors 
> 
                PC1         PC2          PC3           PC4          PC5          PC6
CCL19    0.10284659 -0.18897991  0.208586090 -0.1009068345  0.374883234 -0.234524912
CX3CL1   0.04309737 -0.31681275  0.076792621 -0.0541482871 -0.257376702 -0.129259029
CXCL5    0.24501599  0.06696369  0.172457656 -0.1731295430  0.115955129  0.152656411
CXCL6    0.24339149  0.03529795  0.202214648 -0.1804002232  0.069178745  0.066959033
FGF.23   0.08323356 -0.02455725  0.085871742  0.0528855444 -0.156839242  0.228780439
IL.17C   0.07606006 -0.23378196  0.045816486 -0.1632980086 -0.279835621 -0.108260737
IL10     0.10119757 -0.15475585  0.116332883 -0.1964594167 -0.169445610 -0.489297916
CDH6     0.07443306 -0.24425932  0.098537324  0.1134851739 -0.403413492 -0.019394597
CLEC10A  0.08289410 -0.21123395  0.309736436  0.0496812262  0.301533773 -0.041976642
CLEC1B   0.29556297  0.12470799  0.065402893 -0.1866861598 -0.009481378  0.104917946
CPA2     0.14655038 -0.15880412  0.215754467  0.1700103620  0.041130137 -0.111513183
CPM      0.08309318 -0.17522351  0.289735017  0.1929277074  0.225528948  0.137757989
N.CDase -0.01216949 -0.11054743  0.083630645  0.2075627366  0.160709089  0.206000811
NTRK3    0.09674191 -0.30691879  0.104518377  0.1127457006 -0.252012681  0.012194106
BST2     0.01626102 -0.10236316  0.121684947  0.3300232706 -0.205318890  0.460780735
CCL27    0.18937500 -0.17077390  0.014640680 -0.0110566539  0.218736472 -0.059185570
DPEP2    0.08428850 -0.10865219 -0.452801551  0.0965567178  0.108435192 -0.085531884
EIF4B    0.27504416  0.18139516 -0.073330129  0.0525956657 -0.156858968 -0.016969233
GGT5     0.08055105 -0.17108963 -0.239829843 -0.0157581425  0.118512159  0.187167255
GPNMB    0.12405443 -0.23840449 -0.301742105  0.0391836147 -0.026021327 -0.076977027
IL32     0.04180330 -0.26206986 -0.331367609  0.1687985512  0.083475017 -0.062755452
KIF1BP   0.23822796  0.04521977 -0.126022779  0.2191108580  0.116158577 -0.145010540
NAA10    0.27872450  0.03578127 -0.173836520  0.1342961730  0.119320558  0.021247218
NEFL     0.09844910 -0.18120930 -0.115246010 -0.4370615102  0.081340929  0.363683858
PMVK     0.28183694  0.17829345 -0.002660453  0.0928362462 -0.151118019 -0.065663623
PSG1     0.13184701 -0.13690815 -0.157545898 -0.4357990022 -0.094096604  0.220002113
PSME1    0.22517224  0.03243270 -0.010425472  0.2636583558 -0.095589730  0.097999371
PTPN1    0.30025416  0.18213873  0.001590027 -0.0141873195 -0.031295578 -0.024072480
SNCG     0.10717718 -0.29131582 -0.094244734 -0.0008532881  0.087897246  0.104663380
TBCB     0.28618646  0.19855282  0.091626813 -0.0357146250 -0.115436455 -0.002690789
WWP2     0.29997010  0.11159794 -0.114974757  0.0786561656  0.049618332 -0.118160993
#variables的绝对值越大对PCX的贡献度越大
#screeplot(train.pca, bstick = TRUE, type = "lines")
fviz_pca_var(train.pca,
             axes = c(1,2),          
             col.var = "contrib", # Color by contributions to the PC
             gradient.cols = c("#00AFBB", "#E7B800", "#FC4E07"),
             repel = TRUE,     # Avoid text overlapping
)
```
![PCA1](https://user-images.githubusercontent.com/68887901/132816112-ea5190cb-3676-4a62-9ca8-01c8fe06bd04.png)  
I decide to select variables based on contribution and PC1 only (since PC2 variation = 17% which is too small to explain the contribution of PC2)  
```R
glm_pca.test <- glm(Group~ PTPN1 + WWP2+ CLEC1B +TBCB + PMVK, data = test, family = binomial) # choose the top 5 contributed variables

#ROC
pre_pca<- predict(glm_pca.test, type ='response')
pca_roc<- roc(test$Group, pre_pca)
plot(pca_roc, print.auc=T, auc.polygon = T, grid = c(0.1,0.2),
     grid.col = c("green", "red"), max.auc.polygon = T,
     auc.polygon.col = "skyblue", print.thres = T, main = "PCA-GLM Model")
```







## Best selection model (the most suitable method and model for all data in our project)

