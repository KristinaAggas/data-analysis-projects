![Los Angeles skyline](la_skyline.jpg)

Los Angeles, California 😎. The City of Angels. Tinseltown. The Entertainment Capital of the World! 

Known for its warm weather, palm trees, sprawling coastline, and Hollywood, along with producing some of the most iconic films and songs. However, as with any highly populated city, it isn't always glamorous and there can be a large volume of crime. That's where you can help!

You have been asked to support the Los Angeles Police Department (LAPD) by analyzing crime data to identify patterns in criminal behavior. They plan to use your insights to allocate resources effectively to tackle various crimes in different areas.

## The Data

They have provided you with a single dataset to use. A summary and preview are provided below.

It is a modified version of the original data, which is publicly available from Los Angeles Open Data.

# crimes.csv

| Column     | Description              |
|------------|--------------------------|
| `'DR_NO'` | Division of Records Number: Official file number made up of a 2-digit year, area ID, and 5 digits. |
| `'Date Rptd'` | Date reported - MM/DD/YYYY. |
| `'DATE OCC'` | Date of occurrence - MM/DD/YYYY. |
| `'TIME OCC'` | In 24-hour military time. |
| `'AREA NAME'` | The 21 Geographic Areas or Patrol Divisions are also given a name designation that references a landmark or the surrounding community that it is responsible for. For example, the 77th Street Division is located at the intersection of South Broadway and 77th Street, serving neighborhoods in South Los Angeles. |
| `'Crm Cd Desc'` | Indicates the crime committed. |
| `'Vict Age'` | Victim's age in years. |
| `'Vict Sex'` | Victim's sex: `F`: Female, `M`: Male, `X`: Unknown. |
| `'Vict Descent'` | Victim's descent:<ul><li>`A` - Other Asian</li><li>`B` - Black</li><li>`C` - Chinese</li><li>`D` - Cambodian</li><li>`F` - Filipino</li><li>`G` - Guamanian</li><li>`H` - Hispanic/Latin/Mexican</li><li>`I` - American Indian/Alaskan Native</li><li>`J` - Japanese</li><li>`K` - Korean</li><li>`L` - Laotian</li><li>`O` - Other</li><li>`P` - Pacific Islander</li><li>`S` - Samoan</li><li>`U` - Hawaiian</li><li>`V` - Vietnamese</li><li>`W` - White</li><li>`X` - Unknown</li><li>`Z` - Asian Indian</li> |
| `'Weapon Desc'` | Description of the weapon used (if applicable). |
| `'Status Desc'` | Crime status. |
| `'LOCATION'` | Street address of the crime. |


```python
# Re-run this cell
# Import required libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes.head()
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
      <th>DR_NO</th>
      <th>Date Rptd</th>
      <th>DATE OCC</th>
      <th>TIME OCC</th>
      <th>AREA NAME</th>
      <th>Crm Cd Desc</th>
      <th>Vict Age</th>
      <th>Vict Sex</th>
      <th>Vict Descent</th>
      <th>Weapon Desc</th>
      <th>Status Desc</th>
      <th>LOCATION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>220314085</td>
      <td>2022-07-22</td>
      <td>2020-05-12</td>
      <td>1110</td>
      <td>Southwest</td>
      <td>THEFT OF IDENTITY</td>
      <td>27</td>
      <td>F</td>
      <td>B</td>
      <td>NaN</td>
      <td>Invest Cont</td>
      <td>2500 S  SYCAMORE                     AV</td>
    </tr>
    <tr>
      <th>1</th>
      <td>222013040</td>
      <td>2022-08-06</td>
      <td>2020-06-04</td>
      <td>1620</td>
      <td>Olympic</td>
      <td>THEFT OF IDENTITY</td>
      <td>60</td>
      <td>M</td>
      <td>H</td>
      <td>NaN</td>
      <td>Invest Cont</td>
      <td>3300    SAN MARINO                   ST</td>
    </tr>
    <tr>
      <th>2</th>
      <td>220614831</td>
      <td>2022-08-18</td>
      <td>2020-08-17</td>
      <td>1200</td>
      <td>Hollywood</td>
      <td>THEFT OF IDENTITY</td>
      <td>28</td>
      <td>M</td>
      <td>H</td>
      <td>NaN</td>
      <td>Invest Cont</td>
      <td>1900    TRANSIENT</td>
    </tr>
    <tr>
      <th>3</th>
      <td>231207725</td>
      <td>2023-02-27</td>
      <td>2020-01-27</td>
      <td>0635</td>
      <td>77th Street</td>
      <td>THEFT OF IDENTITY</td>
      <td>37</td>
      <td>M</td>
      <td>H</td>
      <td>NaN</td>
      <td>Invest Cont</td>
      <td>6200    4TH                          AV</td>
    </tr>
    <tr>
      <th>4</th>
      <td>220213256</td>
      <td>2022-07-14</td>
      <td>2020-07-14</td>
      <td>0900</td>
      <td>Rampart</td>
      <td>THEFT OF IDENTITY</td>
      <td>79</td>
      <td>M</td>
      <td>B</td>
      <td>NaN</td>
      <td>Invest Cont</td>
      <td>1200 W  7TH                          ST</td>
    </tr>
  </tbody>
</table>
</div>




```python

# Convert to datetime and extract hour
crimes['hourtime'] = pd.to_datetime(crimes['TIME OCC'], format='%H%M').dt.hour
frequencies = crimes.groupby('hourtime')['Crm Cd Desc'].value_counts()
max_frequency_index = frequencies.idxmax()
peak_crime_hour = max_frequency_index[0]
print(peak_crime_hour)
```

    12
    


```python
night_crimes = crimes[(crimes['hourtime'] >= 22) | (crimes['hourtime'] <= 3)]
area_counts = night_crimes['AREA NAME'].value_counts()
peak_night_crime_location = area_counts.idxmax()
print(peak_night_crime_location)
```

    Central
    


```python
crimes.loc[crimes['Vict Age'] < 0, 'Vict Age'] = np.nan  # exclude negatives
labels = ['0-17', '18-25', '26-34', '35-44', '45-54', '55-64', '65+']
bins = [0, 18, 26, 35, 45, 55, 65, np.inf]
crimes['age_category'] = pd.cut(crimes['Vict Age'], labels=labels, bins=bins, right=False, include_lowest=True)
victim_ages = crimes['age_category'].value_counts().sort_index()
print(victim_ages)
```

    0-17      4528
    18-25    28291
    26-34    47470
    35-44    42157
    45-54    28353
    55-64    20169
    65+      14747
    Name: age_category, dtype: int64
    
