---
layout: post
title: "BNZ Data Challenge Recruitment Event"
---

Notes and ideas from a BNZ recruitment event for data scientists on 27 Sept 2017 for anyone who was curious about what went on. I'm on [Twitter](https://twitter.com/Liza_Bolton) if you have any questions, or check out [my website](www.dataembassy.co.nz) for more general stuff.

## Liza's one sentence summary
BNZ's Analytics and Insights team seems like a **great employment option** for data literate folks, but make sure you bring your **well-rounded** A-game, **data crunching alone is not enough**.

## My key take homes
- They seemed like a pretty cool team, and didn't just trot out the preppy grads, you got access to people with real experience and skills who were managers and leaders.
- BNZ is trying to stay on the cutting edge when it comes to the tools and skills they use and seem to be doing a good job of it.
- There was an emphasis from most of the speakers on communication skills and seeking diverse backgrounds. Being well rounded is important if your data skills are going to actually make a difference. Find ways to build those skills in your studies so you can shine at these kinds of events!

### Comments on the Data Challenge
- The Challenge itself seemed to be more about getting you started making something to submit later as part of your application. I kind of wished we could talk about it as a larger group and it would have been neat for them to model how they might have approached it. People might not have wanted to share so as to keep their competitive edge - would be nice to have a way to signal groups work ability.
- Actual BNZ type data would have been even more interesting, but I guess they didn't want it to look like they were getting us to do free work...
- Lots of people hadn't looked at the data beforehand (hey, students, eh?) so the group work bit wasn't as productive a brainstorm as it could have been, but it was still fun to talk to cool data literate folks.


## Notes from speakers
![Speakers, teams and roles](images/Aistructure.jpg)

### Sarah
*Head of Enterprise Insight*

- "A number of us claim we have the best job in the bank"
- Seeking analytical mindsets, not necessarily just traditional data training - Sarah has an Honours in Criminology from Vic

