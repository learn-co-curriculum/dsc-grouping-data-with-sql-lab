
# Grouping Data with SQL - Lab

## Introduction

In this lab you'll query data from a table populated with Babe Ruth's career hitting statistics.  Then you'll use aggregate functions to pull interesting information from the table that basic queries cannot track. 

## Objectives

* Write queries with aggregate functions like `COUNT`, `MAX`, `MIN`, and `SUM`
* Create an alias for the return value of an aggregate function
* Use `GROUP BY` to sort the data sets returned by aggregate functions
* Compare aggregates using the `HAVING` clause

## Babe Ruth - Career Hitting Statistics


We will query from the `babe_ruth_stats` table featured below.

year|team |league|doubles|triples|hits|HR|games|runs|RBI|at_bats|BB |SB|SO|AVG
----|-----|------|-------|-------|----|--|-----|----|---|-------|---|--|--|------
1914|"BOS"|"AL"  |1      |0      |2   |0 |5    |1   |2  |10     |0  |0 |4 |0.2
1915|"BOS"|"AL"  |10     |1      |29  |4 |42   |16  |21 |92     |9  |0 |23|0.315
1916|"BOS"|"AL"  |5      |3      |37  |3 |67   |18  |15 |136    |10 |0 |23|0.272
1917|"BOS"|"AL"  |6      |3      |40  |2 |52   |14  |12 |123    |12 |0 |18|0.325
1918|"BOS"|"AL"  |26     |11     |95  |11|95   |50  |66 |317    |58 |6 |58|0.3
1919|"BOS"|"AL"  |34     |12     |139 |29|130  |103 |114|432    |101|7 |58|0.322
1920|"NY" |"AL"  |36     |9      |172 |54|142  |158 |137|458    |150|14|80|0.376
1921|"NY" |"AL"  |44     |16     |204 |59|152  |177 |171|540    |145|17|81|0.378
1922|"NY" |"AL"  |24     |8      |128 |35|110  |94  |99 |406    |84 |2 |80|0.315
1923|"NY" |"AL"  |45     |13     |205 |41|152  |151 |131|522    |170|17|93|0.393
1924|"NY" |"AL"  |39     |7      |200 |46|153  |143 |121|529    |142|9 |81|0.378
1925|"NY" |"AL"  |12     |2      |104 |25|98   |61  |66 |359    |59 |2 |68|0.29
1926|"NY" |"AL"  |30     |5      |184 |47|152  |139 |146|495    |144|11|76|0.372
1927|"NY" |"AL"  |29     |8      |192 |60|151  |158 |164|540    |137|7 |89|0.356
1928|"NY" |"AL"  |29     |8      |173 |54|154  |163 |142|536    |137|4 |87|0.323
1929|"NY" |"AL"  |26     |6      |172 |46|135  |121 |154|499    |72 |5 |60|0.345
1930|"NY" |"AL"  |28     |9      |186 |49|145  |150 |153|518    |136|10|61|0.359
1931|"NY" |"AL"  |31     |3      |199 |46|145  |149 |163|534    |128|5 |51|0.373
1932|"NY" |"AL"  |13     |5      |156 |41|133  |120 |137|457    |130|2 |62|0.341
1933|"NY" |"AL"  |21     |3      |138 |34|137  |97  |103|459    |114|4 |90|0.301
1934|"NY" |"AL"  |17     |4      |105 |22|125  |78  |84 |365    |104|1 |63|0.288
1935|"BOS"|"NL"  |0      |0      |13  |6 |28   |13  |12 |72     |20 |0 |24|0.181

## Connect to the Database

Import sqlite3 and pandas. Then, connect to the database and instantiate a cursor.


```python
#Your code here
```


```python
# __SOLUTION__ 
import sqlite3
import pandas as pd
```


```python
# __SOLUTION__ 
conn = sqlite3.connect('babe_ruth.db')
cur = conn.cursor()
```

## Total Seasons
Return the total number of years that Babe Ruth played professional baseball.


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT COUNT(year) AS num_years FROM babe_ruth_stats;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>num_years</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22</td>
    </tr>
  </tbody>
</table>
</div>



## Seasons with NY
Return the total number of years Babe Ruth played with the NY Yankees.


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT COUNT(year) AS num_years
               FROM babe_ruth_stats
               WHERE team ="NY";""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>num_years</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



## Most HR
Select the row with the most HR that Babe Ruth hit in one season


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("SELECT * FROM babe_ruth_stats ORDER BY HR DESC limit 1;")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df

## Alternatively one could also write the following query.
## This includes a subselect, which you will see in an upcoming lesson:
# cur.execute("""SELECT * FROM babe_ruth_stats WHERE HR = (SELECT MAX(HR) FROM babe_ruth_stats);""")
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
      <th>id</th>
      <th>year</th>
      <th>team</th>
      <th>league</th>
      <th>doubles</th>
      <th>triples</th>
      <th>hits</th>
      <th>HR</th>
      <th>games</th>
      <th>runs</th>
      <th>RBI</th>
      <th>at_bats</th>
      <th>BB</th>
      <th>SB</th>
      <th>SO</th>
      <th>AVG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14</td>
      <td>1927</td>
      <td>NY</td>
      <td>AL</td>
      <td>29</td>
      <td>8</td>
      <td>192</td>
      <td>60</td>
      <td>151</td>
      <td>158</td>
      <td>164</td>
      <td>540</td>
      <td>137</td>
      <td>7</td>
      <td>89</td>
      <td>0.356</td>
    </tr>
  </tbody>
</table>
</div>



## Least HR
Select the row with the least number of HR hit in one season.


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("SELECT * FROM babe_ruth_stats ORDER BY HR ASC limit 1;")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df


