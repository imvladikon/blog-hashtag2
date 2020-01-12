---
layout: post
title:  "How to deal with csv, import to sql and so on"
date:   2020-01-12 14:18:16 +0200
categories: 
description: 
tags: python pandas
---

Let's do it something with CSV ;)
<!--break-->
Let's talk today how to deal with CSV. and pandas will help us. 


```python
import pandas as pd
```

There is good package for pandas - [swifter](https://github.com/jmcarpenter2/swifter) 
A package which efficiently applies any function to a pandas dataframe or series in the fastest available manner.


```python
# !pip install swifter
```

I've already installed package, that's why code was commented above. 


```python
import swifter
```

Also we can treat with pandas dataframe like sql data. For that we need another one package - pandasql. For installing: !pip install pandasql . 


```python
import pandasql as ps
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql
    


```python
url = """https://raw.githubusercontent.com/NLPH/NLPH_Resources/master/code/VerbInflector/resources/The%20Verb%20Index.csv"""
```


```python
col_names = ["name", "binyan", "type"]
df = pd.read_csv(url, error_bad_lines=False,header=None, skipinitialspace=True, names=col_names, dtype={'binyan':'category'})
df.head(2)
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
      <th>name</th>
      <th>binyan</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>אבד</td>
      <td>A</td>
      <td>16</td>
    </tr>
    <tr>
      <td>1</td>
      <td>אבזר</td>
      <td>C</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



unique values in the each columns


```python
pd.DataFrame.from_records([(col, df[col].nunique()) for col in df.columns],
                          columns=['Column_Name', 'Num_Unique']).sort_values(by=['Num_Unique'])
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
      <th>Column_Name</th>
      <th>Num_Unique</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>binyan</td>
      <td>7</td>
    </tr>
    <tr>
      <td>2</td>
      <td>type</td>
      <td>60</td>
    </tr>
    <tr>
      <td>0</td>
      <td>name</td>
      <td>3530</td>
    </tr>
  </tbody>
</table>
</div>



We could replace or map values of a column to values, that are more convenient for us


```python
labels = df['binyan'].astype('category').cat.categories.tolist()
replace_map = {'binyan' : {k: v for k,v in zip(labels,list(range(1,len(labels)+1)))}}
replace_map
```




    {'binyan': {'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7}}




```python
replace_map_name = {'binyan': {'A': 'paal', 'B': 'nifal', 'C': 'piel', 'D': 'pual',
                                  'E': 'hitpael', 'F': 'hifil', 'G': 'hufal'}}
```


```python
df.replace(replace_map_name, inplace=True)
df
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
      <th>name</th>
      <th>binyan</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>אבד</td>
      <td>paal</td>
      <td>16</td>
    </tr>
    <tr>
      <td>1</td>
      <td>אבזר</td>
      <td>piel</td>
      <td>20</td>
    </tr>
    <tr>
      <td>2</td>
      <td>אבחן</td>
      <td>piel</td>
      <td>25</td>
    </tr>
    <tr>
      <td>3</td>
      <td>אבטח</td>
      <td>piel</td>
      <td>26</td>
    </tr>
    <tr>
      <td>4</td>
      <td>אגד</td>
      <td>paal</td>
      <td>14</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>4242</td>
      <td>תרגם</td>
      <td>piel</td>
      <td>20</td>
    </tr>
    <tr>
      <td>4243</td>
      <td>תרם</td>
      <td>paal</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4244</td>
      <td>תשאל</td>
      <td>piel</td>
      <td>24</td>
    </tr>
    <tr>
      <td>4245</td>
      <td>תש</td>
      <td>paal</td>
      <td>40</td>
    </tr>
    <tr>
      <td>4246</td>
      <td>תשש</td>
      <td>paal</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>4247 rows × 3 columns</p>
</div>




```python
q1 = """SELECT * FROM df WHERE binyan='paal' and name LIKE 'א%' """
ps.sqldf(q1, locals()).head(10)
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
      <th>name</th>
      <th>binyan</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>אבד</td>
      <td>paal</td>
      <td>16</td>
    </tr>
    <tr>
      <td>1</td>
      <td>אגד</td>
      <td>paal</td>
      <td>14</td>
    </tr>
    <tr>
      <td>2</td>
      <td>אגף</td>
      <td>paal</td>
      <td>14</td>
    </tr>
    <tr>
      <td>3</td>
      <td>אגר</td>
      <td>paal</td>
      <td>14</td>
    </tr>
    <tr>
      <td>4</td>
      <td>אהב</td>
      <td>paal</td>
      <td>21</td>
    </tr>
    <tr>
      <td>5</td>
      <td>אהד</td>
      <td>paal</td>
      <td>20</td>
    </tr>
    <tr>
      <td>6</td>
      <td>אזל</td>
      <td>paal</td>
      <td>15</td>
    </tr>
    <tr>
      <td>7</td>
      <td>אזר</td>
      <td>paal</td>
      <td>14</td>
    </tr>
    <tr>
      <td>8</td>
      <td>אחז</td>
      <td>paal</td>
      <td>20</td>
    </tr>
    <tr>
      <td>9</td>
      <td>אטם</td>
      <td>paal</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>



We could get data also from sql database. For example from postgresql. Sure, we need install drivers' packages. !pip install psycopg2 , !pip install pyodbc. And let's connect (it's just connection string: database_driver://user:password@host/dbname ):


```python
%sql postgresql+psycopg2://postgres:123@localhost/hebrew
```




    'Connected: postgres@hebrew'




```python
result = %sql SELECT * FROM generate_series('2020-01-01 00:00'::timestamp, '2020-02-04 12:00', '10 hours') as date;
df = result.DataFrame()
```

     * postgresql+psycopg2://postgres:***@localhost/hebrew
    83 rows affected.
    


```python
df.head(5)
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
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2020-01-01 00:00:00</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2020-01-01 10:00:00</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2020-01-01 20:00:00</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2020-01-02 06:00:00</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2020-01-02 16:00:00</td>
    </tr>
  </tbody>
</table>
</div>



As we see, it's really easy to get data to dataframe.


```python
url = """https://raw.githubusercontent.com/NLPH/NLPH_Resources/master/code/VerbInflector/resources/Inflected%20verbs%20Extended.txt"""
```


```python
col_names = ["binyan", "type", "inflection", "properties", "verb"]
df_infl = pd.read_csv(url, error_bad_lines=False,header=None, skipinitialspace=True, names=col_names, dtype={'binyan':'category'})
df_infl.head(2)
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
      <th>binyan</th>
      <th>type</th>
      <th>inflection</th>
      <th>properties</th>
      <th>verb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>A</td>
      <td>1</td>
      <td>בָּגַדְתִּי</td>
      <td>PAST+FIRST+MF+SINGULAR+COMPLETE</td>
      <td>בָּגַד</td>
    </tr>
    <tr>
      <td>1</td>
      <td>A</td>
      <td>1</td>
      <td>בָּגַדְתָּ</td>
      <td>PAST+SECOND+M+SINGULAR+COMPLETE</td>
      <td>בָּגַד</td>
    </tr>
  </tbody>
</table>
</div>



In the properties column we have data with '+' separator. But we want to split it. Let's do it: 


```python
df_infl[['form','number', 'gender', 'number', 'property']] = df_infl['properties'].str.split('+',expand=True)
```


```python
df_infl.head(2)
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
      <th>binyan</th>
      <th>type</th>
      <th>inflection</th>
      <th>properties</th>
      <th>verb</th>
      <th>form</th>
      <th>number</th>
      <th>gender</th>
      <th>property</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>A</td>
      <td>1</td>
      <td>בָּגַדְתִּי</td>
      <td>PAST+FIRST+MF+SINGULAR+COMPLETE</td>
      <td>בָּגַד</td>
      <td>PAST</td>
      <td>SINGULAR</td>
      <td>MF</td>
      <td>COMPLETE</td>
    </tr>
    <tr>
      <td>1</td>
      <td>A</td>
      <td>1</td>
      <td>בָּגַדְתָּ</td>
      <td>PAST+SECOND+M+SINGULAR+COMPLETE</td>
      <td>בָּגַד</td>
      <td>PAST</td>
      <td>SINGULAR</td>
      <td>M</td>
      <td>COMPLETE</td>
    </tr>
  </tbody>
</table>
</div>



And we can push it to our sql server really easy

```python
import sqlalchemy
database_username = 'username'
database_password = 'password'
database_ip       = 'host'
database_name     = 'dbname'
conn = sqlalchemy.create_engine('postgresql+psycopg2://{0}:{1}@{2}/{3}'.format(database_username, database_password, database_ip, database_name))
df_infl(con=conn, name='tablename', if_exists='append')
```


```python

```


```python

```
