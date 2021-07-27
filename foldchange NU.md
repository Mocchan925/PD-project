```python
import pandas as pd
import numpy as np

NU_P1=pd.read_csv('NU_fdPD.csv')

NU_P1=NU_P1.drop(['ID','Groups','Plate','Assay','Age','Gender'], axis=1)
NU_P1.head()
```




<div>
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
      <th>Groups</th>
      <th>ID</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Plate</th>
      <th>Assay</th>
      <th>ADAM.22</th>
      <th>ADAM.23</th>
      <th>Alpha.2.MRAP</th>
      <th>BCAN</th>
      <th>...</th>
      <th>THY.1</th>
      <th>TMPRSS5</th>
      <th>TN.R</th>
      <th>TNFRSF12A</th>
      <th>TNFRSF21</th>
      <th>UNC5C</th>
      <th>VWC2</th>
      <th>WFIKKN1</th>
      <th>gal.8</th>
      <th>sFRP.3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PD</td>
      <td>1001</td>
      <td>75</td>
      <td>0</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002140</td>
      <td>0.398135</td>
      <td>-0.066130</td>
      <td>-0.118115</td>
      <td>0.019495</td>
      <td>...</td>
      <td>0.129225</td>
      <td>-0.267695</td>
      <td>-0.278025</td>
      <td>0.392675</td>
      <td>0.123045</td>
      <td>0.034725</td>
      <td>0.16861</td>
      <td>-0.213245</td>
      <td>0.173355</td>
      <td>0.022520</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PD</td>
      <td>1003</td>
      <td>65</td>
      <td>0</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002142</td>
      <td>0.211435</td>
      <td>0.103375</td>
      <td>-0.127970</td>
      <td>0.045400</td>
      <td>...</td>
      <td>0.013650</td>
      <td>0.192150</td>
      <td>-0.369785</td>
      <td>0.113135</td>
      <td>0.127310</td>
      <td>0.074940</td>
      <td>-0.27951</td>
      <td>0.203345</td>
      <td>-0.956605</td>
      <td>-0.418005</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PD</td>
      <td>1006</td>
      <td>67</td>
      <td>1</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002145</td>
      <td>-0.731535</td>
      <td>-0.509860</td>
      <td>-0.582205</td>
      <td>0.257785</td>
      <td>...</td>
      <td>-0.253065</td>
      <td>0.498935</td>
      <td>0.540285</td>
      <td>-0.277365</td>
      <td>-0.173425</td>
      <td>-0.162795</td>
      <td>-0.57638</td>
      <td>-0.190635</td>
      <td>-0.629815</td>
      <td>0.624650</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PD</td>
      <td>1007</td>
      <td>59</td>
      <td>0</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002146</td>
      <td>-0.019695</td>
      <td>-0.619380</td>
      <td>-0.261215</td>
      <td>0.084155</td>
      <td>...</td>
      <td>-0.219195</td>
      <td>0.279565</td>
      <td>-1.296335</td>
      <td>-0.343545</td>
      <td>-0.111405</td>
      <td>-0.382665</td>
      <td>-0.35717</td>
      <td>0.214665</td>
      <td>-0.381725</td>
      <td>-0.733170</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PD</td>
      <td>1009</td>
      <td>61</td>
      <td>1</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002148</td>
      <td>-0.842925</td>
      <td>0.916845</td>
      <td>0.269870</td>
      <td>-0.231440</td>
      <td>...</td>
      <td>0.307990</td>
      <td>-0.048620</td>
      <td>-0.135205</td>
      <td>0.799875</td>
      <td>0.051070</td>
      <td>0.130150</td>
      <td>-0.18051</td>
      <td>-0.557065</td>
      <td>0.289015</td>
      <td>0.004825</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 95 columns</p>
</div>




```python
NU_C1=pd.read_csv('NU_fdHC.csv')

NU_C1=NU_C1.drop(['ID','Groups','Plate','Assay','Age','Gender'], axis=1)
NU_C1.head()
```




