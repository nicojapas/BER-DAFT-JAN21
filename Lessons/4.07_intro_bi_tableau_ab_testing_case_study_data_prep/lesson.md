# Lesson 4.7: Intro to BI & Tableau




```python
import pandas as pd
import numpy as np
pd.set_option('display.max_columns', None)

data1 = pd.read_csv('./ab_testing_case_study_docs/df_final_demo.txt')
print(data1.shape)

data1['client_id'].nunique()  # To check if all the IDs in the data are unique
data1.isna().sum()
data1[data1['clnt_age'].isna()]
data1 = data1.dropna()
data1.to_csv('df_final_demo.csv') # Exporting data to a csv file
```

```python
data2a = pd.read_csv('./ab_testing_case_study_docs/df_final_web_data_pt_1.txt')
data2b = pd.read_csv('./ab_testing_case_study_docs/df_final_web_data_pt_2.txt')

data2a.head()
data2b.head()

data2a.isna().sum()
data2b.isna().sum()

data2a.to_csv('df_final_web_data_pt_1.csv')
data2b.to_csv('df_final_web_data_pt_2.csv')

data2 = pd.concat([data2a, data2b], axis=0)
data2.to_csv('df_final_web_data.csv')
```


### Lesson 4 key concepts


- Merging the two files

```python
data1.head()
data2.head()

data = data2.merge(data1, left_on='client_id', right_on='client_id')
print(data.shape)
data.head()
```

```python
data3 = pd.read_csv('./ab_testing_case_study_docs/df_final_experiment_clients.txt')
data3.head()

data3.isna().sum()
data3 = data3.dropna()

# Checking for unique clients
print(data3.shape)
data3['client_id'].nunique()
```

```python
data = data.merge(data3, left_on='client_id', right_on='client_id')
data3.to_csv('df_final_experiment_clients.csv')

# As you can see, again there is information size drop from ~450k to ~320k.
# You can find out who were the clients whose
# information was available on data3 but not in data2.

data['date_time'] = pd.to_datetime(data['date_time'])
data.to_csv('finalMergedFile.csv')
data.shape

df = data.copy()  # we will use df to analyse data with python later
```


