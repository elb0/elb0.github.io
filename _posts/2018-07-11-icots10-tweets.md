---
layout: post
title: "Battle of the conference hashtags"
tags:
  - event-notes
  - R-code
  - social-media
---

This tweet got me thinking, and I've wanted to use the `twitteR` or `rtweet` package for a while...

<blockquote class="twitter-tweet" data-lang="en">
<p lang="en" dir="ltr">
How does <a href="https://twitter.com/hashtag/ICOTS10?src=hash&amp;ref_src=twsrc%5Etfw">\#ICOTS10</a> measure up? <a href="https://t.co/NUC3k2XAQK">https://t.co/NUC3k2XAQK</a>
</p>
— Kelly Bodwin (@KellyBodwin) <a href="https://twitter.com/KellyBodwin/status/1016743797550485505?ref_src=twsrc%5Etfw">July 10, 2018</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
*Last updated:* 12 Jul approx 12:32am Kyoto time.

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
{% endhighlight %}

Just one thing to be careful of, you'll need to get your own twitter keys. [This might be quite helpful](https://medium.com/@GalarnykMichael/accessing-data-from-twitter-api-using-r-part1-b387a1c7d3e). I had to reset my keys to get the code to work. Not sure why, but [others have had the same issue](https://github.com/geoffjentry/twitteR/issues/74).

`# setup_twitter_oauth(consumer_key, consumer_secret, access_token, access_secret)`

    ## [1] "Using direct authentication"

The tweet that inspired this shared a tweet that compared useR (a conference about using the R statistical programming language in Brisbane, Australia in 2018), with ICML (a conference about machine learning in Stockholm, Sweden in 2018). The below chunk gets the data for these two conferences and for the 10th International Conference on Teaching Statistics in Kyoto, Japan.

Also note, the `n =` were chosen by trial and error. Very sophisticated.

{% highlight r %}
icots = searchTwitter('#icots10', n = 1000)
user = searchTwitter('#useR2018', n = 2000)
icml = searchTwitter('#ICML2018', n = 2500)

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
{% endhighlight %}

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

{% highlight r %}
all_noRT = rbind(icots_noRT, user_noRT, icml_noRT) %>%
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
  ggtitle("Battle of the conference hashtags")
{% endhighlight %}

![]({{ site.baseurl }}/images/conferencetweets_files/figure-markdown_github/graph-1.png)

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

![]({{ site.baseurl }}/images/conferencetweets_files/figure-markdown_github/graph-2.png)

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

![]({{ site.baseurl }}/images/conferencetweets_files/figure-markdown_github/percentages-1.png)

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

