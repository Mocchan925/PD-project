# PD疾病预测模型构建及变量筛选方法 
### _We have analyzed the features differentially expressed in PD and control groups, now we want to build a disease prediction model for PD diagnosis and select variables with the greatest impact. There are many kind of methods are able to acheive this goal. Herein, I summarized the methods I used for model building and variables selection._
***

## 目录 
### In total, there are x# method. All analysis were performed via R software
* [Stepwise regression 逐步回归]()
  * [AIC]()
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




