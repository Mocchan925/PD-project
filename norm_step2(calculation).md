```python
import pandas as pd
import numpy as np

NU_C1=pd.read_csv('NU_HC1.csv')#control panel 1

NU_C1=NU_C1.drop(['ID'], axis=1)
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
      <th>Groups</th>
      <th>Plate</th>
      <th>Assay</th>
      <th>NMNAT1</th>
      <th>NRP2</th>
      <th>CADM3</th>
      <th>UNC5C</th>
      <th>VWC2</th>
      <th>Siglec-9</th>
      <th>CLM-6</th>
      <th>...</th>
      <th>Dkk-4</th>
      <th>EDA2R</th>
      <th>LAT</th>
      <th>NTRK3</th>
      <th>LAIR-2</th>
      <th>MANF</th>
      <th>TN-R</th>
      <th>CD200R1</th>
      <th>Nr-CAM</th>
      <th>KYNU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Control</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002314</td>
      <td>1.91077</td>
      <td>8.39687</td>
      <td>3.51794</td>
      <td>4.06147</td>
      <td>5.67367</td>
      <td>5.39190</td>
      <td>5.62366</td>
      <td>...</td>
      <td>2.42569</td>
      <td>3.08007</td>
      <td>3.44086</td>
      <td>6.88405</td>
      <td>4.26144</td>
      <td>4.36525</td>
      <td>3.34278</td>
      <td>4.82415</td>
      <td>9.59191</td>
      <td>7.62070</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Control</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002312</td>
      <td>2.36197</td>
      <td>8.02378</td>
      <td>3.79546</td>
      <td>4.26308</td>
      <td>5.98000</td>
      <td>4.91581</td>
      <td>5.78566</td>
      <td>...</td>
      <td>2.83123</td>
      <td>3.89635</td>
      <td>3.55734</td>
      <td>7.39376</td>
      <td>6.74207</td>
      <td>5.85071</td>
      <td>4.58080</td>
      <td>3.32894</td>
      <td>9.66886</td>
      <td>7.97449</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Control</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002306</td>
      <td>2.09977</td>
      <td>8.37552</td>
      <td>3.27357</td>
      <td>4.53933</td>
      <td>5.35892</td>
      <td>5.00158</td>
      <td>6.04559</td>
      <td>...</td>
      <td>2.68087</td>
      <td>4.05830</td>
      <td>4.79220</td>
      <td>7.29347</td>
      <td>8.89640</td>
      <td>6.61865</td>
      <td>4.22364</td>
      <td>4.92523</td>
      <td>9.95764</td>
      <td>8.75418</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Control</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002302</td>
      <td>1.87540</td>
      <td>7.95130</td>
      <td>3.50457</td>
      <td>3.90602</td>
      <td>5.94875</td>
      <td>4.61707</td>
      <td>5.70868</td>
      <td>...</td>
      <td>2.79894</td>
      <td>3.48755</td>
      <td>7.70382</td>
      <td>7.02554</td>
      <td>5.25094</td>
      <td>8.66090</td>
      <td>3.78554</td>
      <td>4.64123</td>
      <td>9.50762</td>
      <td>8.41563</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Control</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002293</td>
      <td>1.75289</td>
      <td>8.02091</td>
      <td>3.92815</td>
      <td>4.36489</td>
      <td>5.62851</td>
      <td>4.73604</td>
      <td>5.46378</td>
      <td>...</td>
      <td>3.50739</td>
      <td>4.11978</td>
      <td>8.50793</td>
      <td>7.71739</td>
      <td>8.16886</td>
      <td>8.67504</td>
      <td>5.03295</td>
      <td>5.02305</td>
      <td>9.78657</td>
      <td>8.33441</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 92 columns</p>
</div>




```python
NU_P1=pd.read_csv('NU_PD1.csv')#PD panel 2

