---
layout: post
title: PySpark Note - Find the most common value I
date: 2020-05-05
tags: [PySpark]
---

#### Problems ####
* How to find the most common product each customer bought in the given table, as well as its count?

##### Sample Input #####

| Id |  Product  | Date       |
|:--:|:---------:| :---------:|
|  1 |   Milk    | 11/22/2019 |
|  1 |    Egg    | 11/22/2019 |
|  1 |    Egg    | 11/27/2019 |
|  2 |   Sugar   | 10/02/2019 |
|  2 |   Milk    | 10/02/2019 |
|  2 |   Milk    | 11/14/2019 |
|  2 |   Milk    | 12/09/2019 |

***

#### Analysis ####

Here we define a function for more generic use, as follows:  

##### Parameters #####
* df : spark.DataFrame
* col_name: string, column name

##### Returns #####
* most_common : spark.DataFrame

Return frame with following columns
* ID
* MostCommon: most common value
* MostCommonCount: number of reviews mentioned the most common value

For the example problem, for each **Id**, we want to find its most common **Product**.
So the **col_name = Product**. We can break down our question into steps:
1. Select the corresponding columns from dataset
2. Calculate the frequency of each value in the given column: (ID,product),count
3. Using **Window** functions get the most common value (sort “count” in descending order and get the first row)

##### Solution #####

{% highlight python %}
from pyspark.sql import functions as sf
from pyspark.sql import Window

def find_most_common(df,col_name):
    # Select columns and calculate frequency
    df1 = df.select(["ID",col_name])
    frequency_count = (df1.groupby(["ID",col_name]).count()
                         .where(sf.col(col_name).isNotNull())
    )
    # Apply Window functions, sort, get the most common value
    window = Window.partitionBy("ID").orderBy(sf.desc("count"))
    most_common = (frequency_count.withColumn("_rn",sf.row_number()
                        .over(window)).where(sf.col("_rn")==1).drop("_rn")
    )

    most_common = most_common.select(
                        sf.col(""),
                        sf.col(col_name).alias("MostCommon"),
                        sf.col("count").alias("MostCommonCount")
    )

    return most_common

{% endhighlight %}


#### Sample Output ####

| Id |  MostCommon  | MostCommonCount |
|:--:|:------------:| :--------------:|
|  1 |    Egg       | 2               |
|  2 |    Milk      | 3               |

#### Highlight ####

* Window functions
