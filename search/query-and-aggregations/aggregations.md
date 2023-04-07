# Aggregations

In addition to queries, aggregation capabilities help you analyze your data. Aggregations allow you to group and analyze data based on specific criteria, such as date ranges, terms, or statistical functions. You can use aggregations to generate reports, visualizations, and dashboards to gain insights into your data.

The structure of an aggregation query is as follows:

```json
{
  "aggs": {
    "top-malware-types": {
      "terms": {
        "field": "attributes.malware-type.keyword",
        "size": 50
      }
    },
    "accuracy-stats": {
      "stats": {
        "field": "accuracy"
      }
    }
  }
}
```

In the previous example, we are performing an aggregation that returns the top malware types based on the "attributes.malware-type" field, and we are also obtaining statistics on the "accuracy" field. The "top-malware-types" aggregation uses the "terms" aggregation type to group the results by unique values of the "attributes.malware-type.keyword" field and returns the top 50 results. The "accuracy-stats" aggregation uses the "stats" aggregation type to calculate statistics such as count, min, max, sum, and average on the "accuracy" field.

> Note: If you’re only interested in the aggregation result and not in the results of the query, set size to 0.

Metric aggregations generate straightforward results and cannot include nested aggregations. On the other hand, bucket aggregations produce document buckets that can be nested within other aggregations. By nesting metric and bucket aggregations within bucket aggregations, you can perform complex analyses on your data.

### Table of Contents

