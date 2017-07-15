---
layout: post
draft: true
title: "gdeltPyR and GDELT: Tracking World Events with Analytics and News Metadata" 
comments: true
author:
  twitter: linwoodc3
  dribbble:
  email: valinvescap@gmail.com
  facebook: 
  flickr:
  github: linwoodc3
  instagram:
  linkedin: https://www.linkedin.com/in/linwood-creekmore-iii-a2174538
  pinterest:
  rss: datajourney
  twitter: linwoodc3
  stackoverflow: users/4205207/linwoodc3
  youtube: # channel/<your_long_string> or user/<user-name>
  googleplus: u/0/+LinwoodCreekmore 
summary: This post uses news metadata from GDELT to create a substantive event timeline of the recent crisis in Marawi.  I also introduce my Python client to access and process GDELT data in the Python ecosystem.  This is especially useful if you have business interests in areas with stability issues. If you like time series analysis (you'll see potential), geospatial analysis, data analytics, and data engineering, you will enjoy this post! 
---
## Trouble in Marawi
<br>
<div class="image">
<center><img src="{{ site.url }}/assets/img/marawi-city.jpg" alt="City entranc" ></center>
<div><center><font size=".5"><b>Entrance gate for Marawi in Philippines (aka "Islamic City of Marawi"). <br><i>Philstar.com/John Unson, file</i></b> </font></center></div>
<br>
</div>
<body style="background-color:powderblue;">
<p style="font-family:courier;"><i>It started around 2pm. <br><br>Earlier in the day, villagers near <a href="https://en.wikipedia.org/wiki/Barangay">Barangay</a> Basak Malulut notified security forces of 10-15 men with assault rifles walking in the street. One gunmen matched the description of one of the most-wanted terrorists worldwide; Isnilon Hapilon. According to press reports, Hapilon is the leader of <a href="https://en.wikipedia.org/wiki/Abu_Sayyaf">Abu Sayyaf</a> and the Philippine head of the <a href="https://en.wikipedia.org/wiki/Islamic_state">Islamic State</a>. Motivated by the opportunity to secure such a high value target, Philippine security forces planned a raid on <a href="https://en.wikipedia.org/wiki/Marawi">Marawi City</a>.  They were going after Hapilon.</i></p>
</body>
<br>
<div class="image">
<center><img src="{{ site.url }}/assets/img/gunmen.jpeg" alt="Walking Men" ></center>
<div><center><font size=".5"><b>Armed men walkng down the street in Marawi.</b> </font></center></div>
<br><br>
</div>
<br><br>
<hr>
<br><br>
        
# Code/Tutorial Introduction
This narrative story/data science tutorial provides a short introduction to a python client I built to access and wrangle [Global Database of Events, Language, and Tone (GDELT)](http://www.gdeltproject.org/about.html) data in the Python ecosystem.  And, you'll hopefully get a good story out of this too.  For others considering a tutorial, I hope you take this format up as well. Data scientists and data engineers need to be well versed in the art of writing.  Narratives provide great practice!

### Post Format
We will use a different model for this post. Each narrative section is followed by a coding/description section.  I use a splitter and different font to denote a switch from narrative-to-coding section.  

What you may find interesting, is that I wrote the narrative on the Marawi crisis using news stories chronologically ordered by GDELT (pictures saved from news articles). Even more interesting, is how this Marawi story evloves from a simple raid to catch one guy to a **full blown crisis** that has put a city on alert. We will cover exactly how I accessed the news data.  But first, let's take a minute to understand GDELT.  

<br>
<div class="image">
<center><img src="http://data.gdeltproject.org/dailymaps_noaasos/spinningglobe.gif" alt="GDELT" style="width: 55%" ></center>
<div><center><font size=".5"><b>GDELT monitors world wide news and is a lens into human society.</b> </font></center></div>
</div>
<br>
### What is GDELT?
GDELT is a service that monitors print, broadcast, and web news media in over 100 languages from nearly all countries in the world. Enabled by this constant stream of news data,  GDELT can monitor breaking news events anywhere on the planet.  While it's outside the scope of this introductory post, GDELT data can identify the news providers who have a history of generating original content earliest for breaking issues in certain parts of the world.  Conversely, GDELT data can also find news providers who specialize in redisseminating another provider's content, exposing it to wider audiences at the cost of speed.  And finally, GDELT makes building timelines a simple job; all information about an event is presented chronologically as we will demonstrate in this post.  

### GDELT's Coding System: CAMEO Codes
The CAMEO code is the center of GDELT data analysis.  CAMEO, which stands for [Conflict and Mediation Event Observations](https://en.wikipedia.org/wiki/Conflict_and_Mediation_Event_Observations), is a framework for coding event data to support the study of political and violent events.  Each GDELT record or news story has a CAMEO code that provides a pivot point to filter and categorize news content.  Combining CAMEO code filtering with GDELT's geoinferencing service, it is a simple task to build pipelines that monitor news at the city level on topics across the globe in an automated fashion  (or province/state or country or continent).  The [coding system is pretty extensive](http://data.gdeltproject.org/documentation/CAMEO.Manual.1.1b3.pdf).  For example, to track all news on mass killings in Kigali, Rwanda, we only need to filter on CAMEO code '202' and the [geonames feature identifier for Kigali,  '-2181358' ](http://www.geonames.org/) .  

For all of GDELT's strengths, it would be intellectually dishonest for me to ignore the reported weaknesses.  To see what others have identified as strengths and weaknesses, see [M.D. Ward et al's **Comparing GDELT and ICEWS Event Data**](https://www.researchgate.net/publication/303211430_Comparing_GDELT_and_ICEWS_event_data).  

<br><br>
<hr>
<br><br>
### Explosions and Automatic Gunfire Rock Marawi as Maute Takes on the AFP/PNP 
<br><br>
<p style="font-family:courier;"><i>Between 2 and 3 pm Philippine Time (PHT), explosions and automatic gun fire ripped through the hot, humid air of the Marawi suburb of about 1,700. Reports suggest militants from the <a href="https://en.wikipedia.org/wiki/Maute_group">Maute Group (i.e., Islamic State of Lanao)</a> ambushed a joint force of <a href="https://en.wikipedia.org/wiki/Armed_Forces_of_the_Philippines">Armed Forces of the Philippines (AFP)</a> and <a href="https://en.wikipedia.org/wiki/Philippine_National_Police">Philippine National Police (PNP)</a> personnel just as it arrived to verify the presence of the gunmen. Philippine security forces brought in armoured vehicles, locked down neighborhoods, and used two aircraft to drop bombs on select targets.</i></p>  
<br>

<div class="image">
<center><img src="{{ site.url }}/assets/img/soldiers.jpg" alt="Soldiers" ></center>
<div><center><font size=".5"><b>A soldier watches as a Philippine vehicles drive by on the road</b> </font></center></div>
<br><br>
</div>


<div><p style="font-family:courier;"><i>The sudden outbreak of violence caught citizens and officials off-guard. "There [were] no indications that an attack like this will happen. There are no checkpoints in the city...No news about the city government. Everything is vague," one resident said via text message according to press reports.  Officials did what they could to fill the information gap and urged citizens to "stay in their homes" , "lock their doors", and "be watchful".

<br><br> And that's exactly what the residents did. They watched.</i></p>
</div>
<br><br>
<hr>
<br><br>
## `gdeltPyR`:  A Python Client to Access GDELT data

[`gdeltPyR`](https://github.com/linwoodc3/gdeltPyR) is a simple, user-friendly interface to GDELT. If you know basic command line syntax, Python, and pandas, you will be right at home. To install, [see the installation requirements on the github project page](https://github.com/linwoodc3/gdeltPyR/blob/master/examples/gdeltPyR_basic_use.md#installation). But, install is simple; just hit `pip install gdelt` and you should be good to go!


You may be asking "Linwood, why did you pick Marawi?"  As you can see from the date on the top of this blog,I started writing this in mid-to-late May. Let me stop you right there (if you're thinking it).  No; GDELT can't predict the future.  I basically kept checking the [Wikipedia Current Events page](https://en.wikipedia.org/wiki/Portal:Current_events) until something worthwhile came up.  I started with the [Manchester Arena bombing event](https://en.wikipedia.org/wiki/2017_Manchester_Arena_bombing) (which also works for this demonstration), but wanted to go with something less controversial.  The next day, [Marawi popped up in the Current Events tab and I went with that](https://en.wikipedia.org/wiki/Portal:Current_events/2017_May_23). The major takeaway here is, we need to pull GDELT data for 23 May 2017.  The Manchester data will be in here as well so feel free to experiment.

The first step to any data science project in Python is the import section. Let's get everything we need in this block of code:

```python
##############################
# Standard Library
##############################

import datetime
import re

#############################
# Third-party libraries
#############################

from tzwhere import tzwhere 
import geoplot as gplt
import pandas as pd
import numpy as np
import pytz

#############################
# Modules/functions made by me
#############################

import gdelt
```

GDELT's API pulls data by time.  So, to filter the returned data to Marawi only data, I only need two pieces of information.  The CAMEO code and the geonames feature ID.  In our case, we are looking for all violence (root code 19) and feature id '-2438515'. First, let's set up GDELT:

```python
# tzwhere variable for time normalization
tz1 = tzwhere.tzwhere(forceTZ=True)

# this is the gdeltPyR variable!!!
gd = gdelt.gdelt()
```
<br><br>
<hr>
<br><br>
### Black Flags in Marawi: Residents Capture ISIS links on Social Media Posts
<div>
<br><br>
<div class="image">
<center><img src="{{ site.url }}/assets/img/flag.jpeg" alt="City entranc" ></center>
<div><center><font size=".5"><b>Black flag on vehicle in Marawi</b> </font></center></div>
<center><img src="{{ site.url }}/assets/img/buildings.png" alt="City entranc" ></center>
<div><center><font size=".5"><b>Residents capture militants roaming the streets in Marawi</b> </font></center></div>
<br><br>
</div>
<div><p style="font-family:courier;"><i>Residents of Marawi City took to social media to post photos, videos, and updates on the ongoing clashes. They posted photos of soldiers and their helicopters, Maute members and their black flags, and fires breaking out Tuesday evening. One woman watched as police clashed with 10 armed men who set up positions at the gate of the government-run Amai Pakpak Hospital. Earlier, gunmen had taken over the hospital and raised the black flag representing ISIS.</i></p>


<div class="image">
<center><img src="{{ site.url }}/assets/img/abus.jpeg" alt="Abu Sayyaf" ></center>
<div><center><font size=".5"><b>Abu Sayyaf members.</b> </font></center></div>
</div>
</div></div>

<hr>
<br><br>
## Pulling GDELT Data using `gdeltPyR`
In a few short lines of code, we pull the data (worldwide news) and filter down to urls that are relevant for us.  I also added some custom code to convert times to Philippine local times.  This isn't necessary to get the chronological timeline, but is useful in understanding how fast GDELT is at automatically assigning responsible actors and inferring the location (down to city level). After we filter, it's just a "point, click, read" job.  

So, here's the magic to pull hundreds of thousands of rows of data from GDELT.  Wait for it.  It's complicated. Lots of code:

```python
# code to use gdeltPyR; pulling all of 23 May (coverage = True) 
# and normalizing column names to SQL friendly format (normcols=True)

marawi = gd.Search(['2017 May 23'], coverage=True,normcols=True)
```

That's it!  Should take a 10-20 seconds to pull down a full day depending on your machine (my stats: 19 seconds for 211,911 by 62 dataframe that is 458.9 MB).  GDELT also ships data out in 15 minute intervals; if you set `coverage=False`, that is the 15 minute pull and takes 1-2 seconds and returns 2000-3000 records (stats: 4 seconds for 2198 by 62 dataframe that is 4.8 MB).  


Next we use a custom function to normalize the time (set it to local Philippine time based on the lat/lon).  We need to create our function first:

```python
def striptimen(x):
    """Strip time from numpy array or list of dates that are integers"""
    date = str(int(x))
    n = np.datetime64("{}-{}-{}T{}:{}:{}".format(date[:4],date[4:6],date[6:8],date[8:10],date[10:12],date[12:]))
    return n

def timeget(x):
    '''convert to datetime object with UTC time tag'''
    
    try:
        now_aware = pytz.utc.localize(x[2].to_pydatetime())
    except:
        pass
    
    # get the timezone string representation using lat/lon pair
    try:
        timezone_str=tz1.tzNameAt(x[0],x[1],forceTZ=True)
        
            # get the time offset
        timezone = pytz.timezone(timezone_str)

        # convert UTC to calculated local time
        aware = now_aware.astimezone(timezone)
        return aware
    
    except Exception as e:
        pass

# vectorize our two functions
vect = np.vectorize(striptimen)
vect2=np.vectorize(timeget)

# time-enabling our entire dataset using the function above
dates = vect(marawi.dateadded.values)
marawi = marawi.assign(dates=dates)
marawi.set_index(dates,inplace=True)


# making a timezone aware column
datetz = [timeget(l) for l in marawi[['actiongeolat','actiongeolong','dates']]\
[marawi[['actiongeolat','actiongeolong','dates']].notnull()==True].values.tolist()]

marawi=marawi.assign(datezone=datetz)
```

Now we are ready to filter down to the relevant data.  Again, we only need the feature id for Marawi ('-2438515') and the CAMEO code of interest (event root code '19' of the 2.19 FIGHT section).  To get help on the headers, take a [look at this file](https://github.com/linwoodc3/gdelt2HeaderRows/blob/master/schema_csvs/GDELT_2.0_Events_Column_Labels_Header_Row_Sep2016.csv).

```python
# filter to data in Marawi city and about violence/fighting only
maute2= marawi[(marawi.actiongeofeatureid=='-2438515') & (marawi.eventrootcode=='19')]
print(maute2.shape)
---------------------
Out: (206, 65)

```

Now lets filter the data.  It doesn't take much when you have the power of Python pandas!  

```python
# filtering to 3 columns

muate2.sort_values('datezone')[['datezone','dateadded','sourceurl']].drop_duplicates('sourceurl').head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>datezone</th>
      <th>dates</th>
      <th>sourceurl</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2017-05-23 09:30:00</th>
      <td>2017-05-23 17:30:00+08:00</td>
      <td>2017-05-23 09:30:00</td>
      <td>http://www.philstar.com:8080/nation/2017/05/23...</td>
    </tr>
    <tr>
      <th>2017-05-23 11:00:00</th>
      <td>2017-05-23 19:00:00+08:00</td>
      <td>2017-05-23 11:00:00</td>
      <td>http://hosted2.ap.org/CAANR/0260ea4c3e85456b80...</td>
    </tr>
    <tr>
      <th>2017-05-23 11:15:00</th>
      <td>2017-05-23 19:15:00+08:00</td>
      <td>2017-05-23 11:15:00</td>
      <td>http://www.sfgate.com/news/world/article/Phili...</td>
    </tr>
    <tr>
      <th>2017-05-23 11:30:00</th>
      <td>2017-05-23 19:30:00+08:00</td>
      <td>2017-05-23 11:30:00</td>
      <td>http://www.philstar.com/nation/2017/05/23/1702...</td>
    </tr>
    <tr>
      <th>2017-05-23 11:45:00</th>
      <td>2017-05-23 19:45:00+08:00</td>
      <td>2017-05-23 11:45:00</td>
      <td>http://www.channelnewsasia.com/news/asiapacifi...</td>
    </tr>
  </tbody>
</table>
<br><br>
<hr>
<br><br>
### President Duterte Declares Martial Law
<br><br>
<div><p style="font-family:courier;"><i>
By 1045 pm PHT, Preseident Rodrigo Duterte declared 60 days of martial law in Marawi.  Ten hours after the start of the clash, two soldiers from the 103rd Brigade and one policeman were killed, at least other 12 government personnel were injured, and the city was locked down for at least 2 months. </i></p> 

<div class="image">
<center><img src="{{ site.url }}/assets/img/Duterte.jpeg" alt="Duterte" ></center>
<div><center><font size=".5"><b>Duterte declares martial law while on official travel in Russia.</b> </font></center></div>
<br>
</div>


<div><p style="font-family:courier;"><i>The city had suffered its share of damage as well.  The Maute Group had burned a church, the city jail, the Ninoy Aquino School and the Dansalan College. Moreover,  Maute fighters had occupied the main street of Marawi city, Amai Pakpak Medical Center, City Hall, the city jail, and two bridges.</i></p>  
<div class="image">
<center><img src="{{ site.url }}/assets/img/streetz.jpeg" alt="Twitter pic of residents" ></center>
<div><center><font size=".5"><b>Residents stand on the streets of Marawi as the glow of smoldering buildings paints the horizon.</b> </font></center></div>
<br>
</div>
<div><p style="font-family:courier;"><i>Somehow, a raid on 15 gunmen turned into a city-wide conflict with an estimated 100 gunmen divided into groups of 10 in different locations.  This was only the start of a much longer running conflict.<br><br>This was only the first 10-12 hours...</i></p>
<br><br>
</div>
</div>
</div>
<br><br>
<hr>
<br><br>
## Mission Accomplished: Use `gdeltPyR` and GDELT

That closes out our brief introduction to gdeltPyR and our story. To summarize, we pulled a chronologically ordered list of URLs to news stories on the Marawi crisis.  After we pulled the data from GDELT, we used the geonames featureid and the CAMEO code to filter down to the relevant data; this can be repeated for any issue or location.  To build the final timeline, you only need to read the stories and write coherently to make your story.      


For `gdeltPyR` (which I hope you will use or help me develop!), I focused on abstracting the details of the API calls away from the user. And, I added a little multiprocessing juice in there as well to make full use of all available cores! For those who run into feature ID problems, there's an easy way to build your own lookup table of feature ids.  Just pull a few days of GDELT data using gdeltPyR, isolate unique feature ids, and create a second column that holds the different names tied to the same feature id.  Pretty soon, you'll have the id and name for most cities, provinces/states, and countries.

Now, go forth and use [`gdeltPyR`](https://github.com/linwoodc3/gdeltPyR) or, learn more about [GDELT](http://www.gdeltproject.org/) in general. 

I need to give you a warning on data consumption; GDELT is MASSIVE.  I will connect gdeltPyR directly into the free Google BigQuery into GDELT data, but here is a fair warning. If you do a query with gdeltPyR covering more than a few months, you'd need to have more than 16 GBs of RAM or you can create a pipeline that reads the data in, writes to disc, and flushes RAM before going to the next day.  

Now, the fun part for you.  We could have done MUCH MORE with this data.  Some ideas include:
- Implement web scraping on each data source to extract the content; then compute element wise Levenshtein distance (LD) or semantic similarity; result - determine which documents have most original content
- Extend analytic above by adding news provider and time component to find news providers who provide most original content faster
- Do everything above, but try to use the resulting corpus for text summarization
- New organizations could use this data to ensure they are adding original content when they publish!
- Can you think of more?!?!? 

### The Obigatory Graphic:  See the Time Series Potential of GDELT
The Walkoff:  Every post needs a graphic.  Below, you'll see a simple time series graphic that charts the magnitude of our target CAMEO code and feature id (normalized by total hour count) through time. We see an obvious uptick in activity later in the day, led by a complete lack of activity. 0900 UTC is about 1700 PHT; we were getting automatic extractions within a few hours of some event happening in some remote village in the Phillipines, we are getting automatic data! 

I Welcome your thoughts/comments/corrections. My basicaly thought is, since we have a geoname entry for nearly every city on the planet, we can run this analytic ad finitum on each CAMEO code and each feature id; the only limitations are compute power and storage. Now, the graphic (using my Economist matplotlib style):

```python
import matplotlib.pyplot as plt
import matplotlib as mpl


mpl.style.use('economist')
timeseries= pd.concat([marawi.resample('H')['sourceurl'].count(),muate2.resample('H')['sourceurl'].count()]
         ,axis=1)
timeseries.fillna(0,inplace=True)
timeseries.columns = ['Total Events','Marawi Violent Events Only']
timeseries = timeseries.assign(Normalized=(timeseries['Marawi Violent Events Only']/timeseries['Total Events'])*100)
f,ax = plt.subplots(figsize=(15,9))
ax = timeseries.Normalized.ewm(adjust=True,ignore_na=True,min_periods=5,span=12).mean().plot(label='Explonetially Weight Count')
ax.set_title('Count of Violent Events in Marawi\nNormalized Against Total GDELT Events',fontsize=28)
for label in ax.get_xticklabels():
      label.set_fontsize(22)

ax.legend()
plt.show()

```

<div class="image">
<center><img src="{{ site.url }}/assets/img/countGraphic.png" alt="Count" ></center>
<div><center><font size=".5"><b>Simple analytic of normalizing count of event type in geographic regioin</b> </font></center></div>
<br>
</div>



[^1]:  ["MAPPING THE FUTURE OF MANKIND USING HISTORICAL DATASETS"](http://dataconomy.com/2014/09/mapping-the-future-of-mankind-using-historical-datasets/)

[^2]: [Global Database of Events, Language, and Tone (GDELT)](http://www.gdeltproject.org/)