NU_P1=NU_P1.drop(['ID'], axis=1)
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
      <th>Plate</th>
      <th>Assay</th>
      <th>NMNAT1</th>
      <th>NRP2</th>
      <th>CADM3</th>
      <th>UNC5C</th>
      <th>VWC2</th>
      <th>Siglec-9</th>
      <th>CLM-6</th>
      <th>...</th>
      <th>Dkk-4</th>
      <th>EDA2R</th>
      <th>LAT</th>
      <th>NTRK3</th>
      <th>LAIR-2</th>
      <th>MANF</th>
      <th>TN-R</th>
      <th>CD200R1</th>
      <th>Nr-CAM</th>
      <th>KYNU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PD</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002311</td>
      <td>1.56461</td>
      <td>7.95290</td>
      <td>3.84017</td>
      <td>4.05444</td>
      <td>5.40547</td>
      <td>4.83491</td>
      <td>5.62283</td>
      <td>...</td>
      <td>2.83264</td>
      <td>4.38587</td>
      <td>3.87136</td>
      <td>7.53965</td>
      <td>7.74047</td>
      <td>5.45398</td>
      <td>4.45341</td>
      <td>4.25834</td>
      <td>9.72429</td>
      <td>7.80920</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PD</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002307</td>
      <td>2.31349</td>
      <td>8.05353</td>
      <td>2.50702</td>
      <td>3.95827</td>
      <td>4.77082</td>
      <td>5.15978</td>
      <td>5.56458</td>
      <td>...</td>
      <td>2.15288</td>
      <td>2.92588</td>
      <td>3.14001</td>
      <td>7.20466</td>
      <td>5.11591</td>
      <td>4.71511</td>
      <td>4.84390</td>
      <td>3.78038</td>
      <td>9.60932</td>
      <td>7.77465</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PD</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002301</td>
      <td>2.00468</td>
      <td>8.28085</td>
      <td>4.72649</td>
      <td>4.85564</td>
      <td>6.52415</td>
      <td>5.16322</td>
      <td>6.20494</td>
      <td>...</td>
      <td>4.23509</td>
      <td>5.47490</td>
      <td>6.78068</td>
      <td>7.60763</td>
      <td>6.37535</td>
      <td>8.52207</td>
      <td>4.59266</td>
      <td>3.38770</td>
      <td>10.06219</td>
      <td>8.80163</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PD</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002299</td>
      <td>1.88079</td>
      <td>8.21021</td>
      <td>4.43511</td>
      <td>4.39345</td>
      <td>5.43237</td>
      <td>5.22585</td>
      <td>6.14788</td>
      <td>...</td>
      <td>3.47887</td>
      <td>4.64140</td>
      <td>6.93830</td>
      <td>7.80433</td>
      <td>5.21731</td>
      <td>8.42037</td>
      <td>4.58436</td>
      <td>4.01098</td>
      <td>9.99243</td>
      <td>8.39128</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PD</td>
      <td>GZY_neurology_1</td>
      <td>21PL0002292</td>
      <td>2.66506</td>
      <td>8.34825</td>
      <td>3.23773</td>
      <td>4.03117</td>
      <td>5.52732</td>
      <td>5.13635</td>
      <td>5.83905</td>
      <td>...</td>
      <td>3.69640</td>
      <td>4.27648</td>
      <td>8.55399</td>
      <td>7.76460</td>
      <td>4.88085</td>
      <td>8.81729</td>
      <td>4.23497</td>
      <td>4.81316</td>
      <td>9.95055</td>
      <td>8.68339</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 92 columns</p>
</div>




```python
m_c1 =NU_C1.median(axis =0)#get control median 
#print(m_c1)

x1 = NU_P1 - m_c1#calculation 
x1
x1.to_excel(r'C:\Users\mojiayi\Documents\norm_NU_P1.xlsx',encoding='utf-8')
```


