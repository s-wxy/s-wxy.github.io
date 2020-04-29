---
layout: post
title: PySpark Note - Get null when convert StringType to TimestampType
date: 2020-04-28
tags: [PySpark]
---

#### Problems ####
* How to convert StringType to TimestampType?
* Why get null when convert StringType to TimestampType?
* How to get the Year, Month, Day values from the time field?

##### Sample Input #####
| Id | reviewTime            |  
|----|-----------------------|
| 1  |  11/22/2015 7:42:51 PM|   
| 2  |  1/23/2017 2:37:25 PM |
| 3  |  3/17/2019 8:04:37 AM |

***

#### Convert to TimestampType ####
The data type of time information can be StringType sometimes.
When we want to convert it to TimestampType, we should be careful of the output **format** we given in the code.
We could get **null** like this:

| Id | reviewTime            | reviewTime_timestamp |
|----|-----------------------|----------------------|
| 1  |  11/22/2015 7:42:51 PM|   null               |
| 2  |  1/23/2017 2:37:25 PM |   null               |
| 3  |  3/17/2019 8:04:37 AM |   null               |

Few things we need to pay attention to when giving the format:

{% highlight markdown %}
* Math the data!
* The pattern for 24 hour format is **HH**, **hh** is for am./pm.
* **S** for millisecond and **X** for timezone:
* Spark >= 2.2, we can use **to_timestamp**, otherwise we can use **unix_timestamp**
{% endhighlight %}

##### Solution #####

{% highlight python %}
from pyspark.sql import functions as sf

df1 = (df.withColumn("reviewTime_timestamp",
       sf.to_timestamp(sf.col("reviewTime"), format='MM/dd/yyyy HH:mm'))
      )

{% endhighlight %}

***

### Year, Month, Day ###

{% highlight python %}
from pyspark.sql import functions as sf

df2 = df1.select("employerID",
                sf.year("reviewTime_timestamp").alias("reviewYear"))

{% endhighlight %}


### Highlights ###
* LIMIT
* OFFSET
* IFNULL
