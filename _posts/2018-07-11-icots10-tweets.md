---
layout: post
title: "Battle of the conference hashtags"
tags:
  - event-notes
  - R-code
  - social-media
---

This Tweet about conference hashtags got me thinking, and I've wanted to use the `twitteR` package for a while...

<blockquote class="twitter-tweet" data-lang="en">
<p lang="en" dir="ltr">
How does <a href="https://twitter.com/hashtag/ICOTS10?src=hash&amp;ref_src=twsrc%5Etfw">\#ICOTS10</a> measure up? <a href="https://t.co/NUC3k2XAQK">https://t.co/NUC3k2XAQK</a>
</p>
— Kelly Bodwin (@KellyBodwin) <a href="https://twitter.com/KellyBodwin/status/1016743797550485505?ref_src=twsrc%5Etfw">July 10, 2018</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

*Last updated*: 11 Jul approx 1:10pm Kyoto time.

Here are some libraries we'll use. (Also note that I made a [GitHub gist with the code to get from the beginning to the first graph](https://gist.github.com/elb0/221b98cd7f89674515f2a25a6cde5859).)

{% highlight r %}
library(twitteR)
library(ggplot2)
library(dplyr)
library(lubridate)
library(stringr)
{% endhighlight %}

Just one thing to be careful of, you'll need to get your own twitter keys. [This might be quite helpful](https://medium.com/@GalarnykMichael/accessing-data-from-twitter-api-using-r-part1-b387a1c7d3e). I had to reset my keys to get the code to work. Not sure why, but [others have had the same issue](https://github.com/geoffjentry/twitteR/issues/74).

`# setup_twitter_oauth(consumer_key, consumer_secret, access_token, access_secret)`

The Tweet that inspired this shared a Tweet that compared useR (a conference about using the R statistical programming language in Brisbane, Australia in 2018), with ICLM (a conference about machine learning in Stockholm, Sweden in 2018). The below chunk gets the data for these two conferences and for the 10th International Conference on Teaching Statistics in Kyoto, Japan.

Also note, the `n =` were chosen with trial and error. Very sophisticated.

{% highlight r %}
icots = searchTwitter('#icots10', n = 1000)
user = searchTwitter('#useR2018', n = 1000)
iclm = searchTwitter('#ICML2018', n = 2000)

icots_tweets = twListToDF(icots) %>%
  mutate(Hashtag = "#ICOTS10")
user_tweets = twListToDF(user) %>%
  mutate(Hashtag = "#useR2018")
iclm_tweets = twListToDF(iclm) %>%
  mutate(Hashtag = "#ICLM2018")
{% endhighlight %}

There also seemed to be lots of Tweets under the ICOTS hashtag that were about Ethereum, which wasn't really related to us. I've filtered these out.

And you might want to look at the dates in a different timezone setting - these conferences are all in different countries.

{% highlight r %}
all_tweets = rbind(icots_tweets, user_tweets, iclm_tweets) %>%
  mutate(date = format(created, format="%Y-%m-%d")) %>%
  filter(!str_detect(text, "@EthereumLimited:")) %>%
  filter(!str_detect(text, "Ethlimited"))
{% endhighlight %}

Let's take a look!

{% highlight r %}
all_tweets %>%
  group_by(Hashtag, date) %>%
  summarise(Count = n()) %>%
  mutate(Date = as.Date(date)) %>%
  ggplot(aes(Date, Count, colour = Hashtag)) +
  geom_line() +
  scale_x_date(date_minor_breaks = "1 day") +
  ggtitle("Conference Hashtag Showdown")
{% endhighlight %}

![]({{ site.baseurl }}/images/conferencetweets_files/figure-markdown_github/graph-1.png)




### How about number of retweets by conference?