Check out the most retweeted [ICOTS tweet](https://twitter.com/anyuser/status/1017056191384436736), [ICML tweet](https://twitter.com/anyuser/status/1017046233104384002) and [useR tweet](https://twitter.com/anyuser/status/1017068515092811777).

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

Check out the most favourited [ICOTS tweet](https://twitter.com/anyuser/status/1016569243947724800), [ICML tweet](https://twitter.com/anyuser/status/1016548701248974850) and [useR tweet](https://twitter.com/anyuser/status/1014967711036829696).

### Emojis ❤️

What better place to explore emojis from that the country that gave us the language that gave us the word! Ya follow? Emoji is a Japanese word that, if a quick Google search is to be believed, means "picture character".

Anna Fergusson, \[@annafergussonnz\](<https://twitter.com/annafergussonnz>), and I, in our talk on [modern data in a large introductory statistics course](https://annafergusson.github.io/paristergram/), tried to quickly show how emoji analysis can be a bit of fun. So let's keep that up here.

This first section is mostly jsut copied from the work Anna and I did that will be up on the \[Parisgram\]((<https://annafergusson.github.io/paristergram/>) at somepoint. The emoji handling is not very elegant right now.

There is a table provided by the Unicode organisation that has the emoji, and an additional one that details skintone variations. These also provide a text description and groups the emoji into wider groups, like flags, buildings, types of facial expression etc. It would be ideal to have that information in R to work with directly, so I want to scrape the tables into R. I can check if this seems to be allowed by using the `robotstxt` package. There are a range of other ways to get databases like this, this just seemed easiest for this case.

### Checking if we can scrape the emoji lists

{% highlight r %}
# These are the url with the  list of emoji codes from Unicode
url_base = "https://unicode.org/emoji/charts/full-emoji-list.html"
url_skin = "https://unicode.org/emoji/charts/full-emoji-modifiers.html"

# We can check out the robots.txt file right from R
robotstxt::get_robotstxt("unicode.org")
{% endhighlight %}

    ## User-agent: *
    ## Disallow: /draft/    # not official even if visible
    ## Disallow: /Public/1.1-Update/    # obsolete data
    ## Disallow: /Public/2.1-Update2/   # obsolete data
    ## Disallow: /Public/3.0-Update/    # obsolete data
    ## Disallow: /Public/2.0-Update/    # obsolete data
    ## Disallow: /Public/2.1-Update3/   # obsolete data
    ## Disallow: /Public/2.1-Update/    # obsolete data
    ## Disallow: /Public/2.1-Update4/   # obsolete data
    ## Disallow: /Public/3.0-Update1/ # obsolete data
    ## Disallow: /Public/3.1-Update/  # obsolete data
    ## Disallow: /Public/3.1-Update1/ # obsolete data
    ## Disallow: /Public/3.2-Update/ # obsolete data
    ## Disallow: /Public/4.0-Update/ # obsolete data
    ## Disallow: /Public/4.0-Update1/ # obsolete data
    ## Disallow: /Public/4.1.0/ # obsolete data
    ## Disallow: /Public/5.0.0/ # obsolete data
    ## Disallow: /Public/5.1.0/ # obsolete data
    ## Disallow: /Public/5.2.0/ # obsolete data
    ## Disallow: /Public/6.0.0/ # obsolete data
    ## Disallow: /Public/6.1.0/ # obsolete data
    ## Disallow: /Public/6.2.0/ # obsolete data
    ## Disallow: /Public/6.3.0/ # obsolete data
    ## Disallow: /Public/7.0.0/ # obsolete data
    ## Disallow: /Public/8.0.0/ # obsolete data
    ## Disallow: /Public/9.0.0/ # obsolete data
    ## Disallow: /Public/10.0.0/    # obsolete data
    ## Disallow: /fr/           # obsolete pages and charts
    ## Disallow: /versions/Unicode2.0.0/    # obsolete version
    ## Disallow: /versions/Unicode2.1.0/    # obsolete version
    ## Disallow: /versions/Unicode3.0.0/    # obsolete version
    ## Disallow: /versions/Unicode3.0.1/    # obsolete version
    ## Disallow: /versions/Unicode3.1.0/    # obsolete version
    ## Disallow: /versions/Unicode3.1.1/    # obsolete version
    ## Disallow: /versions/Unicode3.2.0/    # obsolete version
    ## Disallow: /versions/Unicode4.0.0/    # obsolete version
    ## Disallow: /versions/Unicode4.0.1/    # obsolete version
    ## Disallow: /versions/Unicode4.1.0/    # obsolete version
    ## Disallow: /versions/Unicode5.0.0/    # obsolete version
    ## Disallow: /versions/Unicode5.1.0/    # obsolete version
    ## Disallow: /versions/Unicode5.2.0/    # obsolete version
    ## Disallow: /versions/Unicode6.0.0/    # obsolete version
    ## Disallow: /versions/Unicode6.1.0/    # obsolete version
    ## Disallow: /versions/Unicode6.2.0/    # obsolete version
    ## Disallow: /versions/Unicode6.3.0/    # obsolete version
    ## Disallow: /versions/Unicode7.0.0/    # obsolete version
    ## Disallow: /versions/Unicode8.0.0/    # obsolete version
    ## Disallow: /versions/Unicode9.0.0/    # obsolete version
    ## Disallow: /versions/Unicode10.0.0/   # obsolete version
    ## Disallow: /charts/PDF/Unicode-3.1    # obsolete version
    ## Disallow: /charts/PDF/Unicode-3.2    # obsolete version
    ## Disallow: /charts/PDF/Unicode-4.0    # obsolete version
    ## Disallow: /charts/PDF/Unicode-4.1    # obsolete version
    ## Disallow: /charts/PDF/Unicode-5.0    # obsolete version
    ## Disallow: /charts/PDF/Unicode-5.1    # obsolete version
    ## Disallow: /charts/PDF/Unicode-5.2    # obsolete version
    ## Disallow: /charts/PDF/Unicode-6.0    # obsolete version
    ## Disallow: /charts/PDF/Unicode-6.1    # obsolete version
    ## Disallow: /charts/PDF/Unicode-6.2    # obsolete version
    ## Disallow: /charts/PDF/Unicode-6.3    # obsolete version
    ## Disallow: /charts/PDF/Unicode-7.0    # obsolete version
    ## Disallow: /charts/PDF/Unicode-8.0    # obsolete version
    ## Disallow: /charts/PDF/Unicode-9.0    # obsolete version
    ## Disallow: /charts/PDF/Unicode-10.0   # obsolete version
    ## Disallow: /reports/tr1.html  # obsolete TR
    ## Disallow: /reports/tr2.html  # obsolete TR
    ## Disallow: /reports/tr3.html  # obsolete TR
    ## Disallow: /reports/tr1/  # obsolete TR
    ## Disallow: /reports/tr2/  # obsolete TR
    ## Disallow: /reports/tr3/  # obsolete TR
    ## Disallow: /reports/tr7/  # obsolete TR
    ## Disallow: /reports/tr12/ # obsolete TR
    ## Disallow: /reports/tr13/ # obsolete TR
    ## Disallow: /reports/tr19/ # obsolete TR
    ## Disallow: /reports/tr21/ # obsolete TR
    ## Disallow: /reports/tr30/ # obsolete TR
    ## Disallow: /reports/tr32/ # obsolete TR
    ## Disallow: /reports/tr40/ # obsolete TR
    ## Disallow: /reports/tr47/ # obsolete TR
    ## Disallow: /reports/tr49/ # obsolete TR
    ## Disallow: /reports/tr52/ # obsolete TR
    ## Disallow: /anon-ftp/ # same as Public, different path
    ## Disallow: /Public/MAPPINGS/OBSOLETE/ # obsolete data
    ## Disallow: /unicode/members/
    ## Disallow: /members/
    ## Disallow: /repository/   # dynamic
    ## Disallow: /cldr/repository/  # dynamic
    ## Disallow: /cldr/utility/ # dynamic
    ## Disallow: /cgi-bin/  # dynamic
    ## Disallow: /cldr/data/diff/   # dynamic
    ## Disallow: /cldr/trac/    # dynamic
    ## Disallow: /edcom/bugtrack/   # dynamic
    ## Disallow: /uli/trac/ # dynamic
    ## Disallow: /~srloomis/ut/trac/    #dynamic
    ## Disallow: /repos/    # dynamic
    ## Disallow: /repos/cldr-tmp/   # dynamic
    ## Disallow: /utility/trac/ # dynamic
    ## Disallow: /utility/  # dynamic
    ## Disallow: /forum/    # dynamic
    ## Disallow: /forum/viewtopic.php   # dynamic, discontinued
    ## Allow: /cldr/data/common/
    ## Allow: /cldr/data/docs/
    ## Allow: /cldr/data/tools/
    ## Disallow: /cldr/data/   # everything else
    ## Disallow: /cldr/dropbox/ # not really public
    ## Disallow: /~ecartis  # not really public
    ## Disallow: /unibook/oldversions/  # obsolete data
    ## Disallow: /reports/tr46/tr46-4.html
    ## Disallow: /reports/tr46/tr46-3.html
    ## Disallow: /reports/tr46/tr46-2.html
    ## Disallow: /reports/tr46/tr46-1.html
    ## Disallow: /reports/tr15/tr15-24.html
    ## Disallow: /reports/tr15/tr15-23.html
    ## Disallow: /reports/tr15/tr15-31.html
    ## Disallow: /reports/tr14/tr14-15.html
    ## Disallow: /reports/tr9/tr9-21.html
    ## Disallow: /reports/tr51/tr51-1.html
    ## Disallow: /reports/tr51/tr51-1-archive.html
    ## Disallow: /reports/tr51/tr51-2.html
    ## Disallow: /reports/tr34/tr34-1.html
    ## Disallow: /reports/tr34/tr34-2.html
    ## Disallow: /reports/tr34/tr34-3.html
    ## Disallow: /reports/tr34/tr34-4.html
    ## Disallow: /reports/tr34/tr34-5.html
    ## Disallow: /reports/tr34/tr34-6.html
    ## Disallow: /reports/tr34/tr34-7.html
    ## Disallow: /reports/tr34/tr34-8.html
    ## Disallow: /reports/tr34/tr34-9.html
    ## Disallow: /reports/tr34/tr34-10.html
    ## Disallow: /reports/tr34/tr34-11.html
    ## Disallow: /reports/tr34/tr34-12.html
    ## Disallow: /reports/tr34/tr34-13.html
    ## Disallow: /reports/tr34/tr34-14.html
    ## Disallow: /reports/tr34/tr34-15.html
    ## Disallow: /reports/tr34/tr34-16.html
    ## Disallow: /reports/tr34/tr34-17.html
    ## Disallow: /reports/tr34/tr34-18.html
    ## User-Agent: AhrefsBot
    ## Crawl-Delay: 60
    ## User-Agent: Baiduspider
    ## Crawl-Delay: 60
    ## User-agent: CCBot
    ## Crawl-Delay: 60
    ## User-agent: Gigabot
    ## Disallow: /
    ## User-agent: betaBot
    ## Disallow: /
    ## User-agent: CCBot
    ## Crawl-Delay: 60
    ## User-agent: AhrefsBot
    ## Crawl-Delay: 60
    ## User-agent: dotbot
    ## Crawl-Delay: 60
    ## User-agent: MJ12bot
    ## Crawl-Delay: 60
    ## User-agent: msnbot
    ## Crawl-delay: 10

There is a crawl delay for all users of 60 seconds, which as we are just scraping two pages this isn't a big deal, we can easily achieve this by setting a sleep short sleep for the system in between the two scrapes using `Sys.sleep(60)`.

The `paths_allowed()` functions will return true if the path is allowed, based on the robots.txt file for the domain.

{% highlight r %}
robotstxt::paths_allowed(url_base)
{% endhighlight %}

    ## [1] TRUE

{% highlight r %}
robotstxt::paths_allowed(url_skin)
{% endhighlight %}

    ## [1] TRUE

Both come back `TRUE`, we are allowed to scrape them.

### Scraping the emoji lists

As we discovered above, we are allowed to scrape this site, they just ask we don't scrape more than once a minute.

{% highlight r %}
emoji_base = read_html(url_base) %>%
  html_nodes("table") %>%
  html_table()

Sys.sleep(60) # You can interupt the sleep with the 'esc' button from the R console

emoji_skin = read_html(url_skin) %>%
  html_nodes("table") %>%
  html_table()
{% endhighlight %}

Now to clean up the tables we scraped, so they are nice for us to use later.

{% highlight r %}
emoji_ref_interim1 = as.data.frame(emoji_base) %>%
  select(Smileys.1, Smileys.2, Smileys.14) %>%
  rename(code = Smileys.1, emoji = Smileys.2, short_name = Smileys.14) %>%
  filter(code != "Code") %>%
  mutate(descripID = code == emoji) %>%
  mutate(table = "base")

# The "keycap: *" emoji caused me no end of headaches and I have no idea why

emoji_ref_interim2 = as.data.frame(emoji_skin) %>%
  select(People.1, People.2, People.14) %>%
  rename(code = People.1, emoji = People.2, short_name = People.14) %>%
  filter(code != "Code") %>%
  mutate(descripID = code == emoji) %>%
  mutate(table = "skin")

emoji_ref_interim = rbind(emoji_ref_interim1, emoji_ref_interim2)

# This part here just lets us get the group descriptions for the emoji from the table and make them a variable
descriptions = as.data.frame(emoji_ref_interim) %>%
  filter(descripID == TRUE) %>%
  tibble::rownames_to_column(var = "number")

times = c(diff(which(emoji_ref_interim$descripID)), length(emoji_ref_interim$descripID) - max(which(emoji_ref_interim$descripID))+1)

group_desc = rep(descriptions$code, times-1)

emoji_ref = emoji_ref_interim %>%
  filter(descripID == FALSE) %>%  
  mutate(group_description = group_desc) %>%
  tibble::rownames_to_column(var = "number") %>%
  mutate(number = as.numeric(number)) %>%
  mutate(character_length = nchar(code)) %>%
  arrange(number)
{% endhighlight %}

One wrinkle is that as far as I can tell, you can't stop the Twitter app from truncating tweets that have been retweeted when using twitteR. This seems possible in rtweet - but I couldn't get the authentication working. More [on the GitHub issues page](https://github.com/mkearney/rtweet/issues/179).

So, I'm just going to try to make this work out by getting the untruncated.

{% highlight r %}
unique_icots = icots_tweets %>%
  distinct(text, .keep_all=TRUE) %>%
  filter(truncated == FALSE)

unique_user = user_tweets %>%
  distinct(text, .keep_all=TRUE) %>%
  filter(truncated == FALSE)

unique_icml = icml_tweets %>%
  distinct(text, .keep_all=TRUE) %>%
  filter(truncated == FALSE)

emoji_list_icots = str_extract_all(unique_icots$text, '[\U{10000}-\U{1FFFF}]|[\U{20A0}-\U{27FF}]')
emoji_icots = data.table::rbindlist(lapply(emoji_list_icots, as.data.frame), id="src", fill = TRUE, use.names = TRUE)

emoji_spread_icots = emoji_icots %>%
  rename(ID = src, emoji = `X[[i]]`) %>%
  group_by(ID) %>%
  mutate(counter = row_number()) %>%
  spread(counter, emoji, fill = NA) %>%
  unite(all, -ID, sep="", remove=FALSE) %>%
  mutate(all = str_replace_all(all, "NA", ""))

emoji_list_user = str_extract_all(unique_user$text, '[\U{10000}-\U{1FFFF}]|[\U{20A0}-\U{27FF}]')
emoji_user = data.table::rbindlist(lapply(emoji_list_user, as.data.frame), id="src", fill = TRUE, use.names = TRUE)

emoji_spread_user = emoji_user %>%
  rename(ID = src, emoji = `X[[i]]`) %>%
  group_by(ID) %>%
  mutate(counter = row_number()) %>%
  spread(counter, emoji, fill = NA) %>%
  unite(all, -ID, sep="", remove=FALSE) %>%
  mutate(all = str_replace_all(all, "NA", ""))

emoji_list_icml = str_extract_all(unique_icml$text, '[\U{10000}-\U{1FFFF}]|[\U{20A0}-\U{27FF}]')
emoji_icml = data.table::rbindlist(lapply(emoji_list_icml, as.data.frame), id="src", fill = TRUE, use.names = TRUE)

emoji_spread_icml = emoji_icml %>%
  rename(ID = src, emoji = `X[[i]]`) %>%
  group_by(ID) %>%
  mutate(counter = row_number()) %>%
  spread(counter, emoji, fill = NA) %>%
  unite(all, -ID, sep="", remove=FALSE) %>%
  mutate(all = str_replace_all(all, "NA", ""))
{% endhighlight %}

How many tweets (not counting retweets) use at least one emoji in this data?

{% highlight r %}
emoji_prop_icots = sum(!is.na(emoji_spread_icots$`1`))/dim(unique_icots)[1]
emoji_prop_user = sum(!is.na(emoji_spread_user$`1`))/dim(unique_user)[1]
emoji_prop_icml = sum(!is.na(emoji_spread_icml$`1`))/dim(unique_icml)[1]
{% endhighlight %}

So 6% of ICOTS tweets had at least one emoji in them, 13% of useR tweets and 4% of ICML tweets.

#### Which emoji were the most popular?

This is a not very elegant function to count how many times each emoji in the emoji\_ref dataframe appears in the text you're analysing (in the form of the all columns from the emoji\_spread dataset). I'm also not 100% sure the tryCatch is set up properly - it needs a tryCatch because I keep getting errors for the keycap: \* emoji.

{% highlight r %}
emoji_counter = function(data_col, emoji_from_ref){
  counts = tryCatch({
    sum(str_count(data_col, emoji_from_ref))
  }, error=function(e)counts=NA, {}
  )
  return(counts)
}
{% endhighlight %}

{% highlight r %}
# This can take a bit of time to run
emoji_counts_icots = emoji_ref %>%
  rowwise() %>%
  mutate(counts = NA) %>%
  mutate(counts = emoji_counter(emoji_spread_icots$all, emoji)) %>%
  arrange(number) %>%
  mutate(rank = round(rank(-counts), 0)) %>%
  select(-c(character_length, number)) %>%
  filter(counts > 0) %>%
  arrange(desc(counts)) %>%
  filter(rank <= 5) %>%
  select(emoji, short_name, counts)
emoji_counts_icots
{% endhighlight %}

    ## # A tibble: 6 x 3
    ##   emoji short_name                   counts
    ##   <chr> <fct>                         <int>
    ## 1 😍    smiling face with heart-eyes      8
    ## 2 👏    clapping hands                    5
    ## 3 😂    face with tears of joy            2
    ## 4 🐱    cat face                          2
    ## 5 🎨    artist palette                    2
    ## 6 🎼    musical score                     2

{% highlight r %}
emoji_counts_user = emoji_ref %>%
  rowwise() %>%
  mutate(counts = NA) %>%
  mutate(counts = emoji_counter(emoji_spread_user$all, emoji)) %>%
  arrange(number) %>%
  mutate(rank = round(rank(-counts), 0)) %>%
  select(-c(character_length, number)) %>%
  filter(counts > 0)  %>%
  arrange(desc(counts)) %>%
  filter(rank <= 5) %>%
  select(emoji, short_name, counts)
emoji_counts_user
{% endhighlight %}

    ## # A tibble: 5 x 3
    ##   emoji short_name                   counts
    ##   <chr> <fct>                         <int>
    ## 1 👌    OK hand                          11
    ## 2 📦    package                           9
    ## 3 😍    smiling face with heart-eyes      7
    ## 4 👍    thumbs up                         5
    ## 5 👏    clapping hands                    5

{% highlight r %}
emoji_counts_icml = emoji_ref %>%
  rowwise() %>%
  mutate(counts = NA) %>%
  mutate(counts = emoji_counter(emoji_spread_icml$all, emoji)) %>%
  arrange(number) %>%
  mutate(rank = round(rank(-counts), 0)) %>%
  select(-c(character_length, number)) %>%
  filter(counts > 0)  %>%
  arrange(desc(counts)) %>%
  filter(rank <= 5) %>%
  select(emoji, short_name, counts)
emoji_counts_icml
{% endhighlight %}

    ## # A tibble: 5 x 3
    ##   emoji short_name                      counts
    ##   <chr> <fct>                            <int>
    ## 1 🔥    fire                                 9
    ## 2 😄    grinning face with smiling eyes      2
    ## 3 🤔    thinking face                        2
    ## 4 ✈    airplane                             2
    ## 5 ✔    heavy check mark                     2

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
### Vanity time: My best tweets?

I kinda want to know which of my tweets did the best too...

{% highlight r %}
mytweets = all_tweets %>%
  filter(screenName == "Liza_Bolton")

myprop = dim(mytweets)[1]/dim(icots_tweets)[1]

mytweets %>%
  arrange(desc(retweetCount)) %>%
  select(text, retweetCount) %>%
  head()
{% endhighlight %}

    ##                                                                                                                                                             text
    ## 1                   Our brains can be huge drama mongers - learning how to think through these dramatic instincts is a key part of unde… https://t.co/lDqEq1DkFV
    ## 2 Practical advice for Stats educators from @hspter:\n\U0001f469‍\U0001f3eb "Teach design as a fundamental process that is independent o… https://t.co/MCzUNRhu3y
    ## 3                    "What should we want most for our students? The ability to do things machines can't do!" - Chris Wild (Session 9A)… https://t.co/dYN7aIkZPa
    ## 4                                                         The 10 chapters of #Factfulness help us manage our dramatic instincts #icots10 https://t.co/3AEqTTpukr
    ## 5                    Why did more Swedes know extreme poverty had halved? When asked they wrote in "Hans Rosling". I teared up. #icots10 https://t.co/6OZvFfbktY
    ## 6                   "Crime data doesn't exist...the only things we have is arrest data". -@jo_hardin47 (Session 3A) some great points a… https://t.co/DOf6eGgIqJ
    ##   retweetCount
    ## 1            8
    ## 2            8
    ## 3            7
    ## 4            5
    ## 5            5
    ## 6            4

I tweeted 7% of the \#ICOTS10 tweets.
