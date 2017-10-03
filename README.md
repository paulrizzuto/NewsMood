# News Mood

- Each news sources has moderately negative sentiment at the time of analysis.
- The most negative and most positive tweets occurred within the last 20 tweets.
- It would seem that a majority of each news sources tweets' were graded with neutral compound sentiment (0).


```python
consumer_key = "r1CGEJjGlrTmuUILfpJOYboYX"
consumer_secret = "PaAN0u5xUEB6FW59ZtQw9DHS95WWj1d9jzchbxXFVGQrGpvA4R"
access_token = "901933530258829312-y9BaHYc1jigRcWrWuGxcs0eYomJMY12"
access_token_secret = "KCSTd4QoPgiPwtYo0MSo185ACE2vPS5WLOrqnAd3xFsxx"
```


```python
import tweepy
import json
import numpy as np
import pandas as pd
import seaborn
import matplotlib.pyplot as plt
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
import time
```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
tweet_df = pd.DataFrame(columns = ["Source Account", "Tweet Text", "Date", "Tweets Ago", "Compound Sentiment", "Positive Sentiment", "Neutral Sentiment", "Negative Sentiment"])
```


```python
target_user = ["BBCNews", "CBSNews", "CNN", "FoxNews", "nytimes"]
indexcount = 0
comp_avg = []
```


```python
for user in target_user:
    public_tweets = api.user_timeline(user, count=100)
    tweetnumber = 0
    comp_list = []
    for tweet in public_tweets:
        search = tweet["text"]
        tweetdate = tweet["created_at"]
        compoundsent = analyzer.polarity_scores(search)["compound"]
        comp_list.append(analyzer.polarity_scores(search)["compound"])
        positivesent = analyzer.polarity_scores(search)["pos"]
        neutralsent = analyzer.polarity_scores(search)["neu"]
        negativesent = analyzer.polarity_scores(search)["neg"]
        tweet_df.set_value(indexcount, "Source Account", user)
        tweet_df.set_value(indexcount, "Tweet Text", search)
        tweet_df.set_value(indexcount, "Date", tweetdate)
        tweet_df.set_value(indexcount, "Tweets Ago", tweetnumber)
        tweet_df.set_value(indexcount, "Compound Sentiment", compoundsent)            
        tweet_df.set_value(indexcount, "Positive Sentiment", positivesent)
        tweet_df.set_value(indexcount, "Neutral Sentiment", neutralsent)
        tweet_df.set_value(indexcount, "Negative Sentiment", negativesent)
        indexcount = indexcount + 1
        tweetnumber = tweetnumber + 1
    comp_avg.append(np.mean(comp_list))
