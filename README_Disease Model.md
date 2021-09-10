# PD疾病预测模型构建及变量筛选方法 
***We have analyzed the features differentially expressed in PD and control groups, now we want to build a disease prediction model for PD diagnosis and select variables with the most significant impact. There are many kinds of methods that are able to achieve this goal. Herein, I summarized the methods I used for model building and variables selection.***  

## 目录 
**In total, there are 5 method for data filtering. All analysis were performed via R software**
* [Stepwise regression 逐步回归](https://github.com/Mocchan925/PD-project/blob/main/README_Disease%20Model.md#step-regression)
* [BIC](https://github.com/Mocchan925/PD-project/blob/main/README_Disease%20Model.md#bic-bayes-information-criteria)
* [LASSO](https://github.com/Mocchan925/PD-project/blob/main/README_Disease%20Model.md#lasso-regression)
* [PCA](https://github.com/Mocchan925/PD-project/blob/main/README_Disease%20Model.md#pca-principal-component-analysis)
* [All-Subsets Regression 最优子集回归](https://github.com/Mocchan925/PD-project/blob/main/README_Disease%20Model.md#all-subsets-regression-%E5%85%A8%E5%AD%90%E9%9B%86%E5%9B%9E%E5%BD%92%E6%9C%80%E4%BC%98%E5%AD%90%E9%9B%86%E5%9B%9E%E5%BD%92-%E4%B8%AA%E4%BA%BA%E8%AE%A4%E4%B8%BA%E8%BF%99%E4%B8%AA%E6%96%B9%E6%B3%95%E5%8F%AF%E4%BB%A5%E9%80%82%E5%90%88%E7%BB%9D%E5%A4%A7%E9%83%A8%E5%88%86%E6%95%B0%E6%8D%AE)
  * AIC
  * BIC
* [Summary](https://github.com/Mocchan925/PD-project/blob/main/README_Disease%20Model.md#summary)

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
**Therefore, I choose model 5 as the glm model, lets apply it for test data**  
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
![pca2](https://user-images.githubusercontent.com/68887901/132819093-69e269ec-acb2-40a9-abd7-a5761001e6c0.png)  
先用PCA筛选变量再建模的方法不是很理想  

## All-Subsets Regression 全子集回归/最优子集回归 (个人认为这个方法可以适合绝大部分数据)  
从含相同自变量的个数的所有模型中挑选出最佳的模型组合，比如ABCD四个自变量，可以有2^4=16个模型。在进行模型比较时，R^2、校正的R^2越大，Cp值越小模型越佳。  
HOWEVER, 此模型对x变量的数量有严格的限制，<= 15. 所以当变量超过15个时候，需要先筛选变量再进行全子集回归
```R
#As I mentioned above, we need to reduce the number of variables, thus I decide to use LASSO regression to decrease the size of data.
#LASSO's result shows that there were 12 variables left 

library(bestglm)
bestglmdata<-train[,grepl("CCL19|CX3CL1|CXCL5|CXCL6|FGF.23|IL.17C|CDH6|CLEC10A|CPA2|DEPE2|KIF1BP|NEFL|Group",colnames(test))]
#从train测试集选择LASSO得到的变量

bestglmdata$Group2 = bestglmdata$Group#需要把预测变量放在最后一列
bestglmdata<- bestglmdata[,-1]
> head(bestglmdata)
       CCL19  CX3CL1     CXCL5     CXCL6   FGF.23    IL.17C     CDH6  CLEC10A      CPA2
4  -1.281365 0.63210 -2.282955 -1.540605  0.00100 -0.862450  0.48026  0.04021  0.858315
6  -0.255070 0.23732 -0.123230 -0.532760 -0.03938 -0.115565 -0.23008 -0.25294 -0.160530
7   0.008170 0.33039 -2.428760 -1.493970 -0.17637 -0.479345  0.06825 -0.36958 -0.391330
11  0.274985 1.09955  1.206805  0.559865  1.04646  0.488320  0.46556  0.27205 -0.623805
13 -1.164145 0.27350 -1.676125 -0.338335  0.00100 -0.282380 -0.04071 -0.73903 -0.853945
15  0.732395 1.16840  1.351355  1.033545  0.67861  0.913290  0.54722  0.42172  0.805955
      KIF1BP     NEFL Group2
4   0.628935 -1.92378      1
6  -0.856870  0.17208      0
7  -0.686070  0.13700      0
11  0.628800  1.74827      0
13 -0.811760 -0.05656      0
15  0.718770  1.24900      0

> bestglm(bestglmdata, IC="AIC", family = binomial)
Morgan-Tatar search since family is non-gaussian.
AIC
BICq equivalent for q in (0.797402653128122, 0.897025357116168)
Best Model:
              Estimate Std. Error   z value     Pr(>|z|)
(Intercept)  1.4973944  0.3856588  3.882692 0.0001033064
CCL19        1.2107446  0.5211561  2.323190 0.0201689412
CX3CL1      -2.4852003  0.8563437 -2.902106 0.0037066300
CXCL6        0.5341992  0.3857986  1.384658 0.1661569799
FGF.23       2.3749295  1.2674750  1.873749 0.0609650770
IL.17C      -0.9442943  0.5224299 -1.807504 0.0706836891
CDH6        -2.6242213  1.3831637 -1.897260 0.0577936098
CLEC10A      1.4983159  0.6435582  2.328175 0.0199028268
CPA2         1.4611707  0.5450868  2.680620 0.0073485911
NEFL        -1.6766196  0.4666092 -3.593198 0.0003266438
```  
AIC的结果变量数量还是有点多（9个），再看看BIC的

```R
> bestglm(bestglmdata, IC="BIC", family = binomial)
Morgan-Tatar search since family is non-gaussian.
BIC
BICq equivalent for q in (0.374234562902754, 0.673130451812243)
Best Model:
             Estimate Std. Error   z value     Pr(>|z|)
(Intercept)  1.352033  0.3669236  3.684780 0.0002289004
CCL19        1.371708  0.5028895  2.727653 0.0063786650
CX3CL1      -3.093897  0.8304486 -3.725573 0.0001948719
FGF.23       2.348351  1.1711532  2.005161 0.0449458487
IL.17C      -1.205494  0.5286815 -2.280190 0.0225964019
CLEC10A      1.482149  0.6498895  2.280618 0.0225710758
CPA2         1.376348  0.5117724  2.689375 0.0071585864
NEFL        -1.479233  0.4238824 -3.489724 0.0004835192
```  
BIC筛选得出7个变量，建立回归模型看一下  

![bestglm bic](https://user-images.githubusercontent.com/68887901/132826742-db829168-3a88-4c24-83f0-aec92d47a0c2.png)  
**和第二个用BMA包直接根据BIC系数分析得到的结果相比，两者没有什么区别(AUC = 0.906;AUC = 0.909)。 前者最后筛选得到的变量（四个）比全子集回归中经过LASSO回归得到的变量（7个）少。但全子集回归的弊端是变量不能超过15个**  

## Summary  
* Step regression 得到的模型overfitting的几率较大
* LASSO regression是目前进行变量筛选最流行的方法之一，但也面临数据过拟合的可能性，对于不同数据表现结果不同
* PCA 分析也是非常常用的降维方法，对数据的要求比较高
* 全子集回归模型考虑的情况最多，将所有变量的回归模型都历变，根据AIC/BIC 系数选择最佳的model，对数据类型的要求较低；但限制是变量不能超过15个
* About AIC&BIC: 整体来看BIC的预测结果最佳，因为BIC的惩罚程度比AIC大，对回归模型的限制更多；然而当研究的目标是预测性的，即希望预测精度高时，可考虑AIC；当研究的目标是描述性的，即希望刻画自变量和因变量的真实结构时，则可考虑BIC。对与建立疾病预测模型来看，AIC可能更加合适。但是统计学方法可塑性强，只要能自圆其说，选择用什么方法要依据数据的情况判断。

 


