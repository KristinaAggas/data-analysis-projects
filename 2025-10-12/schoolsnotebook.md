Every year, American high school students take SATs, which are standardized tests intended to measure literacy, numeracy, and writing skills. There are three sections - reading, math, and writing, each with a **maximum score of 800 points**. These tests are extremely important for students and colleges, as they play a pivotal role in the admissions process.

Analyzing the performance of schools is important for a variety of stakeholders, including policy and education professionals, researchers, government, and even parents considering which school their children should attend. 

You have been provided with a dataset called `schools.csv`, which is previewed below.

You have been tasked with answering three key questions about New York City (NYC) public school SAT performance.


```python
# Re-run this cell 
import pandas as pd

# Read in the data
schools = pd.read_csv("schools.csv")

# Preview the data
schools.head()

# Start coding here...
# Add as many cells as you like...
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
      <th>school_name</th>
      <th>borough</th>
      <th>building_code</th>
      <th>average_math</th>
      <th>average_reading</th>
      <th>average_writing</th>
      <th>percent_tested</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>New Explorations into Science, Technology and ...</td>
      <td>Manhattan</td>
      <td>M022</td>
      <td>657</td>
      <td>601</td>
      <td>601</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Essex Street Academy</td>
      <td>Manhattan</td>
      <td>M445</td>
      <td>395</td>
      <td>411</td>
      <td>387</td>
      <td>78.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lower Manhattan Arts Academy</td>
      <td>Manhattan</td>
      <td>M445</td>
      <td>418</td>
      <td>428</td>
      <td>415</td>
      <td>65.1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>High School for Dual Language and Asian Studies</td>
      <td>Manhattan</td>
      <td>M445</td>
      <td>613</td>
      <td>453</td>
      <td>463</td>
      <td>95.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Henry Street School for International Studies</td>
      <td>Manhattan</td>
      <td>M056</td>
      <td>410</td>
      <td>406</td>
      <td>381</td>
      <td>59.7</td>
    </tr>
  </tbody>
</table>
</div>




```python
best_math_schools = schools[schools["average_math"] >= 640][["school_name", "average_math"]].sort_values("average_math", ascending=False)
print(best_math_schools)
```

                                               school_name  average_math
    88                              Stuyvesant High School           754
    170                       Bronx High School of Science           714
    93                 Staten Island Technical High School           711
    365  Queens High School for the Sciences at York Co...           701
    68   High School for Mathematics, Science, and Engi...           683
    280                     Brooklyn Technical High School           682
    333                        Townsend Harris High School           680
    174  High School of American Studies at Lehman College           669
    0    New Explorations into Science, Technology and ...           657
    45                       Eleanor Roosevelt High School           641
    


```python
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']
top_10_schools = schools[["school_name", "total_SAT"]].sort_values("total_SAT", ascending=False).head(10)
print(top_10_schools)
```

                                               school_name  total_SAT
    88                              Stuyvesant High School       2144
    170                       Bronx High School of Science       2041
    93                 Staten Island Technical High School       2041
    174  High School of American Studies at Lehman College       2013
    333                        Townsend Harris High School       1981
    365  Queens High School for the Sciences at York Co...       1947
    5                       Bard High School Early College       1914
    280                     Brooklyn Technical High School       1896
    45                       Eleanor Roosevelt High School       1889
    68   High School for Mathematics, Science, and Engi...       1889
    


```python
borough = schools.groupby("borough")["total_SAT"].agg(["count", "mean", "std"]).round(2)
largest_std_dev = borough[borough["std"] == borough["std"].max()]
largest_std_dev = largest_std_dev.rename(columns={"count": "num_schools", "mean": "average_SAT", "std": "std_SAT"})
print(largest_std_dev)
```

               num_schools  average_SAT  std_SAT
    borough                                     
    Manhattan           89      1340.13   230.29
    