1. [Metric Aggregations](aggregations.md#metric)
   1. [Single-value Metric Aggregations](aggregations.md#singleValueMetric)
      1. [sum, min, max and avg](aggregations.md#sum)
      2. [cardinality](aggregations.md#cardinality)
      3. [Value Count](aggregations.md#valueCount)
   2. [Multi-value metric aggregations](aggregations.md#mutiValueMetric)
      1. [Stats, extended\_stats and matrix\_stats](aggregations.md#stats)
      2. [percentiles, percentiles\_ranks](aggregations.md#percentiles)
      3. [Top Hits](aggregations.md#topHits)
2. [Bucket Aggregations](aggregations.md#bucketAggregations)
   1. [Terms](aggregations.md#terms)
   2. [Multi Terms](aggregations.md#multiTerms)
   3. [Significant Terms](aggregations.md#significantTerms)
   4. [Sampler](aggregations.md#sampler)
   5. [Histogram and Date Histogram](aggregations.md#histogram)

### Metric Aggregations <a href="#metric" id="metric"></a>

Metric aggregations let you perform simple calculations such as finding the minimum, maximum, and average values of a field.

### Single-value Metric Aggregations <a href="#singlevaluemetric" id="singlevaluemetric"></a>

Single-value metric aggregations return a single metric.

### sum, min, max, avg <a href="#sum" id="sum"></a>

Single-value metric aggregations, such as the sum, min, max, and avg metrics, enable you to obtain valuable insights about your data. These aggregations calculate different metrics that represent important information about a specific field, including its total value (sum), the lowest value (min), the highest value (max), and the average value (avg).

The following example calculates the average of reputation of the the emails entities:

```json
{
  "aggs": {
    "reputation-avg": {
      "avg": {
        "field": "reputation"
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type.keyword": {
              "value": "email-address"
            }
          }
        }
      ]
    }
  }
}
```

We get a response like:

```json
{
    ... ,
    "aggregations": {
    "reputation-avg": {
      "value": -1.879120879120879
    }
  }
}
```

### Cardinality <a href="#cardinality" id="cardinality"></a>

The cardinality metric is a single-value metric aggregation that counts the number of unique or distinct values of a field.

The following example finds the number of unique types of malware:

```json
{
  "aggs": {
    "unique-malware-types": {
      "cardinality": {
        "field": "attributes.malware-type.keyword"
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        }
      ]
    }
  }
}
```

We get a response like:

```json
{
    ...,
  "aggregations": {
    "unique-malware-types": {
      "value": 30
    }
  }
}
```

Calculating cardinality count accurately can be challenging and resource-intensive. Loading all values into a hash set and counting its size requires a huge amount of memory and can cause high latencies, making it impractical for large datasets.

Fortunately, the precision\_threshold setting allows you to balance memory usage and accuracy. By setting this threshold to a specific value, you can control the trade-off between memory usage and accuracy. The default value of precision\_threshold is 3,000, and the maximum supported value is 40,000. This setting specifies the count level below which the cardinality is expected to be precise, and above which the accuracy may decrease slightly.

### Value Count <a href="#valuecount" id="valuecount"></a>

Number of values an aggregation is based on. This metric is useful in conjunction with other metrics such as avg, as it allows you to determine the number of values used to calculate an average.

This information can be valuable in assessing the accuracy of your results and identifying potential issues in your data.

### Multi-value metric aggregations <a href="#mutivaluemetric" id="mutivaluemetric"></a>

### stats extended\_stats, matrix\_stats <a href="#stats" id="stats"></a>

The **stats** metric is a type of multi-value metric aggregation that provides a comprehensive set of basic metrics in a single query. This aggregation calculates and returns metrics such as min, max, sum, avg, and value\_count all at once.

By using the stats metric, you can easily and efficiently obtain a broad range of information about your data, without having to execute multiple queries. This can save time and resources, especially when working with large datasets.

For Example:

```json
{
  "aggs": {
    "accuracy-stats": {
      "stats": {
        "field": "accuracy"
      }
    }
  }
}
```

We get this as response:

```json
{
    "aggregations": {
        "accuracy-stats": {
            "avg": 1.783289817232376,
            "count": 1149,
            "max": 3,
            "min": 1,
            "sum": 2049
        }
    }
}
```

The **extended\_stats** aggregation is an enhanced version of the stats aggregation. In addition to the basic metrics, such as min, max, sum, avg, and value\_count, it also provides more advanced statistics, including sum\_of\_squares, variance, and std\_deviation.

These additional metrics can be useful for gaining a deeper understanding of your data and identifying patterns and outliers. By leveraging the extended\_stats aggregation, you can obtain a more complete picture of the distribution and variability of your data, and use this information to inform your decisions and strategies.

When we use the same example as before but replace the `stats` keyword with `extended_stats`, the response we obtain includes additional metrics:

```json
{
    "aggregations": {
        "accuracy-stats": {
      "avg": 1.783289817232376,
      "count": 1149,
      "max": 3,
      "min": 1,
      "std_deviation": 0.9183551177362113,
      "std_deviation_bounds": {
        "lower": -0.05342041824004662,
        "lower_population": -0.05342041824004662,
        "lower_sampling": -0.05422020501231728,
        "upper": 3.6200000527047984,
        "upper_population": 3.6200000527047984,
        "upper_sampling": 3.620799839477069
      },
      "std_deviation_population": 0.9183551177362113,
      "std_deviation_sampling": 0.9187550111223466,
      "sum": 2049,
      "sum_of_squares": 4623,
      "variance": 0.8433761222722905,
      "variance_population": 0.8433761222722905,
      "variance_sampling": 0.8441107704624232
    },
    }
}
```

The std\_deviation\_bounds object provides a way to visually represent the variance of the data within an interval of plus or minus two standard deviations from the mean. To adjust the standard deviation to a different value, such as three, you can set the sigma parameter to the desired value.

The **matrix\_stats** aggregation generates advanced statistics for multiple fields in a matrix format.

| Statistic     | Description                                                                                                                                                                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `count`       | The number of samples measured.                                                                                                                                                                                                                                                                         |
| `mean`        | The average value of the field measured from the sample.                                                                                                                                                                                                                                                |
| `variance`    | How far the values of the field measured are spread out from its mean value. The larger the variance, the more it’s spread from its mean value.                                                                                                                                                         |
| `skewness`    | An asymmetric measure of the distribution of the field’s values around the mean.                                                                                                                                                                                                                        |
| `kurtosis`    | A measure of the tail heaviness of a distribution. As the tail becomes lighter, kurtosis decreases. As the tail becomes heavier, kurtosis increases. To learn about kurtosis, see [Wikipedia](https://en.wikipedia.org/wiki/Kurtosis).                                                                  |
| `covariance`  | A measure of the joint variability between two fields. A positive value means their values move in the same direction and vice versa.                                                                                                                                                                   |
| `correlation` | A measure of the strength of the relationship between two fields. The valid values are between \[-1, 1]. A value of -1 means that the value is negatively correlated and a value of 1 means that it’s positively correlated. A value of 0 means that there’s no identifiable relationship between them. |

### percentiles, percentiles\_ranks <a href="#percentiles" id="percentiles"></a>

Percentiles are a useful statistical measure that indicate the relative location of an observation in a dataset. They show the point at which a certain percentage of observed values occur. For example, the 75th percentile represents the value below which 75% of the observations fall.

In addition to identifying outliers, percentiles can be used to gain insight into the distribution of the data. For instance, the range of percentiles can be used to estimate whether the data is skewed, bimodal, or normally distributed.

It's worth noting that in normally distributed data, the 99.87th and 0.13th percentiles correspond to three standard deviations from the mean. Any data outside of this range is often considered an anomaly.

For example:

```json
{
  "aggs": {
    "percentile_reputation": {
      "percentiles": {
        "field": "reputation"
      }
    }
  }
}
```

Ge get this as response:

```json
{
    "aggregations": {
        "percentile_reputation": {
            "values": {
                "1.0": -3,
                "25.0": -2,
                "5.0": -3,
                "50.0": -2,
                "75.0": -1,
                "95.0": 0,
                "99.0": 0
            }
        }
    }
}
```

The percentile rank is a statistical measure that determines the percentage of values that fall at or below a particular threshold, based on a specified grouping. This measure provides insight into how a particular value relates to the overall distribution of the data. For instance, a value with a percentile rank of 80 means that it is greater than or equal to 80% of the values in its group.

```json
{
  "aggs": {
    "percentiles_rank_reputation": {
      "percentile_ranks": {
        "field": "reputation"
        "values": [
          -3,
          -2
        ]
      }
    }
  }
}
```

Ge get this as response:

```json
{
    "aggregations": {
    "reputation_rank": {
      "values": {
        "-2.0": 64.94845360824742,
        "-3.0": 16.49484536082474
      }
    }
  }
}
```

### Top Hits <a href="#tophits" id="tophits"></a>

The multi-value metric aggregation known as top\_hits utilizes a relevance score to rank matching documents based on the field being aggregated. This aggregator can be customized with options such as the starting position of the hit and the maximum size of hits to be returned. Additionally, the matching hits can be sorted by relevance score or other criteria. The top\_hits aggregator is intended to be used as a sub aggregator, making it useful for grouping result sets by specific fields via a bucket aggregator. By determining which properties to slice the result set into, one can effectively make use of the top\_hits aggregator to gather the most relevant documents per bucket.

When using the top\_hits metric aggregation, you can customize the following options to control the results:

**from**: Specifies the starting position of the hit. **size**: Determines the maximum number of top matching hits to be returned for each bucket. The default value is 3. **sort**: Defines how the top matching hits should be sorted. By default, the hits are sorted based on the score of the main query.

In the example, we'll be retrieving the count of malware reports grouped by type in the last 24 hours. Additionally, we'll be using the **top\_hits** aggregation to retrieve the top 3 matching documents per bucket, allowing us to view detailed information on each malware report type. The result will provide valuable insights into the types of malware that are most prevalent in the given timeframe.

```json
{
    "aggs": {
      "most-active-malwares": {
        "terms": {
          "field": "attributes.malware.keyword",
          "size": 50
        },
        "aggs": {
            "top_sales_hits": {
                "top_hits": {
                  "size": 3
                }
              }
        }
      }
    },
    "query": {
      "bool": {
        "filter": [
          {
            "term": {
              "type": {
                "value": "malware"
              }
            }
          },
          {
            "range": {
              "@timestamp": {
                "gte": "now-24h",
                "lte": "now"
              }
            }
          }
        ]
      }
    }
  }
```

### Bucket Aggregations <a href="#bucketaggregations" id="bucketaggregations"></a>

A bucket aggregation is a type of data aggregation that groups together sets of documents based on a specified criterion, effectively creating buckets. Unlike metrics aggregations, which calculate metrics over fields, bucket aggregations categorize documents into sets based on their characteristics. Each bucket is associated with a criterion, which determines whether or not a document "fits" into it. In addition to defining the buckets, bucket aggregations also compute and return the number of documents that are in each bucket.

Bucket aggregations can hold sub-aggregations that are aggregated for the buckets created by their parent bucket aggregation. There are various types of bucket aggregations, each with a different bucketing strategy. Some define a single bucket, while others define a fixed number of multiple buckets, and others dynamically create the buckets during the aggregation process.

### Terms <a href="#terms" id="terms"></a>

The term aggregation is a technique used for data aggregation that creates a dynamic bucket for each unique term of a specified field. The returned values are sorted by key and include the `doc_count`, which specifies the number of documents in each bucket. The buckets are sorted in descending order of doc-count by default.

The response of a terms aggregation includes two additional keys: `doc_count_error_upper_bound` and `sum_other_doc_count`. The `sum_other_doc_count` field is the sum of the documents that are left out of the response when there are many unique terms in the data. If all unique values appear in the response, the `sum_other_doc_count` value is 0. The `doc_count_error_upper_bound` field estimates the maximum possible count for a unique value left out of the final results and can be used to estimate the error margin for the count.

It's important to note that the count may not always be accurate due to the way the aggregation is performed. The coordinating node responsible for the aggregation prompts each shard for its top unique terms. If a shard has an object that's not part of the top requested terms, it won't show up in the response. This is especially true when the size parameter is set to a low number. However, the default size of 10 reduces the likelihood of an error. If high accuracy is not necessary and you want to improve performance, you can reduce the size parameter.

In the following example, we are obtaining the top malware types along with the count for each one:

```json
{
  "aggs": {
    "top-malware-types": {
      "terms": {
        "field": "attributes.malware-type.keyword",
        "size": 50
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        }
      ]
    }
  }
}
```

We receive this as a response:

```json
{
  ...,
  "aggregations": {
    "top-malware-types": {
      "buckets": [
        {
          "doc_count": 728,
          "key": "trojan"
        },
        {
          "doc_count": 181,
          "key": "adware"
        },
        {
          "doc_count": 77,
          "key": "exploit"
        },...
      ],
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 163
    }
  }
}
```

### Multi-Terms <a href="#multiterms" id="multiterms"></a>

The multi-terms aggregation is a type of bucket aggregation that allows you to search for multiple terms and group them together in a single bucket. This aggregation can be useful in scenarios where you need to sort by document count or by a metric aggregation on a composite key to obtain the top N results.

Similar to the terms bucket aggregation, the multi-terms aggregation creates buckets dynamically for each unique set of values. However, it is important to note that the multi-terms aggregation is often slower and consumes more memory than the terms aggregation.

If sorting is not necessary and all values are expected to be retrieved, using nested terms aggregation may be a more efficient solution in terms of speed and memory consumption.

| Parameter    | Description                                                                                                                                                                                                                                                                        |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| multi\_terms | Indicates a multi-terms aggregation that gathers buckets of documents together based on criteria specified by multiple terms.                                                                                                                                                      |
| size         | Specifies the number of buckets to return. Default is 10.                                                                                                                                                                                                                          |
| order        | Indicates the order to sort the buckets. By default, buckets are ordered according to document count per bucket. If the buckets contain the same document count, then `order` can be explicitly set to the term value instead of document count. (e.g., set `order` to “max-cpu”). |
| doc\_count   | Specifies the number of documents to be returned in each bucket. By default, the top 10 terms are returned.                                                                                                                                                                        |

Let's consider a scenario where an analyst wants to identify the top malware families and types in the dataset to prioritize their investigation.

Using Elasticsearch, the analyst can run a query to aggregate the data based on the malware family and type fields, and return the top unique combinations of these values:

```json
{
{
  "aggs": {
    "top-family-types-malware": {
      "multi_terms": {
        "terms": [{
          "field": "attributes.malware-family.keyword" 
        },{
          "field": "attributes.malware-type.keyword"
        }]
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        }
      ]
    }
  }
}
}
```

We receive this as a response:

```json
{...,
"aggregations": {
    "top-malware-types": {
      "buckets": [
        {
          "doc_count": 719,
          "key": [
            "win",
            "trojan"
          ],
          "key_as_string": "win|trojan"
        },
        {
          "doc_count": 181,
          "key": [
            "win",
            "adware"
          ],
          "key_as_string": "win|adware"
        }...],
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 62
    }
  }
}
```

### Significant Terms <a href="#significantterms" id="significantterms"></a>

The significant\_terms aggregation is a powerful tool that enables you to identify unusual or noteworthy term occurrences within a filtered subset of data compared to the rest of the data in the index.

When using the significant\_terms aggregation, the set of filtered documents is known as the foreground set, while the set of all documents in the index is known as the background set. The significant\_terms aggregation then calculates a score for each term, indicating the degree to which its occurrence in the foreground set differs from its occurrence in the background set.

Essentially, this aggregation helps you discover interesting or unusual occurrences of terms in a specific subset of data, allowing you to identify trends or patterns that may be hidden in the larger dataset.

Suppose you want to identify which malware families and types occur most frequently within a subset of documents that meet a certain filter criteria, and compare these occurrences to their frequency in the entire dataset:

```json
{
  "aggs": {
    "malware-family": {
      "terms": { "field": "attributes.malware-family.keyword" },
      "aggs": {
        "significant_malware types": {
          "significant_terms": { "field": "attributes.malware-type.keyword" }
        }
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        }
      ]
    }
  }
}
```

You get this response:

```json
{...
  ,"aggregations": {
    "malware-family": {
      "buckets": [
        {
          "doc_count": 1089,
          "key": "win",
          "significant_malware types": {
            "bg_count": 10854210,
            "buckets": [
              {
                "bg_count": 875,
                "doc_count": 719,
                "key": "trojan",
                "score": 5406.785721751882
              },
              {
                "bg_count": 181,
                "doc_count": 181,
                "key": "adware",
                "score": 1656.4466781514116
              },...
            ]
          }
        },...
      ]
    }
  }
}
```

For example, you may find that the "win" malware family is associated with a disproportionately large number of trojan attacks. Normally, trojans represent only a small percentage of malware attacks in the dataset, but for the "win" family, trojans make up a significant proportion of attacks. This could indicate that trojans are a common method of attack for this family. By identifying these patterns, you can gain insight into the behavior and characteristics of different malware families, and better understand how to defend against them.

### Sampler <a href="#sampler" id="sampler"></a>

If you’re aggregating over millions of documents, you can use a sampler aggregation to reduce its scope to a small sample of documents for a faster response. The sampler aggregation selects the samples by top-scoring documents.

The results are approximate but closely represent the distribution of the real data. The sampler aggregation significantly improves query performance, but the estimated responses are not entirely reliable.

The `shard_size` property tells OpenSearch how many documents (at most) to collect from each shard.

The following example limits the number of documents collected on each shard to 1,000 and then buckets the documents by a terms aggregation:

```json
{
  "aggs": {
    "sample": {
      "sampler": {
        "shard_size": 1000
      },
      "aggs": {
        "terms": {
          "terms": {
            "field": "attributes.malware-type.keyword"
          }
        }
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        }
      ]
    }
  }
}
```

We get a response like:

```json
{...,
"aggregations": {
    "sample": {
      "doc_count": 1000,
      "terms": {
        "buckets": [
          {
            "doc_count": 700,
            "key": "trojan"
          },
          {
            "doc_count": 181,
            "key": "adware"
          },
          {
            "doc_count": 67,
            "key": "worm"
          },...
        ],
         "doc_count_error_upper_bound": 0,
        "sum_other_doc_count": 4
      }
    }
  }
}
```

### Histogram and <a href="#histogram" id="histogram"></a>

The histogram aggregation is a powerful way to group documents based on a specified interval. It's particularly useful for visualizing the distribution of values within a range of documents. Although it doesn't directly generate a graph, it provides a JSON response that can be used to construct a customized visualization of the data.

Suppose you want to gain insights into the distribution of email reputation in the dataset accessed through the entites endpoint. One way to achieve this is by using the histogram aggregation. This aggregation can bucket the emails based on a specified interval, allowing you to easily visualize the distribution of reputation values within a given range of emails:

```json
{
  "aggs": {
      "number_of_bytes": {
      "histogram": {
        "field": "reputation",
        "interval": 1
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "email"
            }
          }
        }
      ]
    }
  }
}
```

We get this as response:

```json
{...,
 "aggregations": {
    "number_of_bytes": {
      "buckets": [
        {
          "doc_count": 232,
          "key": -3
        },
        {
          "doc_count": 119,
          "key": -2
        },
        {
          "doc_count": 274,
          "key": -1
        },
        {
          "doc_count": 160,
          "key": 0
        }
      ]
    }
  }
}
```

The date\_histogram aggregation uses date math to generate histograms for time-series data.

Date math supports the following time units: **y**: Years **M**: Months **w**: Weeks **d**: Days **h** or **H**: Hours **m**: Minutes **s**: Seconds

| Operator | Description   | Example                                   |
| -------- | ------------- | ----------------------------------------- |
| `+`      | Addition      | `+1M`: Add 1 month.                       |
| `-`      | Subtraction   | `-1y`: Subtract 1 year.                   |
| `/`      | Rounding down | `/h`: Round to the beginning of the hour. |

Suppose you want to obtain insights into the distribution of malware reports for each day in a week. One way to achieve this is by using time-based aggregations, such as the date histogram aggregation.

```json
{
  "aggs": {
    "malware_per_minute": {
      "date_histogram": {
        "field": "@timestamp",
        "interval": "day"
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-7d",
              "lte": "now"
            }
          }
        }
      ]
    }
  }
}
```

we get this as response:

```json
{...,
"aggregations": {
    "malware_per_minute": {
      "buckets": [
        {
          "doc_count": 10,
          "key": 1680134400000,
          "key_as_string": "2023-03-30T00:00:00.000Z"
        },
        {
          "doc_count": 187,
          "key": 1680220800000,
          "key_as_string": "2023-03-31T00:00:00.000Z"
        },...
      ]
    }
  }
}
```

When you use the date histogram aggregation, the timestamps are returned as as the `key` name of the bucket. Additionally, you can use the format parameter to convert the timestamps into formatted date strings, which are returned as the `key_as_string` values for each bucket.