[Sarah's LinkedIn](https://www.linkedin.com/in/sarah-cawsey-55002925/?ppe=1)

### Macushla and David
*Lean analytcis team, David is Chief Analytics Officer*

- "Human centred design"
- Mix of data scientists and qualitative research
- Cycle of work and problem solving, fast paced

[Macushla's LinkedIn](https://www.linkedin.com/in/macushla-howell-89546a71/)

[David's LinkedIn](https://www.linkedin.com/in/davidthomasnz/)

###Reema
*Head of Market Research and Partnerships*

- Been with BNZ for 8 years
- NPS = net promoter score *(I learned a new thing)*

[Reema's LinkedIn](https://www.linkedin.com/in/reema-wentworth-930b90127/)

###Shawn
*Head of Analytics Transformation*

- Was wearing and awesome Cloudera "Data is the new bacon" t-shirt
- Comes from a business intelligence background

[Shawn's LinkedIn](https://www.linkedin.com/in/shawnmlewis/)

### Cathy
*Manages Performance Analytics team within Sarah's team*

- Also an Arts background

### Building The Bridge video
- Built originally with PwC, now totally owned in house.
- Strengthening the links between coalface and analytics teams.
- How to operationalise insights the big issue. It is all about communication.
- In-store testing with customers of prototypes.
- Really neat efforts at ["sensemaking"](https://www.bookdepository.com/Sensemaking-Christian-Madsbjerg/9781408708378), which ties in nicely with a book I'm reading at the moment.
- Other issues around pace and business cycles.
- Didn't necessarily explain what "Building the Bridge" was going...but everyone in the video feels really good about it!

## What inspires them?
- Lots of freedom to be creative and "disruptive"
- Control over work space, may sound trivial but this is actually really cool. Autonomy and the right resources <3
- Diverse backgrounds and opinions in the team
- 4/6 of speakers women! Doing well on gender

## What are they working on?
- Sarah has been working on a messaging bot. Doing some testing and play.
    - The bot is meant to help answer the basic email questions
    - Start with sentiment analysis
        - If the customer is already mad DO NOT BOT
      - Personalisation, do customers use their phone or computer for logins? Do do you send them a text or an email?
- Use Cloudera, one of the few companies doing end to end data infrastructures well in NZ
    - "Appetite do things faster and first"
- Classic customer segmentation work pieces as well

## What do they look for in potential hires?
- Can you explain and have a conversation about what you're doing?
    - The most exciting technique in the world won't do you much good if you can't explain it...
- Range of backgrounds
- Open to new ways of thinking
- Passion for the customers and business
- Nice emphasis on a job that is supportive/you can bring your authentic self
    - Mentorship pathways
- "It's all happening". It is really hard to say what the next few years are going to look like.

## Questions
- What tools are you using?
    - Cloudera, Tableau for viz, Zeppelin with Hadoop, Spark R, SPSS, Hive/Pig *(and I missed a bunch of others too)**
- How do you select students?
    - Graduate programme, 4 grads starting next February
    - Assessment centre day pretty intense
    - They expect that they are going to teach new hires a lot
    - This kind of event is how they are trying to innovate (yay! success!)
- Business as a whole receptive to change?
    - Executive sponsorship
    - Good buy-in across the business now
    - Not a traditional bank, we're a data company
- What is the future of the team in terms of growth?
    - Looking to grow significantly in the next few years

## Recruitment-y stuff
**Caroline** from BNZ HR will send exclusive recruitment links out through CDES. Options for future graduate programme and general talent pool paddling.

# Data Challenge
Attendees were asked beforehand to check out the [NYC Tree dataset](https://www.kaggle.com/nycparks/tree-census) on Kaggle and consider ways it could be commercialised.

![Data Challenge Questions](images/datachallenge.jpg)

## General ideas

What follows are some of the things I had brainstormed beforehand. I did a weird amount of research on tree pollen.

- Real estate site plugin for people with allergies to use when considering homes
      - There has been a move toward more allergenic trees over time - especially in the 1960s and 1970s due to Dutch Elm disease killing off lots of the low allergenic native American elm. 30% of adults and 40% could be pollen sensitive. Lots of info in this [NYT article from 2010](http://www.nytimes.com/2010/04/06/opinion/06ogren.html?mcubz=3).
    - Female plant produce no pollen
    - Norway maples and London planes always produce pollen
    - Allergy and depression link? https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2592251/
- Predictive modelling for housing values
- Wi-Fi router locations
- Community time capsules - also can have sponsorship from local businesses or chains
- Selling consulting services back to the City Parks team
    - Which areas are high turnover and why?
        - Wrong trees for the location?
        - Disease
    - Projecting costs for future years based on current turnover rates
    - Climate change risk modelling
        - How will the current tree stock fare in a changing climate?
        - Will tree varieties need to change in future?
        - How can the current trees and future plans help deal with more severe weather, especially heat
    - Pollution and air quality
    - Planning more diversity of tree types in locations to support lower allergy levels, greater range better for people (see NYT article)

## Additional data that might be useful
- Allergen scores for different tree species
- Historical sales and rental data by borough
    - https://www.trulia.com/home_prices/New_York/New_York-heat_map/
- Shapefiles for mapping
    - Below I use *Borough Boundaries (Clipped to Shoreline)* from  http://www1.nyc.gov/site/planning/data-maps/open-data/districts-download-metadata.page

## Setting up data



{% highlight r %}
# Packages
library(tidyverse)
library(rgdal) # for spatial stuff

# Read in 4 datasets from Kaggle
c95 = read.csv("new_york_tree_census_1995.csv", na.strings = c("", "None", 999999))
c05 = read.csv("new_york_tree_census_2005.csv", na.strings = c("", "None", 999999))
c15 = read.csv("new_york_tree_census_2015.csv", na.strings = c("", "None", 999999))
species = read.csv("new_york_tree_species.csv", na.strings = c("", "None", 999999))

names(c95)
{% endhighlight %}



{% highlight text %}
##  [1] "recordid"           "address"            "house_number"      
##  [4] "street"             "zip_original"       "cb_original"       
##  [7] "site"               "species"            "diameter"          
## [10] "status"             "wires"              "sidewalk_condition"
## [13] "support_structure"  "borough"            "x"                 
## [16] "y"                  "longitude"          "latitude"          
## [19] "cb_new"             "zip_new"            "censustract_2010"  
## [22] "censusblock_2010"   "nta_2010"           "segmentid"         
## [25] "spc_common"         "spc_latin"          "location"
{% endhighlight %}



{% highlight r %}
names(c05)
{% endhighlight %}



{% highlight text %}
##  [1] "objectid"   "cen_year"   "tree_dbh"   "tree_loc"   "pit_type"  
##  [6] "soil_lvl"   "status"     "spc_latin"  "spc_common" "vert_other"
## [11] "vert_pgrd"  "vert_tgrd"  "vert_wall"  "horz_blck"  "horz_grate"
## [16] "horz_plant" "horz_other" "sidw_crack" "sidw_raise" "wire_htap"
## [21] "wire_prime" "wire_2nd"   "wire_other" "inf_canopy" "inf_guard"
## [26] "inf_wires"  "inf_paving" "inf_outlet" "inf_shoes"  "inf_lights"
## [31] "inf_other"  "trunk_dmg"  "zipcode"    "zip_city"   "cb_num"    
## [36] "borocode"   "boroname"   "cncldist"   "st_assem"   "st_senate"
## [41] "nta"        "nta_name"   "boro_ct"    "x_sp"       "y_sp"      
## [46] "objectid_1" "location_1"
{% endhighlight %}



{% highlight r %}
names(c15)
{% endhighlight %}



{% highlight text %}
##  [1] "tree_id"    "block_id"   "created_at" "tree_dbh"   "stump_diam"
##  [6] "curb_loc"   "status"     "health"     "spc_latin"  "spc_common"
## [11] "steward"    "guards"     "sidewalk"   "user_type"  "problems"  
## [16] "root_stone" "root_grate" "root_other" "trunk_wire" "trnk_light"
## [21] "trnk_other" "brch_light" "brch_shoe"  "brch_other" "address"   
## [26] "zipcode"    "zip_city"   "cb_num"     "borocode"   "boroname"  
## [31] "cncldist"   "st_assem"   "st_senate"  "nta"        "nta_name"  
## [36] "boro_ct"    "state"      "latitude"   "longitude"  "x_sp"      
## [41] "y_sp"
{% endhighlight %}



{% highlight r %}
#
{% endhighlight %}

## Exploring the data
This should be made more informative by having the colours match for the different species - but I just made them autumn leaf coloured for now. The general sense also given in the 2010 article linked above is that there are some highly dominant species, of which many are planted, even though there are 133 different species on this list in 2015. It does seem like they may have worked on addressing this more recently though...

{% highlight r %}
length(unique(c15$spc_common))
{% endhighlight %}



{% highlight text %}
## [1] 133
{% endhighlight %}



{% highlight r %}
opar = par()
par(mar=c(2.1,4.1,2.1,0.5), mfrow=c(3,1))

barplot(head(sort(table(c95$spc_common), decreasing = TRUE)/dim(c95)[1]*100, n =10),
     main = "Top 10 species 1995",
     xlab = "Percentage",
     col = "darkorange3",
     las = 1,
     horiz = F
    )

# Bunch of annoying blanks from how the data is set up that I only noticed now, hence [-1] and jiggery pokery with the denominator

# head(c05[which(c05$spc_common == "", arr.ind=T)[1]])

barplot(head(sort(table(c05$spc_common), decreasing = TRUE)[-1]/(dim(c05)[1]-sort(table(c05$spc_common), decreasing = TRUE)[1])*100, n =11),
     main = "Top 10 species 2005",
     xlab = "Percentage",
     col = "darkorange3",
     las = 1,
     horiz = F
    )

barplot(head(sort(table(c15$spc_common), decreasing = TRUE)/dim(c15)[1]*100, n =10),
     main = "Top 10 species 2015",
     xlab = "Percentage",
     col = "darkorange3",
     las = 1,
     horiz = F
    )
{% endhighlight %}

![plot of chunk visualisation](/assets/Rfig/visualisation-1.svg)

## A touch of ggplot
This is based on work in this great post: http://zevross.com/blog/2014/07/16/mapping-in-r-using-the-ggplot2-package/
It seems that the upper half of the island has more of the Pin Oaks, and interestingly the London planetree and Norway maples that are problematic elsewhere, while present here, are outnumbered.

{% highlight r %}
counties = readOGR("nybb.shp", layer="nybb")
{% endhighlight %}



{% highlight text %}
## OGR data source with driver: ESRI Shapefile
## Source: "nybb.shp", layer: "nybb"
## with 5 features
## It has 4 fields
{% endhighlight %}



{% highlight r %}
# Check the projections used in the shape file
proj4string(counties) #NAD83 is the important part
{% endhighlight %}



{% highlight text %}
## [1] "+proj=lcc +lat_1=40.66666666666666 +lat_2=41.03333333333333 +lat_0=40.16666666666666 +lon_0=-74 +x_0=300000 +y_0=0 +datum=NAD83 +units=us-ft +no_defs +ellps=GRS80 +towgs84=0,0,0"
{% endhighlight %}



{% highlight r %}
coordinates(c15) = ~longitude+latitude
class(c15)
{% endhighlight %}



{% highlight text %}
## [1] "SpatialPointsDataFrame"
## attr(,"package")
## [1] "sp"
{% endhighlight %}



{% highlight r %}
# Check projection assigned (you'll need this)
proj4string(c15) # NA = no system
{% endhighlight %}



{% highlight text %}
## [1] NA
{% endhighlight %}



{% highlight r %}
# Lazy - tell R manually what system we want
proj4string(c15)<-CRS("+proj=longlat +datum=NAD83")

# Use the projection from the counties shape file and project the tree data this way too. Include the coord ref system from the shape file.
tree15 = spTransform(c15, CRS(proj4string(counties)))

# Did it work?
identical(proj4string(tree15),proj4string(counties)) # yay!
{% endhighlight %}



{% highlight text %}
## [1] TRUE
{% endhighlight %}



{% highlight r %}
# ggplots fave food is data.frame
tree15 = data.frame(tree15)

# I want to plot the locations of just the top 5 species in Manhattan
man15 = filter(tree15, boroname == "Manhattan")
head(sort(table(man15$spc_common), decreasing = TRUE), n=25)
{% endhighlight %}



{% highlight text %}
##
##         honeylocust        Callery pear              ginkgo
##               13176                7297                5859
##             pin oak             Sophora    London planetree
##                4584                4453                4122
##    Japanese zelkova   littleleaf linden        American elm
##                3596                3333                1698
##     American linden    northern red oak          willow oak
##                1583                1143                 889
##              cherry         Chinese elm           green ash
##                 869                 785                 770
##     swamp white oak       silver linden          crab apple
##                 681                 541                 437
##     golden raintree           red maple        sawtooth oak
##                 359                 356                 353
## Kentucky coffeetree        Norway maple        black locust
##                 348                 290                 259
##           white oak
##                 241
{% endhighlight %}



{% highlight r %}
man15.10 = filter(man15, spc_common %in% names(head(sort(table(man15$spc_common), decreasing = TRUE), n=5)))

#
barplot(head(sort(table(man15$spc_common), decreasing = TRUE)/dim(man15)[1]*100, n =5),
     main = "Top 5 species in Manhattan, 2015",
     xlab = "Percentage",
     col = "darkorange3",
     las = 1,
     horiz = F
    )
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-1.svg)

{% highlight r %}
# Create a nice looking map
ggplot() +  
    geom_polygon(data=counties[which(counties$BoroCode==1),], aes(x=long, y=lat, group=group), fill="grey40",
        colour="grey90", alpha=1)+
    labs(x="", y="", title="Trees of Manhattan - Locations of the top 5 species")+ #labels
    theme(axis.ticks.y = element_blank(),axis.text.y = element_blank(), # get rid of x ticks/text
          axis.ticks.x = element_blank(),axis.text.x = element_blank(), # get rid of y ticks/text
          plot.title = element_text(lineheight=.8, face="bold", vjust=1))+ # make title bold and add space
    geom_point(aes(x=longitude, y=latitude, color=factor(spc_common)), data=man15.10, alpha=0.5, size=0.3)+
    coord_equal(ratio=1) # square plot to avoid the distortion
{% endhighlight %}



{% highlight text %}
## Regions defined for each Polygons
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-2.svg)

{% highlight r %}
ggplot() +  
    geom_polygon(data=counties[which(counties$BoroCode==1),], aes(x=long, y=lat, group=group), fill="grey40",
        colour="grey90", alpha=1)+
    labs(x="", y="", title="Honeylocust")+ #labels
    theme(axis.ticks.y = element_blank(),axis.text.y = element_blank(), # get rid of x ticks/text
          axis.ticks.x = element_blank(),axis.text.x = element_blank(), # get rid of y ticks/text
          plot.title = element_text(lineheight=.8, face="bold", vjust=1))+ # make title bold and add space
    geom_point(aes(x=longitude, y=latitude, color=factor(spc_common)), data=man15.10[which(man15.10$spc_common=="honeylocust"),], alpha=0.5, size=0.3)+
    coord_equal(ratio=1) # square plot to avoid the distortion
{% endhighlight %}



{% highlight text %}
## Regions defined for each Polygons
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-3.svg)

{% highlight r %}
ggplot() +  
    geom_polygon(data=counties[which(counties$BoroCode==1),], aes(x=long, y=lat, group=group), fill="grey40",
        colour="grey90", alpha=1)+
    labs(x="", y="", title="Callery pear")+ #labels
    theme(axis.ticks.y = element_blank(),axis.text.y = element_blank(), # get rid of x ticks/text
          axis.ticks.x = element_blank(),axis.text.x = element_blank(), # get rid of y ticks/text
          plot.title = element_text(lineheight=.8, face="bold", vjust=1))+ # make title bold and add space
    geom_point(aes(x=longitude, y=latitude, color=factor(spc_common)), data=man15.10[which(man15.10$spc_common=="Callery pear"),], alpha=0.5, size=0.3)+
    coord_equal(ratio=1) # square plot to avoid the distortion
{% endhighlight %}



{% highlight text %}
## Regions defined for each Polygons
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-4.svg)

{% highlight r %}
ggplot() +  
    geom_polygon(data=counties[which(counties$BoroCode==1),], aes(x=long, y=lat, group=group), fill="grey40",
        colour="grey90", alpha=1)+
    labs(x="", y="", title="Ginkgo")+ #labels
    theme(axis.ticks.y = element_blank(),axis.text.y = element_blank(), # get rid of x ticks/text
          axis.ticks.x = element_blank(),axis.text.x = element_blank(), # get rid of y ticks/text
          plot.title = element_text(lineheight=.8, face="bold", vjust=1))+ # make title bold and add space
    geom_point(aes(x=longitude, y=latitude, color=factor(spc_common)), data=man15.10[which(man15.10$spc_common=="ginkgo"),], alpha=0.5, size=0.3)+
    coord_equal(ratio=1) # square plot to avoid the distortion
{% endhighlight %}



{% highlight text %}
## Regions defined for each Polygons
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-5.svg)

{% highlight r %}
ggplot() +  
    geom_polygon(data=counties[which(counties$BoroCode==1),], aes(x=long, y=lat, group=group), fill="grey40",
        colour="grey90", alpha=1)+
    labs(x="", y="", title="Pin Oak")+ #labels
    theme(axis.ticks.y = element_blank(),axis.text.y = element_blank(), # get rid of x ticks/text
          axis.ticks.x = element_blank(),axis.text.x = element_blank(), # get rid of y ticks/text
          plot.title = element_text(lineheight=.8, face="bold", vjust=1))+ # make title bold and add space
    geom_point(aes(x=longitude, y=latitude, color=factor(spc_common)), data=man15.10[which(man15.10$spc_common=="pin oak"),], alpha=0.5, size=0.3)+
    coord_equal(ratio=1) # square plot to avoid the distortion
{% endhighlight %}



{% highlight text %}
## Regions defined for each Polygons
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-6.svg)

{% highlight r %}
ggplot() +  
    geom_polygon(data=counties[which(counties$BoroCode==1),], aes(x=long, y=lat, group=group), fill="grey40",
        colour="grey90", alpha=1)+
    labs(x="", y="", title="Sophora")+ #labels
    theme(axis.ticks.y = element_blank(),axis.text.y = element_blank(), # get rid of x ticks/text
          axis.ticks.x = element_blank(),axis.text.x = element_blank(), # get rid of y ticks/text
          plot.title = element_text(lineheight=.8, face="bold", vjust=1))+ # make title bold and add space
    geom_point(aes(x=longitude, y=latitude, color=factor(spc_common)), data=man15.10[which(man15.10$spc_common=="Sophora"),], alpha=0.5, size=0.3)+
    coord_equal(ratio=1) # square plot to avoid the distortion
{% endhighlight %}



{% highlight text %}
## Regions defined for each Polygons
{% endhighlight %}

![plot of chunk ggplot-attempt](/assets/Rfig/ggplot-attempt-7.svg)

There are **heaps** of other cool things I think you could do with this data, but I have already procrastinated enough on this tonight. I'm not going to apply as my PhD is my focus at the moment, but this talk did get me thinking about BNZ as a great potenial employer...though lecturing and my consulting business are probably still my main plan.


Are you going to apply? What is your great commercialisation idea for this Kaggle dataset? Let me know!
