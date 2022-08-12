# Project: WeRateDogs Data Wrangling Project

## Overview

This is submitted to Udacity as a project on Data Wrangling as part of the requirements for the Udacity Data Analytics Nanodegree program.   

## Introduction

The dataset that I __wrangled__, analyzed and visualized is the tweet archive of Twitter user @dog_rates, also known as WeRateDogs. WeRateDogs is a Twitter account that rates people's dogs with a humorous comment about the dog. These ratings almost always have a denominator of 10. The numerators, though? Almost always greater than 10. 11/10, 12/10, 13/10, etc. Why? Because "they're good dogs Brent.

## Dataset Description 

The dataset used is gathered from three data sources, the first one is the twitter archive of @dog_rates submitted to Udacity, the second one is the image prediction data which is programmatically gotten from [cloud front](https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv) to know whether the animal is dog or another animal using requests module, while the last one is gotten by scrapping favorite count and retweet count of each tweet ids from twitter api using tweepy.

## Methods Used

- descriptive statistics
- programmatic data gathering
- data visualization


## Quality and Tidiness Issues Found and Resolved

### Quality Issues

#### *df2 (data extracted with tweepy)*
* Some of the extracted data in the retweet_count and favourite_count contains some symbols which are not part of the desired data
* The data in retweet_count and favorite_count are meant to be int but they are read as string

#### *df (twitter_archive data)*
* Remove all the retweets in the dataset.
* In df (i.e. twitter_archive dataset), some of the columns have smaller data, less than 10% of the dataset rendering such columns unsuitable for analysis without regathering through for all tweet_id such as in_reply_ columns and retweeted_status_columns
* Missing values in expanded_urls
* There are some ids in which the animal is not dog id=849776966551130114
* the timestamp in twitter_archive should be of the data type datetime and not string(object)
* Inconsistent numerator and denominator ratings (i.e. numerators and denominators having common multiples and denominators in 10s) at the ids 697463031882764288, 675853064436391936, 713900603437621249, 677716515794329600, 684225744407494656, 704054845121142784, 709198395643068416, 710658690886586372, 716439118184652801, 731156023742988288, 758467244762497024, 820690176645140481;  (75 instead of 9.75) at 832215909146226688, 786709082849828864, 680494726643068929;	(26 instead of 11.26), 778027034220126208 (27 instead of 11.27); for 810984652412424192, 24\7 means every time and not ratings; for 666287406224695296 (1/2 instead of 9/10);for 740373189193256964 (9/11 instead of 14/10); 682962037429899265 (7/11 instead of 10/10)
* Invalid dog names a, an. So, we can use text column to re-extract dog names for the affected dogs.

### Tidiness Issues
- doggo, floofer, pupper and puppo are maturity stages in dog. Hence, the columns with these names in the dataframe twitter_archive (df) should be one column named _'dog_stages'_
- There should be one dataframe, that is merging the three datasets on the tweet ids.


## Key Insights

- Dogs whose dog_stages are not specified has the highest total favorite and retweet count, which make it not logical to really determine the dog_stage with the highest retweet count and favorite count. Hence, @WeRateDogs should mandate clients to indicate dog stages for every submission for a better analysis. But for the specified, the analysis shows that __'pupper'__ has the highest total favorite count and same as the highest retweet count, followed by __'doggo'__, then __'puppo'__ and so on.
- 'doggo/puppo' have the highest average rating_numerator while 'pupper' has the lowest average rating_numerator.
- There is a slightly positive correlation between retweet count and favorite count, because the correlation is not evenly distributed across the values. This implies that a dog with high favorite count will have high retweet count and vice versa, but this only hold for dogs whose retweet count is less than 20,000 and its favorite count is not more than 40,000. To gain more insight in to this, a predictive analysis can be carried out on the data or gather more tweet ids with relatively high retweet and favorite count.