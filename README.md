# Multiomics study of Parkinson's Disease 
### 该文件用来记录帕金森多组学项目的分析过程，包括蛋白组学和代谢组学的统计分析。数据处理用到Python和R
---
![IcarbonX](https://res.cloudinary.com/crunchbase-production/image/upload/c_lpad,h_170,w_170,f_auto,b_white,q_auto:eco,dpr_1/v1481221422/tinzpbzzcjquq5ikhj5m.png)

## 目录
* [数据清理data cleaning](#readme)
  * 蛋白
  * 代谢
* [均一化Normalization](#readme)
* [广义线性模型General linear model](#readme)
* [P值校正FDR](#readme)
* [差异倍数Fold change](#readme)

## 数据清理data cleaning
**蛋白**  
蛋白数据共有三个panel：Neurology，Neuro exploratory，Inflammation，每个panel有两张芯片进行测序。 先将未通过质检的样本和不合格率超过60%的特征删除，再将data根据芯片和PD, Control进行分类。最后用来做均一化的panel有四份数据，分别是PD-panel1，PD-panel2，HC-panel1，HC-panel2.  
**代谢**  
代谢数据分为positive and negative，根据ms2是否测出物质进行数据清理。代谢数据产出时已做过处理，因此不需要再做均一化。  

## 均一化Normalization  
#### normalization of PD
``` python
import pandas as pd
import numpy as np
IF_P1=pd.read_csv('ifp1.csv')#equal to PD-panel1

IF_P1=IF_P1.drop(['UserID','Gender','Age'], axis=1)
IF_P1.head()
```  
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Group</th>
      <th>Plate.ID</th>
      <th>Assay</th>
      <th>IL8</th>
      <th>VEGFA</th>
      <th>CD8A</th>
      <th>MCP.3</th>
      <th>GDNF</th>
      <th>CDCP1</th>
      <th>CD244</th>
      <th>...</th>
      <th>CX3CL1</th>
      <th>TNFRSF9</th>
      <th>NT.3</th>
      <th>TWEAK</th>
      <th>CCL20</th>
      <th>ST1A1</th>
      <th>STAMBP</th>
      <th>ADA</th>
      <th>TNFB</th>
      <th>CSF.1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002311</td>
      <td>4.16889</td>
      <td>10.06486</td>
      <td>7.75988</td>
      <td>0.81916</td>
      <td>NaN</td>
      <td>2.04374</td>
      <td>4.95962</td>
      <td>...</td>
      <td>2.99380</td>
      <td>5.55857</td>
      <td>1.29640</td>
      <td>7.97405</td>
      <td>5.79145</td>
      <td>NaN</td>
      <td>2.69484</td>
      <td>4.53159</td>
      <td>4.10535</td>
      <td>9.11740</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002307</td>
      <td>3.50459</td>
      <td>9.66227</td>
      <td>8.18773</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.91879</td>
      <td>4.99841</td>
      <td>...</td>
      <td>3.28112</td>
      <td>5.83565</td>
      <td>0.76792</td>
      <td>7.50903</td>
      <td>6.10810</td>
      <td>NaN</td>
      <td>3.83273</td>
      <td>5.37031</td>
      <td>3.95246</td>
      <td>9.05924</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002301</td>
      <td>4.41294</td>
      <td>10.67367</td>
      <td>10.28250</td>
      <td>1.65980</td>
      <td>1.66194</td>
      <td>4.43131</td>
      <td>5.52997</td>
      <td>...</td>
      <td>4.15485</td>
      <td>6.92286</td>
      <td>1.09596</td>
      <td>8.60979</td>
      <td>7.41052</td>
      <td>2.04113</td>
      <td>5.01406</td>
      <td>5.45309</td>
      <td>4.23890</td>
      <td>9.72397</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002299</td>
      <td>5.03311</td>
      <td>10.65473</td>
      <td>8.29052</td>
      <td>1.00666</td>
      <td>2.16319</td>
      <td>5.38868</td>
      <td>5.49881</td>
      <td>...</td>
      <td>4.07880</td>
      <td>5.99086</td>
      <td>1.65809</td>
      <td>8.55730</td>
      <td>6.01717</td>
      <td>1.87538</td>
      <td>5.03361</td>
      <td>5.05606</td>
      <td>4.07568</td>
      <td>9.58692</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002292</td>
      <td>4.01711</td>
      <td>10.08239</td>
      <td>6.43367</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.47636</td>
      <td>5.49203</td>
      <td>...</td>
      <td>3.45272</td>
      <td>5.32484</td>
      <td>2.35178</td>
      <td>7.93455</td>
      <td>5.94395</td>
      <td>2.68789</td>
      <td>5.53552</td>
      <td>5.07869</td>
      <td>3.29371</td>
      <td>9.07582</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 79 columns</p>  

```python
IF_C1=pd.read_csv('ifh1.csv')#equal to HC-panel1

IF_C1=IF_C1.drop(['UserID','Gender','Age'], axis=1)
IF_C1.head()
```

<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Group</th>
      <th>Plate.ID</th>
      <th>Assay</th>
      <th>IL8</th>
      <th>VEGFA</th>
      <th>CD8A</th>
      <th>MCP.3</th>
      <th>GDNF</th>
      <th>CDCP1</th>
      <th>CD244</th>
      <th>...</th>
      <th>CX3CL1</th>
      <th>TNFRSF9</th>
      <th>NT.3</th>
      <th>TWEAK</th>
      <th>CCL20</th>
      <th>ST1A1</th>
      <th>STAMBP</th>
      <th>ADA</th>
      <th>TNFB</th>
      <th>CSF.1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002314</td>
      <td>3.89522</td>
      <td>10.18204</td>
      <td>9.14996</td>
      <td>1.09396</td>
      <td>NaN</td>
      <td>1.56627</td>
      <td>5.56602</td>
      <td>...</td>
      <td>3.63245</td>
      <td>6.29676</td>
      <td>0.91995</td>
      <td>8.24908</td>
      <td>5.37870</td>
      <td>NaN</td>
      <td>3.63388</td>
      <td>5.43582</td>
      <td>4.43627</td>
      <td>9.41672</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002312</td>
      <td>3.76095</td>
      <td>10.21934</td>
      <td>8.69257</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.53567</td>
      <td>5.10902</td>
      <td>...</td>
      <td>3.33099</td>
      <td>5.96170</td>
      <td>2.83282</td>
      <td>8.21249</td>
      <td>5.69785</td>
      <td>NaN</td>
      <td>2.93924</td>
      <td>4.95142</td>
      <td>4.06061</td>
      <td>9.21923</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002306</td>
      <td>4.05139</td>
      <td>10.46147</td>
      <td>9.07339</td>
      <td>1.18860</td>
      <td>1.62558</td>
      <td>3.21269</td>
      <td>5.49846</td>
      <td>...</td>
      <td>2.81874</td>
      <td>6.09542</td>
      <td>NaN</td>
      <td>7.99964</td>
      <td>8.20579</td>
      <td>NaN</td>
      <td>3.98320</td>
      <td>5.01403</td>
      <td>4.09171</td>
      <td>9.43764</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002302</td>
      <td>3.92506</td>
      <td>10.23915</td>
      <td>9.01008</td>
      <td>1.18430</td>
      <td>1.29839</td>
      <td>2.44290</td>
      <td>5.38613</td>
      <td>...</td>
      <td>2.71939</td>
      <td>5.55264</td>
      <td>1.69324</td>
      <td>7.93822</td>
      <td>6.30369</td>
      <td>2.48834</td>
      <td>5.82307</td>
      <td>4.73310</td>
      <td>4.05900</td>
      <td>9.32248</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002293</td>
      <td>6.22011</td>
      <td>10.50030</td>
      <td>8.50171</td>
      <td>0.86378</td>
      <td>1.66967</td>
      <td>2.61385</td>
      <td>5.71948</td>
      <td>...</td>
      <td>3.25419</td>
      <td>6.56084</td>
      <td>2.75551</td>
      <td>8.84502</td>
      <td>7.46634</td>
      <td>2.71210</td>
      <td>6.55289</td>
      <td>5.74284</td>
      <td>4.26170</td>
      <td>9.35748</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 79 columns</p>  

#### calculation  
```python
m_c1 =IF_C1.median(axis =0)
x1 = IF_P1 - m_c1 
x1.to_excel(r'C:\Users\mojiayi\Documents\PD\normalization\if\n_IF_P1.xlsx',encoding='utf-8')
```  
#### import the second panel's data
```python
IF_P2=pd.read_csv('ifp2.csv') #equal to PD-panel2

IF_P2=IF_P2.drop(['UserID','Gender','Age'], axis=1)
IF_P2.head()

IF_C2=pd.read_csv('ifh2.csv') #equal to HC-panel2

IF_C2=IF_C2.drop(['UserID','Gender','Age'], axis=1)
IF_C2.head()
```  
```python
m_c2 =IF_C2.median(axis =0)

x2 = IF_P2 - m_c2
x2.to_excel(r'C:\Users\mojiayi\Documents\n_IF_P2.xlsx',encoding='utf-8')
```  

#### normalization of control
```python


x3 = IF_C1 - m_c1
x3
x3.to_excel(r'C:\Users\mojiayi\Documents\n_IF_C1.xlsx',encoding='utf-8')
```

```python


x4 = IF_C2 - m_c2
x4
x4.to_excel(r'C:\Users\mojiayi\Documents\n_IF_C2.xlsx',encoding='utf-8')
```

# 广义线性模型General linear model  

# 差异倍数Fold change  
这里使用的是PD组的mean÷control组的mean，不需要额外分panel  
```python
import pandas as pd
import numpy as np
IF_P1=pd.read_csv('IFPD.csv')

IF_P1=IF_P1.drop(['UserID','Age','Gender'], axis=1)
IF_P1.head()
```
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Group</th>
      <th>Plate ID</th>
      <th>Assay</th>
      <th>IL8</th>
      <th>VEGFA</th>
      <th>CD8A</th>
      <th>MCP-3</th>
      <th>GDNF</th>
      <th>CDCP1</th>
      <th>CD244</th>
      <th>...</th>
      <th>CX3CL1</th>
      <th>TNFRSF9</th>
      <th>NT-3</th>
      <th>TWEAK</th>
      <th>CCL20</th>
      <th>ST1A1</th>
      <th>STAMBP</th>
      <th>ADA</th>
      <th>TNFB</th>
      <th>CSF-1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PD</td>
      <td>GZY_inflammation_plt2</td>
      <td>21PL0002315</td>
      <td>5.29128</td>
      <td>11.02175</td>
      <td>8.52877</td>
      <td>2.22123</td>
      <td>2.05192</td>
      <td>4.88180</td>
      <td>5.97885</td>
      <td>...</td>
      <td>4.10612</td>
      <td>6.93940</td>
      <td>2.13334</td>
      <td>8.39014</td>
      <td>8.15773</td>
      <td>NaN</td>
      <td>4.26375</td>
      <td>5.66733</td>
      <td>4.53015</td>
      <td>9.90687</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PD</td>
      <td>GZY_inflammation_plt2</td>
      <td>21PL0002313</td>
      <td>4.13581</td>
      <td>10.40386</td>
      <td>9.38735</td>
      <td>NaN</td>
      <td>1.77309</td>
      <td>3.04965</td>
      <td>5.49519</td>
      <td>...</td>
      <td>3.50792</td>
      <td>6.15154</td>
      <td>1.17716</td>
      <td>8.25495</td>
      <td>7.25970</td>
      <td>NaN</td>
      <td>3.35403</td>
      <td>5.38768</td>
      <td>3.95088</td>
      <td>9.63697</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002311</td>
      <td>4.16889</td>
      <td>10.06486</td>
      <td>7.75988</td>
      <td>0.81916</td>
      <td>NaN</td>
      <td>2.04374</td>
      <td>4.95962</td>
      <td>...</td>
      <td>2.99380</td>
      <td>5.55857</td>
      <td>1.29640</td>
      <td>7.97405</td>
      <td>5.79145</td>
      <td>NaN</td>
      <td>2.69484</td>
      <td>4.53159</td>
      <td>4.10535</td>
      <td>9.11740</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PD</td>
      <td>GZY_inflammation_plt2</td>
      <td>21PL0002309</td>
      <td>4.09913</td>
      <td>10.17232</td>
      <td>9.43521</td>
      <td>NaN</td>
      <td>2.56456</td>
      <td>3.51403</td>
      <td>6.03388</td>
      <td>...</td>
      <td>3.78566</td>
      <td>5.95378</td>
      <td>1.41323</td>
      <td>8.39196</td>
      <td>6.49964</td>
      <td>NaN</td>
      <td>3.05883</td>
      <td>5.89240</td>
      <td>4.46112</td>
      <td>9.92339</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PD</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002307</td>
      <td>3.50459</td>
      <td>9.66227</td>
      <td>8.18773</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.91879</td>
      <td>4.99841</td>
      <td>...</td>
      <td>3.28112</td>
      <td>5.83565</td>
      <td>0.76792</td>
      <td>7.50903</td>
      <td>6.10810</td>
      <td>NaN</td>
      <td>3.83273</td>
      <td>5.37031</td>
      <td>3.95246</td>
      <td>9.05924</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 79 columns</p>  

```python
IF_C1=pd.read_csv('IFHC.csv')

IF_C1=IF_C1.drop(['UserID','Age','Gender'], axis=1)
IF_C1.head()
```
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Group</th>
      <th>Plate ID</th>
      <th>Assay</th>
      <th>IL8</th>
      <th>VEGFA</th>
      <th>CD8A</th>
      <th>MCP-3</th>
      <th>GDNF</th>
      <th>CDCP1</th>
      <th>CD244</th>
      <th>...</th>
      <th>CX3CL1</th>
      <th>TNFRSF9</th>
      <th>NT-3</th>
      <th>TWEAK</th>
      <th>CCL20</th>
      <th>ST1A1</th>
      <th>STAMBP</th>
      <th>ADA</th>
      <th>TNFB</th>
      <th>CSF-1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002314</td>
      <td>3.89522</td>
      <td>10.18204</td>
      <td>9.14996</td>
      <td>1.09396</td>
      <td>NaN</td>
      <td>1.56627</td>
      <td>5.56602</td>
      <td>...</td>
      <td>3.63245</td>
      <td>6.29676</td>
      <td>0.91995</td>
      <td>8.24908</td>
      <td>5.37870</td>
      <td>NaN</td>
      <td>3.63388</td>
      <td>5.43582</td>
      <td>4.43627</td>
      <td>9.41672</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002312</td>
      <td>3.76095</td>
      <td>10.21934</td>
      <td>8.69257</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.53567</td>
      <td>5.10902</td>
      <td>...</td>
      <td>3.33099</td>
      <td>5.96170</td>
      <td>2.83282</td>
      <td>8.21249</td>
      <td>5.69785</td>
      <td>NaN</td>
      <td>2.93924</td>
      <td>4.95142</td>
      <td>4.06061</td>
      <td>9.21923</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Control</td>
      <td>GZY_inflammation_plt2</td>
      <td>21PL0002310</td>
      <td>4.56666</td>
      <td>10.42998</td>
      <td>10.90433</td>
      <td>0.70731</td>
      <td>1.94193</td>
      <td>3.16281</td>
      <td>6.09306</td>
      <td>...</td>
      <td>3.88250</td>
      <td>6.19146</td>
      <td>1.73360</td>
      <td>8.75609</td>
      <td>5.65447</td>
      <td>NaN</td>
      <td>3.68718</td>
      <td>5.28674</td>
      <td>4.74685</td>
      <td>9.88588</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Control</td>
      <td>GZY_inflammation_plt2</td>
      <td>21PL0002308</td>
      <td>3.77190</td>
      <td>10.16737</td>
      <td>8.55935</td>
      <td>NaN</td>
      <td>1.23819</td>
      <td>2.12033</td>
      <td>5.73973</td>
      <td>...</td>
      <td>3.46738</td>
      <td>5.53094</td>
      <td>0.72240</td>
      <td>8.18269</td>
      <td>5.96445</td>
      <td>NaN</td>
      <td>3.42124</td>
      <td>4.74069</td>
      <td>4.29661</td>
      <td>9.68345</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Control</td>
      <td>GZY_1_inflammation</td>
      <td>21PL0002306</td>
      <td>4.05139</td>
      <td>10.46147</td>
      <td>9.07339</td>
      <td>1.18860</td>
      <td>1.62558</td>
      <td>3.21269</td>
      <td>5.49846</td>
      <td>...</td>
      <td>2.81874</td>
      <td>6.09542</td>
      <td>NaN</td>
      <td>7.99964</td>
      <td>8.20579</td>
      <td>NaN</td>
      <td>3.98320</td>
      <td>5.01403</td>
      <td>4.09171</td>
      <td>9.43764</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 79 columns</p>
</div>


```python
m_P1 =IF_P1.mean(axis =0)
m_C1 =IF_C1.mean(axis =0)

x1 = m_P1 / m_C1
x1.to_excel(r'C:\Users\mojiayi\Documents\PD\normalization\if\FC_IF.xlsx',encoding='utf-8')
```