{% highlight r %}
all_tweets %>%
  group_by(Hashtag) %>%
  summarise(Retweets = sum(retweetCount)) %>%
  ggplot(aes(Hashtag, Retweets, fill = Hashtag)) +
  geom_bar(stat="identity") +
  ggtitle("Retweets by conference hashtag")
{% endhighlight %}

![]({{ site.baseurl }}/images/conferencetweets_files/figure-markdown_github/biggest_retweets-1.png)

### Number of favourites?

{% highlight r %}
all_tweets %>%
  group_by(Hashtag) %>%
  summarise(Favourites = sum(favoriteCount)) %>%
  ggplot(aes(Hashtag, Favourites, fill = Hashtag)) +
  geom_bar(stat="identity") +
  ggtitle("Favourites by hashtag")
{% endhighlight %}

![]({{ site.baseurl }}/images/conferencetweets_files/figure-markdown_github/biggest_favourites-1.png)

### Which Tweets saw the most retweets?

{% highlight r %}
toptweet = all_tweets %>%
  group_by(Hashtag) %>%
  filter(retweetCount == max(retweetCount)) %>%
  distinct(Hashtag, .keep_all = TRUE)

topretweeticots = toptweet %>%
  filter(Hashtag == "#ICOTS10")

topretweeticml = toptweet %>%
  filter(Hashtag == "#ICLM2018")

topretweetuser = toptweet %>%
  filter(Hashtag == "#useR2018")
{% endhighlight %}

Check out the most retweeted [ICOTS Tweet]('paste0(%22https://twitter.com/anyuser/status/%22,%20topretweeticots$id)%60), [ICLM Tweet]('paste0(%22https://twitter.com/anyuser/status/%22,%20topretweeticml$id)%60) and [useR Tweet]('paste0(%22https://twitter.com/anyuser/status/%22,%20topretweetuser$id)%60).

### The most favourites?

{% highlight r %}
topfav = all_tweets %>%
  group_by(Hashtag) %>%
  filter(favoriteCount == max(favoriteCount)) %>%
  distinct(Hashtag, .keep_all = TRUE)

topfavicots = topfav %>%
  filter(Hashtag == "#ICOTS10")

topfavicml = topfav %>%
  filter(Hashtag == "#ICLM2018")

topfavuser = topfav %>%
  filter(Hashtag == "#useR2018")
{% endhighlight %}

Check out the most favourited [ICOTS Tweet]('paste0(%22https://twitter.com/anyuser/status/%22,%20topfavicots$id)%60), [ICLM Tweet]('paste0(%22https://twitter.com/anyuser/status/%22,%20topfavicml$id)%60) and [useR Tweet]('paste0(%22https://twitter.com/anyuser/status/%22,%20topfavuser$id)%60).

More from ICOTS
---------------

Most of my tweets from this conference are in the below thread and I also set up a [list of resources](http://blog.dataembassy.co.nz/icots10/) to check out after the conference based on people's recommendations.

<blockquote class="twitter-tweet" data-lang="en">
<p lang="en" dir="ltr">
Delighted to be at <a href="https://twitter.com/hashtag/icots10?src=hash&amp;ref_src=twsrc%5Etfw">\#icots10</a> with an awesome crew from <a href="https://twitter.com/statsauckuni?ref_src=twsrc%5Etfw">@statsauckuni</a>, and almost 500 other delegates from 59 countries. Ft. <a href="https://twitter.com/annafergussonnz?ref_src=twsrc%5Etfw">@annafergussonnz</a> <a href="https://twitter.com/rhysauck?ref_src=twsrc%5Etfw">@rhysauck</a> <a href="https://twitter.com/Emmawonwon?ref_src=twsrc%5Etfw">@Emmawonwon</a> <a href="https://t.co/nnKnezTfsu">pic.twitter.com/nnKnezTfsu</a>
</p>
— Liza Bolton (@Liza\_Bolton) <a href="https://twitter.com/Liza_Bolton/status/1015879520576942080?ref_src=twsrc%5Etfw">July 8, 2018</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
