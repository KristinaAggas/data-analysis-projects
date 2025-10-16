**Netflix**! What started in 1997 as a DVD rental service has since exploded into one of the largest entertainment and media companies.

Given the large number of movies and series available on the platform, it is a perfect opportunity to flex your exploratory data analysis skills and dive into the entertainment industry.

You work for a production company that specializes in nostalgic styles. You want to do some research on movies released in the 1990's. You'll delve into Netflix data and perform exploratory data analysis to better understand this awesome movie decade!

You have been supplied with the dataset `netflix_data.csv`, along with the following table detailing the column names and descriptions. Feel free to experiment further after submitting!

## The data
### **netflix_data.csv**
| Column | Description |
|--------|-------------|
| `show_id` | The ID of the show |
| `type` | Type of show |
| `title` | Title of the show |
| `director` | Director of the show |
| `cast` | Cast of the show |
| `country` | Country of origin |
| `date_added` | Date added to Netflix |
| `release_year` | Year of Netflix release |
| `duration` | Duration of the show in minutes |
| `description` | Description of the show |
| `genre` | Show genre |


```python
# Importing pandas and matplotlib
import pandas as pd
import matplotlib.pyplot as plt

# Read in the Netflix CSV as a DataFrame
netflix_df = pd.read_csv("netflix_data.csv")
```


```python
# Start coding here! Use as many cells as you like

# Select relevant columns (optional, not used in filter)
netflix_df[['type', 'release_year', 'duration', 'genre']]


filtered_df = netflix_df.loc[
    (netflix_df['type'] == 'Movie') &
    (netflix_df['release_year'] >= 1990) &
    (netflix_df['release_year'] <= 1999) 
]

filtered_df
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
      <th>show_id</th>
      <th>type</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>duration</th>
      <th>description</th>
      <th>genre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>s8</td>
      <td>Movie</td>
      <td>187</td>
      <td>Kevin Reynolds</td>
      <td>Samuel L. Jackson, John Heard, Kelly Rowan, Cl...</td>
      <td>United States</td>
      <td>November 1, 2019</td>
      <td>1997</td>
      <td>119</td>
      <td>After one of his high school students attacks ...</td>
      <td>Dramas</td>
    </tr>
    <tr>
      <th>118</th>
      <td>s167</td>
      <td>Movie</td>
      <td>A Dangerous Woman</td>
      <td>Stephen Gyllenhaal</td>
      <td>Debra Winger, Barbara Hershey, Gabriel Byrne, ...</td>
      <td>United States</td>
      <td>April 1, 2018</td>
      <td>1993</td>
      <td>101</td>
      <td>At the center of this engrossing melodrama is ...</td>
      <td>Dramas</td>
    </tr>
    <tr>
      <th>145</th>
      <td>s211</td>
      <td>Movie</td>
      <td>A Night at the Roxbury</td>
      <td>John Fortenberry</td>
      <td>Will Ferrell, Chris Kattan, Dan Hedaya, Molly ...</td>
      <td>United States</td>
      <td>December 1, 2019</td>
      <td>1998</td>
      <td>82</td>
      <td>After a run-in with Richard Grieco, dimwits Do...</td>
      <td>Comedies</td>
    </tr>
    <tr>
      <th>167</th>
      <td>s239</td>
      <td>Movie</td>
      <td>A Thin Line Between Love &amp; Hate</td>
      <td>Martin Lawrence</td>
      <td>Martin Lawrence, Lynn Whitfield, Regina King, ...</td>
      <td>United States</td>
      <td>December 1, 2020</td>
      <td>1996</td>
      <td>108</td>
      <td>When a philandering club promoter sets out to ...</td>
      <td>Comedies</td>
    </tr>
    <tr>
      <th>194</th>
      <td>s274</td>
      <td>Movie</td>
      <td>Aashik Awara</td>
      <td>Umesh Mehra</td>
      <td>Saif Ali Khan, Mamta Kulkarni, Mohnish Bahl, S...</td>
      <td>India</td>
      <td>June 1, 2017</td>
      <td>1993</td>
      <td>154</td>
      <td>Raised by a kindly thief, orphaned Jimmy goes ...</td>
      <td>Dramas</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4672</th>
      <td>s7536</td>
      <td>Movie</td>
      <td>West Beirut</td>
      <td>Ziad Doueiri</td>
      <td>Rami Doueiri, Mohamad Chamas, Rola Al Amin, Ca...</td>
      <td>France</td>
      <td>October 19, 2020</td>
      <td>1999</td>
      <td>106</td>
      <td>Three intrepid teens roam the streets of Beiru...</td>
      <td>Dramas</td>
    </tr>
    <tr>
      <th>4689</th>
      <td>s7571</td>
      <td>Movie</td>
      <td>What's Eating Gilbert Grape</td>
      <td>Lasse Hallström</td>
      <td>Johnny Depp, Leonardo DiCaprio, Juliette Lewis...</td>
      <td>United States</td>
      <td>January 1, 2021</td>
      <td>1993</td>
      <td>118</td>
      <td>In a backwater Iowa town, young Gilbert is tor...</td>
      <td>Classic Movies</td>
    </tr>
    <tr>
      <th>4718</th>
      <td>s7624</td>
      <td>Movie</td>
      <td>Wild Wild West</td>
      <td>Barry Sonnenfeld</td>
      <td>Will Smith, Kevin Kline, Kenneth Branagh, Salm...</td>
      <td>United States</td>
      <td>January 1, 2020</td>
      <td>1999</td>
      <td>106</td>
      <td>Armed with an ingenious arsenal, two top-notch...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>4746</th>
      <td>s7682</td>
      <td>Movie</td>
      <td>Wyatt Earp</td>
      <td>Lawrence Kasdan</td>
      <td>Kevin Costner, Dennis Quaid, Gene Hackman, Dav...</td>
      <td>United States</td>
      <td>January 1, 2020</td>
      <td>1994</td>
      <td>191</td>
      <td>Legendary lawman Wyatt Earp is continually at ...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>4756</th>
      <td>s7695</td>
      <td>Movie</td>
      <td>Yaar Gaddar</td>
      <td>Umesh Mehra</td>
      <td>Mithun Chakraborty, Saif Ali Khan, Somy Ali, P...</td>
      <td>India</td>
      <td>July 1, 2017</td>
      <td>1994</td>
      <td>148</td>
      <td>When his brother becomes involved in a deadly ...</td>
      <td>Dramas</td>
    </tr>
  </tbody>
</table>
<p>183 rows × 11 columns</p>
</div>




```python
import pandas as pd
import matplotlib.pyplot as plt

