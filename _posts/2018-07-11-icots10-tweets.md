---
layout: post
title: "Battle of the conference hashtags"
tags:
  - event-notes
  - R-code
  - social-media
---

A tweet comparing ICML and useR got me thinking, and I've wanted to use the `twitteR` or `rtweet` package for a while...

<blockquote class="twitter-tweet" data-lang="en">
<p lang="en" dir="ltr">
How does <a href="https://twitter.com/hashtag/ICOTS10?src=hash&amp;ref_src=twsrc%5Etfw">\#ICOTS10</a> measure up? <a href="https://t.co/NUC3k2XAQK">https://t.co/NUC3k2XAQK</a>
</p>
— Kelly Bodwin (@KellyBodwin) <a href="https://twitter.com/KellyBodwin/status/1016743797550485505?ref_src=twsrc%5Etfw">July 10, 2018</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

-   [Number of tweets by conference](#number-of-tweets-by-conference)
-   [How about number of retweets by conference?](#how-about-number-of-retweets-by-conference)
-   [Number of favourites?](#number-of-favourites)
-   [What about percentage of tweets getting favourited or retweeted?](#what-about-percentage-of-tweets-getting-favourited-or-retweeted)
-   [Which tweets saw the most retweets?](#which-tweets-saw-the-most-retweets)
-   [The most favourites?](#the-most-favourites)


*Last updated:* 12 Jul approx 11:15pm Kyoto time. Took out emoji stuff that was here previously, for now, because I need to fix some things! 🤔

Here are some libraries we'll use. (Also note that I made a [GitHub gist with the code to get from the beginning to the first graph](https://gist.github.com/elb0/221b98cd7f89674515f2a25a6cde5859).) I couldn't get the authentication to work for `rtweet`, unfortunately.

{% highlight r %}
library(twitteR)
library(ggplot2)
library(dplyr)
library(lubridate)
library(stringr)
library(tidyr)
library(RColorBrewer)
library(robotstxt)
library(rvest)
library(tibble)
library(data.table)
library(knitr)
{% endhighlight %}

Just one thing to be careful of, you'll need to get your own twitter keys. [This might be quite helpful](https://medium.com/@GalarnykMichael/accessing-data-from-twitter-api-using-r-part1-b387a1c7d3e). I had to reset my keys to get the code to work. Not sure why, but [others have had the same issue](https://github.com/geoffjentry/twitteR/issues/74).

{% highlight r %}
setup_twitter_oauth(consumer_key, consumer_secret, access_token, access_secret)
{% endhighlight %}

    ## [1] "Using direct authentication"

The tweet that inspired this shared a tweet that compared useR (a conference about using the R statistical programming language in Brisbane, Australia in 2018), with ICML (a conference about machine learning in Stockholm, Sweden in 2018). The below chunk gets the data for these two conferences and for the 10th International Conference on Teaching Statistics in Kyoto, Japan.

Also note, the `n =` were chosen by trial and error. Very sophisticated.

{% highlight r %}
icots = searchTwitter('#icots10', n = 1100)
user = searchTwitter('#useR2018', n = 3200)
icml = searchTwitter('#ICML2018', n = 4000)

icots_tweets = twListToDF(icots) %>%
  mutate(Hashtag = "#ICOTS10")
user_tweets = twListToDF(user) %>%
  mutate(Hashtag = "#useR2018")
icml_tweets = twListToDF(icml) %>%
  mutate(Hashtag = "#ICML2018")

### We might also want to strip out all the retweets and only work with that data. I have not opted to remove modified tweets as it seemed to get rid of things that I thought should stay.

icots_noRT = strip_retweets(icots, strip_mt=FALSE) %>%
  twListToDF() %>%
  mutate(Hashtag = "#ICOTS10")
user_noRT = strip_retweets(user, , strip_mt=FALSE) %>%
  twListToDF() %>%
  mutate(Hashtag = "#useR2018")
icml_noRT = strip_retweets(icml, , strip_mt=FALSE) %>%
  twListToDF() %>%
  mutate(Hashtag = "#ICML2018")
{% endhighlight %}

There also seemed to be lots of tweets under the ICOTS hashtag that were about Ethereum, which wasn't really related to us. I've filtered these out.

And you might want to look at the dates in a different timezone setting - these conferences are all in different countries.

{% highlight r %}
all_tweets = rbind(icots_tweets, user_tweets, icml_tweets) %>%
  mutate(date = format(created, format="%Y-%m-%d")) %>%
  filter(!str_detect(text, "@EthereumLimited:")) %>%
  filter(!str_detect(text, "Ethlimited"))

all_noRT = rbind(icots_noRT, user_noRT, icml_noRT) %>%
  mutate(date = format(created, format="%Y-%m-%d")) %>%
  filter(!str_detect(text, "@EthereumLimited:")) %>%
  filter(!str_detect(text, "Ethlimited"))
{% endhighlight %}

Let's take a look!

### Number of tweets by conference

{% highlight r %}
all_tweets %>%
  group_by(Hashtag, date) %>%
  summarise(Count = n()) %>%
  mutate(Date = as.Date(date)) %>%
  ggplot(aes(Date, Count, colour = Hashtag)) +
  geom_line() +
  scale_x_date(date_minor_breaks = "1 day") +
  ggtitle("Battle of the conference hashtags")
{% endhighlight %}

![]({{site.baseurl}}/images/conferencetweets_files/figure-markdown_github/graph-1.png)

{% highlight r %}
all_noRT %>%
  group_by(Hashtag, date) %>%
  summarise(Count = n()) %>%
  mutate(Date = as.Date(date)) %>%
  ggplot(aes(Date, Count, colour = Hashtag)) +
  geom_line() +
  scale_x_date(date_minor_breaks = "1 day") +
  ggtitle("Battle of the conference hashtags - retweets removed")
{% endhighlight %}

![]({{site.baseurl}}/images/conferencetweets_files/figure-markdown_github/graph-2.png)

### How about number of retweets by conference?

{% highlight r %}
all_tweets %>%
  group_by(Hashtag) %>%
  summarise(Retweets = sum(retweetCount)) %>%
  ggplot(aes(Hashtag, Retweets, fill = Hashtag)) +
  geom_bar(stat="identity") +
  ggtitle("Retweets by conference hashtag")
{% endhighlight %}

![]({{site.baseurl}}/images/conferencetweets_files/figure-markdown_github/biggest_retweets-1.png)

### Number of favourites?

{% highlight r %}
all_tweets %>%
  group_by(Hashtag) %>%
  summarise(Favourites = sum(favoriteCount)) %>%
  ggplot(aes(Hashtag, Favourites, fill = Hashtag)) +
  geom_bar(stat="identity") +
  ggtitle("Favourites by hashtag")
{% endhighlight %}

![]({{site.baseurl}}/images/conferencetweets_files/figure-markdown_github/biggest_favourites-1.png)

### What about percentage of tweets getting favourited or retweeted?

I was curious whether support/quality within the hashtags was similar. I think proportion of tweets getting retweeted or favourited might be an interesting measure of the "interestingness" of the tweets, but more likely an indicator of how supportive the group on that hashtag are.

{% highlight r %}
all_noRT %>%
  group_by(Hashtag) %>%
  filter(isRetweet == FALSE) %>%
  summarise(Favourited = sum(favoriteCount > 0), Retweeted = sum(retweetCount > 0), total = n()) %>%
  mutate(`Percentage Favourited`= Favourited/total, `Percentage Retweeted` = Retweeted/total) %>%
  select(Hashtag, `Percentage Favourited`, `Percentage Retweeted`) %>%
  gather(Measure, Percentage, -Hashtag) %>%
  ggplot(aes(Hashtag, Percentage, fill = Measure)) +
  geom_bar(stat="identity", width = 0.5, position = "dodge") +
  ggtitle("Percentage of Original Tweets Favourited or Retweeted by Conference Hashtag") +
  scale_fill_brewer(palette="Accent") +
  scale_y_continuous(labels = scales::percent)
{% endhighlight %}

![]({{site.baseurl}}/images/conferencetweets_files/figure-markdown_github/percentages-1.png)

The retweets are strong with \#ICOTS10!

### Which tweets saw the most retweets?

{% highlight r %}
toptweet = all_tweets %>%
  group_by(Hashtag) %>%
  filter(retweetCount == max(retweetCount)) %>%
  distinct(Hashtag, .keep_all = TRUE)

topretweeticots = toptweet %>%
  filter(Hashtag == "#ICOTS10")

topretweeticml = toptweet %>%
  filter(Hashtag == "#ICML2018")

topretweetuser = toptweet %>%
  filter(Hashtag == "#useR2018")
{% endhighlight %}

Check out the most retweeted [ICOTS tweet](https://twitter.com/anyuser/status/1017403257851822080), [ICML tweet](https://twitter.com/anyuser/status/1017411760339341312) and [useR tweet](https://twitter.com/anyuser/status/1017410998725050368).

### The most favourites?

{% highlight r %}
topfav = all_tweets %>%
  group_by(Hashtag) %>%
  filter(favoriteCount == max(favoriteCount)) %>%
  distinct(Hashtag, .keep_all = TRUE)

topfavicots = topfav %>%
  filter(Hashtag == "#ICOTS10")

topfavicml = topfav %>%
  filter(Hashtag == "#ICML2018")

topfavuser = topfav %>%
  filter(Hashtag == "#useR2018")
{% endhighlight %}

Check out the most favourited [ICOTS tweet](https://twitter.com/anyuser/status/1017244182035849216), [ICML tweet](https://twitter.com/anyuser/status/1017059295052140544) and [useR tweet](https://twitter.com/anyuser/status/1014967711036829696).


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
Vanity time: My best tweets?
----------------------------

I kinda want to know which of my tweets did the best too...

{% highlight r %}
mytweets = all_tweets %>%
  filter(screenName == "Liza_Bolton")

myprop = dim(mytweets)[1]/dim(icots_tweets)[1]

myfavtweets = mytweets %>%
  arrange(desc(retweetCount)) %>%
  select(text, retweetCount, favoriteCount) %>%
  filter(retweetCount == max(retweetCount)) %>%
  filter(favoriteCount == max(favoriteCount))
{% endhighlight %}

[Turns out this one was pretty good](https://twitter.com/anyuser/status/1016130460697522177), with 8 retweets and 31 favourites. Thanks guys!

I tweeted 6% of the \#ICOTS10 tweets.

<script type="application/json" data-for="htmlwidget-a5007af146bcf44139ac">{"x":{"url":"conferencetweets_files/figure-markdown_github//widgets/widget_icml_emoji.html","options":{"xdomain":"\*","allowfullscreen":false,"lazyload":false}},"evals":[],"jsHooks":[]}</script>
