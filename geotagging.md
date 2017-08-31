## GeoTagging

As the amount of data that is obtainable keeps growing, the necessity to be able to extract nuggets of useful information becomes ever more important. Researchers have been looking into the extraction of data from text for decades and useful techniques such as sentiment analysis, entity extraction and document summarizing have evolved in recent years to be almost as accurate as humans (sometimes, even better!). Saying this, there's still nuggets of information that seems to have escaped researchers keen eyes and Geotagging seems to be one of these. Geotagging is the process of adding geospatial metadata (usually lat/longs) to media such as photos, videos or websites to name a few. Geotagging photos is old news now but what about online data such as news articles or social media... How much can we gain from knowing where an article/social media posting is published from or even where it's talking about?

Let's have a look at an example to answer these questions before I delve into the ins and outs of geotagging. 

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Believe it or not, this is a shark on the freeway in Houston, Texas. <a href="https://twitter.com/hashtag/HurricaneHarvy?src=hash">#HurricaneHarvy</a> <a href="https://t.co/ANkEiEQ3Y6">pic.twitter.com/ANkEiEQ3Y6</a></p>&mdash; Jason Michael 🦈 (@Jeggit) <a href="https://twitter.com/Jeggit/status/902048241646280704">August 28, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

So if anyone didn't see this, someone based in Dublin tweeted a remarkably believable photo of a shark swimming on a freeway in Texas in the aftermath of Hurricane Harvey. Safe to say, anyone that actually noticed where @Jeggit was based, they could probably safely assume that this was fake news. This is where geotagging comes into play, could we use the fact that @Jeggit posted this tweet from somewhere so far away from where he was actually talking about an indicator of fake news? Personally, I think so and so do quite a few others:  LINK??

So now with an understanding of why we want this information, you might be asking..how hard is it? For news articles and more structured text - not that difficult, there are geo-coding websites such as: https://mynasadata.larc.nasa.gov/latitudelongitude-finder/ where you can find the lat/longs of any address in the world. However, this is with the assumption that you have nice, clean data and as we all know, data is messy more often than not. Here's some examples of twitter locations that people decided to tag their tweets with:

* orange beach, al
* bristol
* ali's house

Now i know that some of these may strike you as odd examples of messy data, 'orange beach, al' can be corrected quite nicely to 'Orange Beach, Alabama' as long as you know that the state code of Alabama is al and al is not referring to Albania whose country code is also 'al'. And what about Bristol? Did you know that there are 23 places in the world which are named Bristol (if you don't believe me...https://en.wikipedia.org/wiki/Bristol_(disambiguation)). How would you know if an article is talking about Bristol, UK or about Bristol, Virginia? Obviously that last location which I've included in that list are examples of places you will never be able to resolve from Geotagging unless you gain more information about 'ali' and where she lives..

There are a few solutions to Geotagging out there already such as [1][2] which explore the problem of disambiguating locations and not locations. [1] also looks at disambiguation of multiple geo places and builds off the assumption that the distance between where someone has posted from and where they are posting about will be a relatively short distance. I took this assumption and expanded it to assume that text with multiple location entities would be talking about places within short distances rather than large distances. This obviously does not work when talking about countries and therefore I investigated a series of edge cases like this and eventually landed on what I think of as a good solution (even if simple!).

I tackled both source geotagging and entity geotagging which finds the lat/longs of where a document is published from and where the document is talking about respectively. These are two quite separate problems and so I will talk briefly about both. Source geotagging is ultimately the easier type as the main problem is with social media and 'correcting' tags such as the ones I mentioned earlier. To correct these locations, I came up with a 4 step process:

*Splits the location by delimiters such as commas,full stops, hyphons.
* Check each of the split locations and check if either is a country or state code and convert it to the corresponding country/state name.
* Ask a geocoder for information about the location which you get back something like:
```json
{
   "properties": {
   "osm_key": "place",
   "extent": [
   -73.3529631,
   15.9158431,
   -59.5427078,
   0.647529
   ],
   "name": "Venezuela",
   "osm_id": 272644,
   "osm_type": "R",
   "osm_value": "country",
   "country": "Venezuela"
   },
   "type": "Feature",
   "geometry": {
   "type": "Point",
   "coordinates": [
   -66.1109317,
   8.0018709
   ]
   }
}
```


[1] =  Paradesi, Sharon Myrtle. "Geotagging Tweets Using Their Content." FLAIRS conference. 2011.
[2] =  Sultanik, Evan A., and Clayton Fink. "Rapid geotagging and disambiguation of social media text via an indexed gazetteer." Proceedings of ISCRAM 12 (2012): 1-10.