```python
NU_C2=pd.read_csv('NU_HC2.csv')#control panel 2

NU_C2=NU_C2.drop(['ID'], axis=1)
NU_C2.head()


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
      <th>Plate</th>
      <th>Assay</th>
      <th>NMNAT1</th>
      <th>NRP2</th>
      <th>CADM3</th>
      <th>UNC5C</th>
      <th>VWC2</th>
      <th>Siglec-9</th>
      <th>CLM-6</th>
      <th>...</th>
      <th>Dkk-4</th>
      <th>EDA2R</th>
      <th>LAT</th>
      <th>NTRK3</th>
      <th>LAIR-2</th>
      <th>MANF</th>
      <th>TN-R</th>
      <th>CD200R1</th>
      <th>Nr-CAM</th>
      <th>KYNU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Control</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002310</td>
      <td>2.49050</td>
      <td>8.03036</td>
      <td>4.08925</td>
      <td>4.61448</td>
      <td>5.97148</td>
      <td>5.05758</td>
      <td>5.68111</td>
      <td>...</td>
      <td>3.27018</td>
      <td>3.86470</td>
      <td>3.69820</td>
      <td>7.28585</td>
      <td>8.75258</td>
      <td>5.80736</td>
      <td>5.08772</td>
      <td>5.15201</td>
      <td>9.65051</td>
      <td>9.03561</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Control</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002308</td>
      <td>2.87115</td>
      <td>7.95803</td>
      <td>3.10248</td>
      <td>3.80825</td>
      <td>6.28186</td>
      <td>5.12704</td>
      <td>5.90989</td>
      <td>...</td>
      <td>2.37544</td>
      <td>3.48095</td>
      <td>3.24858</td>
      <td>6.69010</td>
      <td>5.56540</td>
      <td>4.78290</td>
      <td>4.23009</td>
      <td>4.46301</td>
      <td>9.42627</td>
      <td>7.12412</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Control</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002304</td>
      <td>3.19297</td>
      <td>8.18273</td>
      <td>3.92817</td>
      <td>4.32836</td>
      <td>6.11747</td>
      <td>4.92533</td>
      <td>5.75622</td>
      <td>...</td>
      <td>3.25613</td>
      <td>4.76298</td>
      <td>2.98565</td>
      <td>7.48987</td>
      <td>6.65738</td>
      <td>5.27690</td>
      <td>4.91061</td>
      <td>4.81078</td>
      <td>9.86888</td>
      <td>8.02353</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Control</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002300</td>
      <td>2.12966</td>
      <td>7.90240</td>
      <td>4.07655</td>
      <td>4.32824</td>
      <td>5.76950</td>
      <td>4.66717</td>
      <td>5.60278</td>
      <td>...</td>
      <td>4.18634</td>
      <td>4.51471</td>
      <td>7.79815</td>
      <td>7.78929</td>
      <td>6.17436</td>
      <td>8.63040</td>
      <td>4.42631</td>
      <td>3.56534</td>
      <td>9.77250</td>
      <td>8.12448</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Control</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002298</td>
      <td>2.40775</td>
      <td>8.11736</td>
      <td>2.44187</td>
      <td>4.16469</td>
      <td>4.87183</td>
      <td>5.33044</td>
      <td>5.52633</td>
      <td>...</td>
      <td>2.81783</td>
      <td>3.24937</td>
      <td>7.19021</td>
      <td>7.04411</td>
      <td>5.99317</td>
      <td>8.56447</td>
      <td>5.16916</td>
      <td>5.59792</td>
      <td>9.66775</td>
      <td>10.71130</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 92 columns</p>
</div>




```python
NU_P2=pd.read_csv('NU_PD2.csv')#PD panel 2