filtered_df['duration'].hist(bins = 100)
plt.title('Histogram of Duration') 
plt.xlabel('Duration') 
plt.ylabel('Frequency') 
plt.show()
```


    
![png](output_3_0.png)
    



```python
netflix_df[['type', 'release_year', 'duration', 'genre']]

genre_filtered_df = netflix_df.loc[
    (netflix_df['type'] == 'Movie') &
    (netflix_df['release_year'] >= 1990) &
    (netflix_df['release_year'] <= 1999) &
    (netflix_df['genre'] == 'Action') &
    (netflix_df['duration'] < 90)
]

genre_filtered_df
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
      <th>show_id</th>
      <th>type</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>duration</th>
      <th>description</th>
      <th>genre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1288</th>
      <td>s2039</td>
      <td>Movie</td>
      <td>EVANGELION: DEATH (TRUE)²</td>
      <td>Hideaki Anno</td>
      <td>Megumi Ogata, Kotono Mitsuishi, Megumi Hayashi...</td>
      <td>Japan</td>
      <td>June 21, 2019</td>
      <td>1998</td>
      <td>69</td>
      <td>Fifteen years after the Second Impact, apathet...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>1698</th>
      <td>s2708</td>
      <td>Movie</td>
      <td>Hero</td>
      <td>Corey Yuen</td>
      <td>Takeshi Kaneshiro, Yuen Biao, Valerie Chow, Je...</td>
      <td>Hong Kong</td>
      <td>August 1, 2018</td>
      <td>1997</td>
      <td>89</td>
      <td>A pugilist from Shantung struggles to rise to ...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>2328</th>
      <td>s3718</td>
      <td>Movie</td>
      <td>Look Out, Officer</td>
      <td>Sze Yu Lau</td>
      <td>Stephen Chow, Bill Tung, Stanley Sui-Fan Fung,...</td>
      <td>Hong Kong</td>
      <td>August 16, 2018</td>
      <td>1990</td>
      <td>88</td>
      <td>An officer killed on the job returns to Earth ...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>2993</th>
      <td>s4787</td>
      <td>Movie</td>
      <td>Passenger 57</td>
      <td>Kevin Hooks</td>
      <td>Wesley Snipes, Bruce Payne, Tom Sizemore, Alex...</td>
      <td>United States</td>
      <td>January 1, 2021</td>
      <td>1992</td>
      <td>84</td>
      <td>Air marshal John Cutter must stop notorious te...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>3297</th>
      <td>s5299</td>
      <td>Movie</td>
      <td>Rumble in the Bronx</td>
      <td>Stanley Tong</td>
      <td>Jackie Chan, Anita Mui, Françoise Yip, Bill Tu...</td>
      <td>Hong Kong</td>
      <td>November 1, 2019</td>
      <td>1995</td>
      <td>89</td>
      <td>During a visit to the Bronx to help out at his...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>3806</th>
      <td>s6114</td>
      <td>Movie</td>
      <td>The Bare-Footed Kid</td>
      <td>Johnnie To</td>
      <td>Aaron Kwok, Lung Ti, Maggie Cheung, Chien-lien...</td>
      <td>Hong Kong</td>
      <td>August 16, 2018</td>
      <td>1993</td>
      <td>83</td>
      <td>While working at a family friend's business, a...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>3943</th>
      <td>s6330</td>
      <td>Movie</td>
      <td>The End of Evangelion</td>
      <td>Hideaki Anno, Kazuya Tsurumaki</td>
      <td>Megumi Ogata, Kotono Mitsuishi, Megumi Hayashi...</td>
      <td>Japan</td>
      <td>June 21, 2019</td>
      <td>1997</td>
      <td>87</td>
      <td>Seele orders an all-out attack on NERV, aiming...</td>
      <td>Action</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Select relevant columns (this line does not modify the DataFrame, so it's not necessary unless for display)
# netflix_df[['type', 'release_year', 'duration', 'genre']]

# Filter the DataFrame
genre_filtered_df = netflix_df.loc[
    (netflix_df['type'] == 'Movie') &
    (netflix_df['release_year'] >= 1990) &
    (netflix_df['release_year'] <= 1999) &
    (netflix_df['genre'] == 'Action') &
    (netflix_df['duration'] < 90)
]

short_duration = genre_filtered_df

print("count_short_duration =", len(short_duration))
```

    count_short_duration = 7
    


```python
duration = 95
short_movie_count = 7
```