```


```python
comp_avg
```




    [-0.14169799999999999,
     -0.11824999999999999,
     -0.135129,
     -0.13838100000000003,
     -0.13440000000000002]




```python
tweet_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Source Account</th>
      <th>Tweet Text</th>
      <th>Date</th>
      <th>Tweets Ago</th>
      <th>Compound Sentiment</th>
      <th>Positive Sentiment</th>
      <th>Neutral Sentiment</th>
      <th>Negative Sentiment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BBCNews</td>
      <td>RT @BBCRoryCJ: Uber + TfL met at a secret loca...</td>
      <td>Tue Oct 03 16:08:03 +0000 2017</td>
      <td>0</td>
      <td>0.4877</td>
      <td>0.143</td>
      <td>0.857</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BBCNews</td>
      <td>RT @BBCNewsbeat: The government wants to give ...</td>
      <td>Tue Oct 03 15:59:02 +0000 2017</td>
      <td>1</td>
      <td>-0.2023</td>
      <td>0</td>
      <td>0.924</td>
      <td>0.076</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BBCNews</td>
      <td>Boris Johnson: Let the British lion roar https...</td>
      <td>Tue Oct 03 15:51:06 +0000 2017</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BBCNews</td>
      <td>Tuition fee changes 'to save students £15,700'...</td>
      <td>Tue Oct 03 15:21:53 +0000 2017</td>
      <td>3</td>
      <td>0.4939</td>
      <td>0.314</td>
      <td>0.686</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BBCNews</td>
      <td>Landfill search for airman to restart https://...</td>
      <td>Tue Oct 03 15:21:50 +0000 2017</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>BBCNews</td>
      <td>"We are the party that believes in this countr...</td>
      <td>Tue Oct 03 15:20:41 +0000 2017</td>
      <td>5</td>
      <td>0.4019</td>
      <td>0.184</td>
      <td>0.816</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>BBCNews</td>
      <td>RT @BBCBreaking: Police to resume search of la...</td>
      <td>Tue Oct 03 15:19:50 +0000 2017</td>
      <td>6</td>
      <td>-0.296</td>
      <td>0</td>
      <td>0.879</td>
      <td>0.121</td>
    </tr>
    <tr>
      <th>7</th>
      <td>BBCNews</td>
      <td>RT @BBCBreaking: US expels 15 Cuban diplomats,...</td>
      <td>Tue Oct 03 15:19:45 +0000 2017</td>
      <td>7</td>
      <td>-0.7351</td>
      <td>0.088</td>
      <td>0.612</td>
      <td>0.299</td>
    </tr>
    <tr>
      <th>8</th>
      <td>BBCNews</td>
      <td>"It is time to be bold, to seize the opportuni...</td>
      <td>Tue Oct 03 15:05:25 +0000 2017</td>
      <td>8</td>
      <td>0.6369</td>
      <td>0.286</td>
      <td>0.714</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>BBCNews</td>
      <td>"We must believe in global Britain" - @BorisJo...</td>
      <td>Tue Oct 03 14:53:35 +0000 2017</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>BBCNews</td>
      <td>RT @BBCSport: England players Eniola Aluko and...</td>
      <td>Tue Oct 03 14:21:11 +0000 2017</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>BBCNews</td>
      <td>Follow the latest news from #CPC17 in Manchest...</td>
      <td>Tue Oct 03 13:46:32 +0000 2017</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>BBCNews</td>
      <td>Scottish government backs ban on fracking http...</td>
      <td>Tue Oct 03 13:40:23 +0000 2017</td>
      <td>12</td>
      <td>-0.5859</td>
      <td>0</td>
      <td>0.51</td>
      <td>0.49</td>
    </tr>
    <tr>
      <th>13</th>
      <td>BBCNews</td>
      <td>RT @BBCNormanS: David Davis accuses unnamed EU...</td>
      <td>Tue Oct 03 13:39:05 +0000 2017</td>
      <td>13</td>
      <td>-0.6597</td>
      <td>0</td>
      <td>0.707</td>
      <td>0.293</td>
    </tr>
    <tr>
      <th>14</th>
      <td>BBCNews</td>
      <td>UK aiming for "a good deal" on #Brexit but is ...</td>
      <td>Tue Oct 03 13:38:59 +0000 2017</td>
      <td>14</td>
      <td>0.6369</td>
      <td>0.224</td>
      <td>0.776</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>BBCNews</td>
      <td>Stourbridge stabbing: Aaron Barley admits murd...</td>
      <td>Tue Oct 03 13:25:05 +0000 2017</td>
      <td>15</td>
      <td>-0.5423</td>
      <td>0.185</td>
      <td>0.42</td>
      <td>0.395</td>
    </tr>
    <tr>
      <th>16</th>
      <td>BBCNews</td>
      <td>Jenna Miller 'killed by drivers in 70mph race'...</td>
      <td>Tue Oct 03 13:25:04 +0000 2017</td>
      <td>16</td>
      <td>-0.6705</td>
      <td>0</td>
      <td>0.69</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>17</th>
      <td>BBCNews</td>
      <td>RT @ChrisMasonBBC: Pensions campaigners outsid...</td>
      <td>Tue Oct 03 13:00:59 +0000 2017</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>BBCNews</td>
      <td>RT @BBCScotlandNews: The UK government has for...</td>
      <td>Tue Oct 03 12:45:14 +0000 2017</td>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>BBCNews</td>
      <td>RT @BBCNormanS: Universal Credit heading towar...</td>
      <td>Tue Oct 03 12:45:05 +0000 2017</td>
      <td>19</td>
      <td>-0.4404</td>
      <td>0.118</td>
      <td>0.633</td>
      <td>0.249</td>
    </tr>
    <tr>
      <th>20</th>
      <td>BBCNews</td>
      <td>RT @BBCNormanS: Liam Fox says BBC coverage "tr...</td>
      <td>Tue Oct 03 12:28:42 +0000 2017</td>
      <td>20</td>
      <td>-0.296</td>
      <td>0</td>
      <td>0.833</td>
      <td>0.167</td>
    </tr>
    <tr>
      <th>21</th>
      <td>BBCNews</td>
      <td>Conservative fears of a downward spiral — blog...</td>
      <td>Tue Oct 03 12:22:48 +0000 2017</td>
      <td>21</td>
      <td>-0.4215</td>
      <td>0</td>
      <td>0.763</td>
      <td>0.237</td>
    </tr>
    <tr>
      <th>22</th>
      <td>BBCNews</td>
      <td>RT @BBCEngland: An imam accused of making Isla...</td>
      <td>Tue Oct 03 12:15:19 +0000 2017</td>
      <td>22</td>
      <td>-0.296</td>
      <td>0</td>
      <td>0.896</td>
      <td>0.104</td>
    </tr>
    <tr>
      <th>23</th>
      <td>BBCNews</td>
      <td>Rodney Bickerstaffe, ex-Unision general secret...</td>
      <td>Tue Oct 03 12:04:58 +0000 2017</td>
      <td>23</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>BBCNews</td>
      <td>Teenager charged after Ellon Academy incident ...</td>
      <td>Tue Oct 03 12:04:55 +0000 2017</td>
      <td>24</td>
      <td>-0.2023</td>
      <td>0</td>
      <td>0.769</td>
      <td>0.231</td>
    </tr>
    <tr>
      <th>25</th>
      <td>BBCNews</td>
      <td>RT @daily_politics: Jacob Rees-Mogg confronted...</td>
      <td>Tue Oct 03 12:04:00 +0000 2017</td>
      <td>25</td>
      <td>-0.2023</td>
      <td>0</td>
      <td>0.899</td>
      <td>0.101</td>
    </tr>
    <tr>
      <th>26</th>
      <td>BBCNews</td>
      <td>RT @DannyShawBBC: More on acid: Sulphuric acid...</td>
      <td>Tue Oct 03 11:59:41 +0000 2017</td>
      <td>26</td>
      <td>-0.4588</td>
      <td>0</td>
      <td>0.87</td>
      <td>0.13</td>
    </tr>
    <tr>
      <th>27</th>
      <td>BBCNews</td>
      <td>RT @BBCBreaking: Sale of acids to people aged ...</td>
      <td>Tue Oct 03 11:39:36 +0000 2017</td>
      <td>27</td>
      <td>-0.4588</td>
      <td>0</td>
      <td>0.87</td>
      <td>0.13</td>
    </tr>
    <tr>
      <th>28</th>
      <td>BBCNews</td>
      <td>Sale of acids to under-18s to be banned https:...</td>
      <td>Tue Oct 03 11:38:28 +0000 2017</td>
      <td>28</td>
      <td>-0.4588</td>
      <td>0</td>
      <td>0.727</td>
      <td>0.273</td>
    </tr>
    <tr>
      <th>29</th>
      <td>BBCNews</td>
      <td>Former women's college fields all-male Univers...</td>
      <td>Tue Oct 03 11:36:10 +0000 2017</td>
      <td>29</td>
      <td>0.0772</td>
      <td>0.14</td>
      <td>0.86</td>
      <td>0</td>
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
    </tr>
    <tr>
      <th>470</th>
      <td>nytimes</td>
      <td>Tech companies should act decisively to preven...</td>
      <td>Tue Oct 03 00:47:03 +0000 2017</td>
      <td>70</td>
      <td>-0.296</td>
      <td>0.067</td>
      <td>0.793</td>
      <td>0.14</td>
    </tr>
    <tr>
      <th>471</th>
      <td>nytimes</td>
      <td>RT @nytimesmusic: CBS erroneously reports Tom ...</td>
      <td>Tue Oct 03 00:32:02 +0000 2017</td>
      <td>71</td>
      <td>-0.5994</td>
      <td>0</td>
      <td>0.769</td>
      <td>0.231</td>
    </tr>
    <tr>
      <th>472</th>
      <td>nytimes</td>
      <td>Ariana Grande, whose concert was struck by a s...</td>
      <td>Tue Oct 03 00:17:04 +0000 2017</td>
      <td>72</td>
      <td>-0.836</td>
      <td>0</td>
      <td>0.656</td>
      <td>0.344</td>
    </tr>
    <tr>
      <th>473</th>
      <td>nytimes</td>
      <td>RT @nytopinion: A Congressman thought his coll...</td>
      <td>Tue Oct 03 00:02:06 +0000 2017</td>
      <td>73</td>
      <td>-0.6597</td>
      <td>0</td>
      <td>0.812</td>
      <td>0.188</td>
    </tr>
    <tr>
      <th>474</th>
      <td>nytimes</td>
      <td>GM plans at least 20 all-electric vehicles by ...</td>
      <td>Mon Oct 02 23:47:02 +0000 2017</td>
      <td>74</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>475</th>
      <td>nytimes</td>
      <td>“I’ve been a nurse for 30 years, and this was ...</td>
      <td>Mon Oct 02 23:32:03 +0000 2017</td>
      <td>75</td>
      <td>-0.8481</td>
      <td>0</td>
      <td>0.687</td>
      <td>0.313</td>
    </tr>
    <tr>
      <th>476</th>
      <td>nytimes</td>
      <td>A musician who was at the Las Vegas festival s...</td>
      <td>Mon Oct 02 23:17:05 +0000 2017</td>
      <td>76</td>
      <td>0.2023</td>
      <td>0.142</td>
      <td>0.752</td>
      <td>0.106</td>
    </tr>
    <tr>
      <th>477</th>
      <td>nytimes</td>
      <td>RT @UpshotNYT: Another look at what might work...</td>
      <td>Mon Oct 02 23:02:06 +0000 2017</td>
      <td>77</td>
      <td>-0.3182</td>
      <td>0.076</td>
      <td>0.759</td>
      <td>0.166</td>
    </tr>
    <tr>
      <th>478</th>
      <td>nytimes</td>
      <td>Equifax believes up to 145.5 million people ma...</td>
      <td>Mon Oct 02 22:47:06 +0000 2017</td>
      <td>78</td>
      <td>-0.4939</td>
      <td>0</td>
      <td>0.856</td>
      <td>0.144</td>
    </tr>
    <tr>
      <th>479</th>
      <td>nytimes</td>
      <td>Last year, her husband forgot their anniversar...</td>
      <td>Mon Oct 02 22:32:04 +0000 2017</td>
      <td>79</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>480</th>
      <td>nytimes</td>
      <td>In Gaza, a reconciliation may be happening bet...</td>
      <td>Mon Oct 02 22:18:05 +0000 2017</td>
      <td>80</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>481</th>
      <td>nytimes</td>
      <td>Fact Check: Viral claims and rumors in the Las...</td>
      <td>Mon Oct 02 22:07:05 +0000 2017</td>
      <td>81</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>482</th>
      <td>nytimes</td>
      <td>One NYT reader's reaction to the mass shooting...</td>
      <td>Mon Oct 02 21:55:56 +0000 2017</td>
      <td>82</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>483</th>
      <td>nytimes</td>
      <td>Incomplete records of Puerto Rico's landscape ...</td>
      <td>Mon Oct 02 21:41:30 +0000 2017</td>
      <td>83</td>
      <td>-0.1027</td>
      <td>0</td>
      <td>0.931</td>
      <td>0.069</td>
    </tr>
    <tr>
      <th>484</th>
      <td>nytimes</td>
      <td>RT @palafo: When they heard the shooting, Sonn...</td>
      <td>Mon Oct 02 21:32:04 +0000 2017</td>
      <td>84</td>
      <td>0.4939</td>
      <td>0.132</td>
      <td>0.868</td>
      <td>0</td>
    </tr>
    <tr>
      <th>485</th>
      <td>nytimes</td>
      <td>After defying the Madrid government, separatis...</td>
      <td>Mon Oct 02 21:15:07 +0000 2017</td>
      <td>85</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>486</th>
      <td>nytimes</td>
      <td>RT @nytopinion: Let’s mourn. But even more imp...</td>
      <td>Mon Oct 02 20:56:05 +0000 2017</td>
      <td>86</td>
      <td>-0.1796</td>
      <td>0.132</td>
      <td>0.692</td>
      <td>0.176</td>
    </tr>
    <tr>
      <th>487</th>
      <td>nytimes</td>
      <td>One NYT reader's reaction to the mass shooting...</td>
      <td>Mon Oct 02 20:46:04 +0000 2017</td>
      <td>87</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>488</th>
      <td>nytimes</td>
      <td>Watch the controlled explosion that brought do...</td>
      <td>Mon Oct 02 20:35:05 +0000 2017</td>
      <td>88</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>489</th>
      <td>nytimes</td>
      <td>What makes Singapore's health care so cheap? h...</td>
      <td>Mon Oct 02 20:25:02 +0000 2017</td>
      <td>89</td>
      <td>0.4939</td>
      <td>0.314</td>
      <td>0.686</td>
      <td>0</td>
    </tr>
    <tr>
      <th>490</th>
      <td>nytimes</td>
      <td>The duration of the bursts in Las Vegas sugges...</td>
      <td>Mon Oct 02 20:15:13 +0000 2017</td>
      <td>90</td>
      <td>-0.4404</td>
      <td>0</td>
      <td>0.879</td>
      <td>0.121</td>
    </tr>
    <tr>
      <th>491</th>
      <td>nytimes</td>
      <td>The Las Vegas gunman had 19 rifles in his hote...</td>
      <td>Mon Oct 02 20:08:35 +0000 2017</td>
      <td>91</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>492</th>
      <td>nytimes</td>
      <td>Oct. 1, 2017\nJuly 7, 2016\nJune 12, 2016\nDec...</td>
      <td>Mon Oct 02 20:05:06 +0000 2017</td>
      <td>92</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>493</th>
      <td>nytimes</td>
      <td>Some blood donors in Las Vegas were in line as...</td>
      <td>Mon Oct 02 19:55:04 +0000 2017</td>
      <td>93</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>494</th>
      <td>nytimes</td>
      <td>"This world is sick." Musicians from the Las V...</td>
      <td>Mon Oct 02 19:45:07 +0000 2017</td>
      <td>94</td>
      <td>-0.6705</td>
      <td>0</td>
      <td>0.791</td>
      <td>0.209</td>
    </tr>
    <tr>
      <th>495</th>
      <td>nytimes</td>
      <td>Thousands of young undocumented immigrants are...</td>
      <td>Mon Oct 02 19:35:07 +0000 2017</td>
      <td>95</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>496</th>
      <td>nytimes</td>
      <td>“No one has experienced patient volumes to thi...</td>
      <td>Mon Oct 02 19:25:07 +0000 2017</td>
      <td>96</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>497</th>
      <td>nytimes</td>
      <td>Opinion: "Let’s mourn. But even more important...</td>
      <td>Mon Oct 02 19:15:10 +0000 2017</td>
      <td>97</td>
      <td>-0.1796</td>
      <td>0.162</td>
      <td>0.62</td>
      <td>0.217</td>
    </tr>
    <tr>
      <th>498</th>
      <td>nytimes</td>
      <td>Eric Paddock, the Las Vegas suspect’s brother,...</td>
      <td>Mon Oct 02 19:05:08 +0000 2017</td>
      <td>98</td>
      <td>-0.0516</td>
      <td>0.097</td>
      <td>0.796</td>
      <td>0.106</td>
    </tr>
    <tr>
      <th>499</th>
      <td>nytimes</td>
      <td>Caleb Keeter of Josh Abbott Band said that the...</td>
      <td>Mon Oct 02 18:55:03 +0000 2017</td>
      <td>99</td>
      <td>-0.34</td>
      <td>0</td>
      <td>0.902</td>
      <td>0.098</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 8 columns</p>