<div>
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
      <th>ADAM.22</th>
      <th>ADAM.23</th>
      <th>Alpha.2.MRAP</th>
      <th>BCAN</th>
      <th>BMP.4</th>
      <th>Beta.NGF</th>
      <th>CADM3</th>
      <th>CD200</th>
      <th>CD200R1</th>
      <th>CD38</th>
      <th>...</th>
      <th>THY.1</th>
      <th>TMPRSS5</th>
      <th>TN.R</th>
      <th>TNFRSF12A</th>
      <th>TNFRSF21</th>
      <th>UNC5C</th>
      <th>VWC2</th>
      <th>WFIKKN1</th>
      <th>gal.8</th>
      <th>sFRP.3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.173145</td>
      <td>-0.093215</td>
      <td>-0.29864</td>
      <td>0.19473</td>
      <td>-0.01148</td>
      <td>-0.05383</td>
      <td>0.539995</td>
      <td>0.467705</td>
      <td>-0.655555</td>
      <td>0.081405</td>
      <td>...</td>
      <td>0.15583</td>
      <td>0.33373</td>
      <td>-0.983725</td>
      <td>0.294525</td>
      <td>0.07360</td>
      <td>-0.06659</td>
      <td>0.20692</td>
      <td>-0.153425</td>
      <td>-0.040925</td>
      <td>0.156905</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.310155</td>
      <td>0.885695</td>
      <td>-0.43717</td>
      <td>0.34167</td>
      <td>0.24165</td>
      <td>0.00267</td>
      <td>0.192275</td>
      <td>0.267005</td>
      <td>-0.567365</td>
      <td>-0.218365</td>
      <td>...</td>
      <td>0.06272</td>
      <td>0.40900</td>
      <td>0.752135</td>
      <td>-0.403735</td>
      <td>0.22327</td>
      <td>0.31485</td>
      <td>-0.39796</td>
      <td>-0.566505</td>
      <td>-0.496815</td>
      <td>-0.274655</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.228085</td>
      <td>0.386005</td>
      <td>-0.28465</td>
      <td>0.11368</td>
      <td>-0.13964</td>
      <td>0.33706</td>
      <td>-0.048845</td>
      <td>0.179925</td>
      <td>0.781505</td>
      <td>0.058685</td>
      <td>...</td>
      <td>-0.21918</td>
      <td>0.69611</td>
      <td>0.354685</td>
      <td>-0.212045</td>
      <td>0.22665</td>
      <td>0.24754</td>
      <td>0.42779</td>
      <td>1.094425</td>
      <td>-0.565155</td>
      <td>0.787195</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.478865</td>
      <td>-0.841035</td>
      <td>-1.05490</td>
      <td>-0.53597</td>
      <td>0.01148</td>
      <td>2.27776</td>
      <td>-0.767885</td>
      <td>-0.696775</td>
      <td>0.195935</td>
      <td>-0.392335</td>
      <td>...</td>
      <td>-0.39327</td>
      <td>-0.12089</td>
      <td>-0.504505</td>
      <td>-0.157715</td>
      <td>-0.52777</td>
      <td>-0.43728</td>
      <td>-0.96303</td>
      <td>-0.289695</td>
      <td>-0.893115</td>
      <td>-0.832465</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.927315</td>
      <td>0.303435</td>
      <td>-0.32345</td>
      <td>-0.45066</td>
      <td>-0.44289</td>
      <td>-0.09031</td>
      <td>-0.570955</td>
      <td>-0.458645</td>
      <td>0.380605</td>
      <td>-1.045395</td>
      <td>...</td>
      <td>-0.13471</td>
      <td>-0.20334</td>
      <td>-0.132165</td>
      <td>-0.092825</td>
      <td>-0.22982</td>
      <td>-0.34896</td>
      <td>-0.47157</td>
      <td>-0.176875</td>
      <td>-0.326395</td>
      <td>-0.265335</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 89 columns</p>
</div>




```python
m_P1 =NU_P1.mean(axis =0)
m_C1 =NU_C1.mean(axis =0)


x1 = m_P1 / m_C1 #fold change calculation
x1
x1.to_excel(r'C:\Users\mojiayi\Documents\FC_NU.xlsx',encoding='utf-8')
```
