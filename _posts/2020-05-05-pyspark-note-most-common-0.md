---
layout: post
title: PySpark Note - How to find the most common value in the given column
date: 2020-05-05
tags: [PySpark]
---

#### Problems ####
* How to find the most common value in the given column?

##### Sample Input #####

| Id |       reviewTime      |
|:--:|:---------------------:|
|  1 | 11/22/2015 7:42:51 PM |
|  2 |  1/23/2017 2:37:25 PM |
|  3 |  3/17/2019 8:04:37 AM |

***

#### Framework ####

##### Parameters #####
* df : spark.DataFrame
* col_name: string, column name

##### Returns #####
* most_common : spark.DataFrame

Return frame with following columns
* employerID
* col_name+"MostCommon": most common value
* col_name+"_maxCount": number of reviews mentioned the most common value

##### Solution #####

{% highlight python %}


def find_most_common(self,df,col_name):
  df1 = df.select(["employerID","reviewID",col_name])
  frequency_count = (df1.groupby(["employerID",col_name]).count()
                       .where(sf.col(col_name).isNotNull())
  )

  window = Window.partitionBy("employerID").orderBy(sf.desc("count"))
  most_common = (frequency_count.withColumn("_rn",sf.row_number()
                      .over(window)).where(sf.col("_rn")==1).drop("_rn")
  )

  most_common = most_common.select(
                      sf.col("employerID"),
                      sf.col(col_name).alias(col_name+"MostCommon"),
                      sf.col("count").alias(col_name+"_maxCount"))

  return most_common

{% endhighlight %}



#### Sample Output ####

| Id |       reviewTime      | Year | Month | Day |
|:--:|:---------------------:|:----:|:-----:|:---:|
|  1 | 11/22/2015 7:42:51 PM | 2015 |   11  |  22 |
|  2 |  1/23/2017 2:37:25 PM | 2017 |   1   |  23 |
|  3 |  3/17/2019 8:04:37 AM | 2019 |   3   |  17 |

Other functions:
* pyspark.sql.functions.dayofweek()
* pyspark.sql.functions.dayofyear
* pyspark.sql.functions.weekofyear()