</div>




```python
#export to csv for final file
```


```python
x_axis = (100, 0, 1)
bbcplot = plt.scatter(tweet_df[tweet_df["Source Account"] == "BBCNews"]["Tweets Ago"], tweet_df[tweet_df["Source Account"] == "BBCNews"]["Compound Sentiment"], marker="o", c="turquoise", edgecolors="black", linewidth=1 ,s=125, alpha=1, label="BBC")
cbsplot = plt.scatter(tweet_df[tweet_df["Source Account"] == "CBSNews"]["Tweets Ago"], tweet_df[tweet_df["Source Account"] == "CBSNews"]["Compound Sentiment"], marker="o", facecolors="green", edgecolors="black", linewidth=1, s=125, alpha=1, label="CBS")
cnnplot = plt.scatter(tweet_df[tweet_df["Source Account"] == "CNN"]["Tweets Ago"], tweet_df[tweet_df["Source Account"] == "CNN"]["Compound Sentiment"], marker="o", facecolors="red", edgecolors="black", linewidth=1,s=125, alpha=1, label="CNN")
foxplot = plt.scatter(tweet_df[tweet_df["Source Account"] == "FoxNews"]["Tweets Ago"], tweet_df[tweet_df["Source Account"] == "FoxNews"]["Compound Sentiment"], marker="o", facecolors="blue", edgecolors="black", linewidth=1,s=125, alpha=1, label="Fox")
nytplot = plt.scatter(tweet_df[tweet_df["Source Account"] == "nytimes"]["Tweets Ago"], tweet_df[tweet_df["Source Account"] == "nytimes"]["Compound Sentiment"], marker="o", facecolors="yellow", edgecolors="black", linewidth=1,s=125, alpha=1, label="New York Times")
leg = plt.legend(title="Media Sources",fontsize=12, bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.setp(leg.get_title(),fontsize=14)
bbox_inches="tight"
plt.title("Sentiment Analysis of Media Tweets " + str(time.strftime("%x")), fontsize=16)
plt.xlabel("Tweets Ago", fontsize=14)
plt.ylabel("Tweet Polarity", fontsize=14)
plt.ylim(-1.1, 1.1)
plt.xlim(105, -5)
plt.show()
```