NU_P2=NU_P2.drop(['ID'], axis=1)
NU_P2.head()
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
      <th>Plate</th>
      <th>Assay</th>
      <th>NMNAT1</th>
      <th>NRP2</th>
      <th>CADM3</th>
      <th>UNC5C</th>
      <th>VWC2</th>
      <th>Siglec-9</th>
      <th>CLM-6</th>
      <th>...</th>
      <th>Dkk-4</th>
      <th>EDA2R</th>
      <th>LAT</th>
      <th>NTRK3</th>
      <th>LAIR-2</th>
      <th>MANF</th>
      <th>TN-R</th>
      <th>CD200R1</th>
      <th>Nr-CAM</th>
      <th>KYNU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PD</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002315</td>
      <td>3.25262</td>
      <td>7.94450</td>
      <td>3.67470</td>
      <td>4.77793</td>
      <td>6.55306</td>
      <td>5.20366</td>
      <td>6.36646</td>
      <td>...</td>
      <td>3.59556</td>
      <td>5.03090</td>
      <td>3.98446</td>
      <td>7.28432</td>
      <td>6.07873</td>
      <td>5.79584</td>
      <td>4.23493</td>
      <td>4.73392</td>
      <td>9.68826</td>
      <td>9.42285</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PD</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002313</td>
      <td>2.38992</td>
      <td>8.09467</td>
      <td>3.86023</td>
      <td>4.39910</td>
      <td>5.88559</td>
      <td>4.84743</td>
      <td>5.80846</td>
      <td>...</td>
      <td>3.22582</td>
      <td>3.82420</td>
      <td>2.46770</td>
      <td>7.05774</td>
      <td>4.53802</td>
      <td>4.27524</td>
      <td>3.19340</td>
      <td>4.71522</td>
      <td>9.64471</td>
      <td>7.34021</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PD</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002309</td>
      <td>2.87755</td>
      <td>8.33252</td>
      <td>3.88343</td>
      <td>4.19892</td>
      <td>5.62718</td>
      <td>5.09833</td>
      <td>5.65330</td>
      <td>...</td>
      <td>3.09918</td>
      <td>4.26062</td>
      <td>NaN</td>
      <td>7.71083</td>
      <td>7.35140</td>
      <td>4.56327</td>
      <td>4.28899</td>
      <td>4.79340</td>
      <td>9.81762</td>
      <td>9.40570</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PD</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002305</td>
      <td>1.52385</td>
      <td>8.10172</td>
      <td>4.87356</td>
      <td>4.58806</td>
      <td>6.08069</td>
      <td>5.32857</td>
      <td>5.78771</td>
      <td>...</td>
      <td>3.76832</td>
      <td>5.12866</td>
      <td>4.36764</td>
      <td>7.45915</td>
      <td>7.66668</td>
      <td>6.90910</td>
      <td>4.63275</td>
      <td>4.80337</td>
      <td>9.69751</td>
      <td>7.76246</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PD</td>
      <td>GZY_neurology_plt2</td>
      <td>21PL0002303</td>
      <td>2.56219</td>
      <td>7.86001</td>
      <td>3.75368</td>
      <td>4.46940</td>
      <td>5.50746</td>
      <td>4.38825</td>
      <td>6.22893</td>
      <td>...</td>
      <td>3.09700</td>
      <td>4.38300</td>
      <td>3.75561</td>
      <td>7.58702</td>
      <td>5.91902</td>
      <td>6.25795</td>
      <td>3.67373</td>
      <td>3.37270</td>
      <td>9.60827</td>
      <td>9.23976</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 92 columns</p>
</div>




```python
m_c2 =NU_C2.median(axis =0)#get control 2 median 
print(m_c2)

x2 = NU_P2 - m_c2
x2
x2.to_excel(r'C:\Users\mojiayi\Documents\norm_NU_P2.xlsx',encoding='utf-8')
```

    NMNAT1     2.433750
    NRP2       8.117810
    CADM3      3.382840
    UNC5C      4.312285
    VWC2       5.567990
                 ...   
    MANF       8.513630
    TN-R       4.456555
    CD200R1    4.157655
    Nr-CAM     9.629495
    KYNU       8.384980
    Length: 89, dtype: float64
    


```python
#calculate contorl 2 normalization 
x3 = NU_C2 - m_c2
x3
x3.to_excel(r'C:\Users\mojiayi\Documents\norm_NU_C2.xlsx',encoding='utf-8')
```

    NMNAT1     2.433750
    NRP2       8.117810
    CADM3      3.382840
    UNC5C      4.312285
    VWC2       5.567990
                 ...   
    MANF       8.513630
    TN-R       4.456555
    CD200R1    4.157655
    Nr-CAM     9.629495
    KYNU       8.384980
    Length: 89, dtype: float64
    


```python
#calculate control 1 normalization 
x4 = NU_C1 - m_c1
x4
x4.to_excel(r'C:\Users\mojiayi\Documents\norm_NU_C1.xlsx',encoding='utf-8')
```