## Alternatively one could also write the following query.
## This includes a subselect, which you will see in an upcoming lesson:
# cur.execute("""SELECT * FROM babe_ruth_stats WHERE HR = (SELECT MIN(HR) FROM babe_ruth_stats);""")
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
      <th>id</th>
      <th>year</th>
      <th>team</th>
      <th>league</th>
      <th>doubles</th>
      <th>triples</th>
      <th>hits</th>
      <th>HR</th>
      <th>games</th>
      <th>runs</th>
      <th>RBI</th>
      <th>at_bats</th>
      <th>BB</th>
      <th>SB</th>
      <th>SO</th>
      <th>AVG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1914</td>
      <td>BOS</td>
      <td>AL</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



## Total HR
Return the total number of `HR` hit by Babe Ruth during his career


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT sum(HR) FROM babe_ruth_stats;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>sum(HR)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>714</td>
    </tr>
  </tbody>
</table>
</div>



##  Five Worst HR Seasons With at Least 100 Games Played
Above you saw that Babe Ruth hit 0 home runs in his first year when he played only five games.  To avoid this and other extreme  outliers, first filter the data to those years in which Ruth played in at least 100 games. Then, select all of the columns for the 5 worst seasons, in terms of the number of home runs, where he played over 100 games.


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT *
               FROM babe_ruth_stats
               WHERE games > 100
               GROUP BY 1
               ORDER BY HR ASC
               LIMIT 5;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>id</th>
      <th>year</th>
      <th>team</th>
      <th>league</th>
      <th>doubles</th>
      <th>triples</th>
      <th>hits</th>
      <th>HR</th>
      <th>games</th>
      <th>runs</th>
      <th>RBI</th>
      <th>at_bats</th>
      <th>BB</th>
      <th>SB</th>
      <th>SO</th>
      <th>AVG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>1934</td>
      <td>NY</td>
      <td>AL</td>
      <td>17</td>
      <td>4</td>
      <td>105</td>
      <td>22</td>
      <td>125</td>
      <td>78</td>
      <td>84</td>
      <td>365</td>
      <td>104</td>
      <td>1</td>
      <td>63</td>
      <td>0.288</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6</td>
      <td>1919</td>
      <td>BOS</td>
      <td>AL</td>
      <td>34</td>
      <td>12</td>
      <td>139</td>
      <td>29</td>
      <td>130</td>
      <td>103</td>
      <td>114</td>
      <td>432</td>
      <td>101</td>
      <td>7</td>
      <td>58</td>
      <td>0.322</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>1933</td>
      <td>NY</td>
      <td>AL</td>
      <td>21</td>
      <td>3</td>
      <td>138</td>
      <td>34</td>
      <td>137</td>
      <td>97</td>
      <td>103</td>
      <td>459</td>
      <td>114</td>
      <td>4</td>
      <td>90</td>
      <td>0.301</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9</td>
      <td>1922</td>
      <td>NY</td>
      <td>AL</td>
      <td>24</td>
      <td>8</td>
      <td>128</td>
      <td>35</td>
      <td>110</td>
      <td>94</td>
      <td>99</td>
      <td>406</td>
      <td>84</td>
      <td>2</td>
      <td>80</td>
      <td>0.315</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10</td>
      <td>1923</td>
      <td>NY</td>
      <td>AL</td>
      <td>45</td>
      <td>13</td>
      <td>205</td>
      <td>41</td>
      <td>152</td>
      <td>151</td>
      <td>131</td>
      <td>522</td>
      <td>170</td>
      <td>17</td>
      <td>93</td>
      <td>0.393</td>
    </tr>
  </tbody>
</table>
</div>



## Average Batting Average
Select the average, `AVG`, of Ruth's batting averages.  The header of the result would be `AVG(AVG)` which is quite confusing, so provide an alias of `career_average`.


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT AVG(AVG) AS career_average FROM babe_ruth_stats;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>career_average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.322864</td>
    </tr>
  </tbody>
</table>
</div>



## Total Years and Hits Per Team
Select the total number of years played (AS num_years) and total hits (AS total_hits) Babe Ruth had for each team he played for.


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT team, COUNT(year) AS num_years, SUM(hits) AS total_hits
               FROM babe_ruth_stats
               GROUP BY team;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>team</th>
      <th>num_years</th>
      <th>total_hits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BOS</td>
      <td>7</td>
      <td>355</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NY</td>
      <td>15</td>
      <td>2518</td>
    </tr>
  </tbody>
</table>
</div>



## Number of Years with Over 300 Times On Base
We want to know the years in which Ruth successfully reached base over 300 times.  We need to add `hits` and `BB` to calculate how many times Ruth reached base.  Simply add the two columns together (ie: `SELECT [columnName] + [columnName] AS ...`) and give this value an alias of `on_base`.  Select the `year` and `on_base` for only those years with an `on_base` over 300.  


```python
#Your code here
```


```python
# __SOLUTION__ 
cur.execute("""SELECT year, hits + BB AS on_base
               FROM babe_ruth_stats
               GROUP BY year
               HAVING on_base > 300
               ORDER BY on_base DESC;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
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
      <th>year</th>
      <th>on_base</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1923</td>
      <td>375</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1921</td>
      <td>349</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1924</td>
      <td>342</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1927</td>
      <td>329</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1926</td>
      <td>328</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1931</td>
      <td>327</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1920</td>
      <td>322</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1930</td>
      <td>322</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1928</td>
      <td>310</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Well done! In this lab, you continued to add complexity to SQL statements, which included using some aggregate functions. You wrote queries that showed Babe Ruth's total years and home runs per team as well as calculated Babe Ruth's total on base percentage and then selected only years that met a minimum value of our calculated on base attribute. 