![png](output_11_0.png)



```python
#tweet_df.groupby('Source Account')["Compound Sentiment"].mean()
tweet_df.info()

```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 500 entries, 0 to 499
    Data columns (total 8 columns):
    Source Account        500 non-null object
    Tweet Text            500 non-null object
    Date                  500 non-null object
    Tweets Ago            500 non-null object
    Compound Sentiment    500 non-null object
    Positive Sentiment    500 non-null object
    Neutral Sentiment     500 non-null object
    Negative Sentiment    500 non-null object
    dtypes: object(8)
    memory usage: 55.2+ KB



```python
plt.figure(figsize=(10,7))
x_axis2 = np.arange(len(target_user))

rect1 = plt.bar(0, comp_avg[0], color='turquoise', alpha=1, align="edge", ec="black", width=1)
rect2 = plt.bar(1, comp_avg[1], color='green', alpha=1, align="edge", ec="black", width=1)
rect3 = plt.bar(2, comp_avg[2], color='red', alpha=1, align="edge", ec="black", width=1)
rect4 = plt.bar(3, comp_avg[3], color='blue', alpha=1, align="edge", ec="black", width=1)
rect5 = plt.bar(4, comp_avg[4], color='yellow', alpha=1, align="edge", ec="black", width=1)

tick_locations = [value+0.5 for value in x_axis2]
plt.grid(linestyle="dashed")
plt.xticks(tick_locations, target_user)
plt.xlim(0, 5)
plt.ylim(-.2, .05)
```




    (-0.2, 0.05)




```python
plt.title("Overall Media Sentiment Based on Twitter " + str(time.strftime("%x")), fontsize=20)
plt.ylabel("Tweet Polarity")
```




    <matplotlib.text.Text at 0x117fa07b8>




```python
def label_negative(rects):
    for rect in rects:
        height = rect.get_height()
        plt.text(rect.get_x() + rect.get_width()/2., (height)-0.01,
                '-%.2f' % float(height),
                ha='center', va='bottom')


label_negative(rect1)
label_negative(rect2)
label_negative(rect3)
label_negative(rect4)
label_negative(rect5)
```


```python
plt.show()
```


![png](output_16_0.png)



```python

```


```python

```
