<h2>Query and Agregation</h2>

Queries are a critical component of any search system, and a Query DSL provides a flexible and powerful way to construct them. A Query DSL is a JSON-based language that enables you to specify various query clauses to create complex queries that consider multiple criteria, such as IP ranges, regular expressions, or even fuzzy matching. Each query clause performs a specific function and can be combined with other clauses to build more sophisticated queries.

Query DSL and aggregation capabilities provide a robust and flexible platform for searching and analyzing your data. Whether you are building an antivirus, security platform, or analytics application, Query DSL and aggregation features can help you build sophisticated and powerful systems.



<nav>
<h4>Table of Contents</h4>
    <ol>
        <li><a href="#queryContext">Query Context</li>
        <ol>
          <li><a href="#boolean">Boolean Query</li>
          <ol>
          <li><a href="#filter">Filter Context</li>
          <li><a href="#must">Must Match</li>
          <li><a href="#mustNot">Must Not Match</li>
          <li><a href="#should">Should Match</li>
          </ol>
          <li><a href="#termLevelVsFullText">Term-Level vs Full-Text Query</li>
        <li><a href="#termLevel">Term-Level Queries</li>
        <ol>
          <li><a href="#term">Term Query</li>
          <li><a href="#term">Terms Query</li>
          <li><a href="#ids">Ids Query</li>
          <li><a href="#range">Range Query</li>
          <li><a href="#prefix">Prefix Query</li>
          <li><a href="#exists">Exists Query</li>
          <li><a href="#fuzzi">Fuzzi Query</li>
          <li><a href="#wildcard">Wildcard Query</li>
          <li><a href="#regexp">Regexp Query</li>
          </ol>
        <li><a href="#fullText">Full-Text Queries</li>
        <ol>
        <li><a href="#match">Match Query</li>
        <li><a href="#multiMatch">Multi Match Query</li>
        <li><a href="#matchBooleanPrefix">Match Bool Prefix Query</li>
        <li><a href="#matchPhrase">Match Phrase Query</li>
        <li><a href="#matchPhrasePrefix">Match Phrase Prefix</li>
        <li><a href="#queryString">Query String Query</li>
        <li><a href="#simpleQueryString">Simple Query String</li>  
        <li><a href="#advancedOptions">Advanced Options Section</a></li>
        </ol>
    </ol>
</nav>


 <h3 id="queryContext">Query</h3>

In the query context, a query clause answers the question “How well does this document match this query clause?” Besides deciding whether or not the document matches.

When running a query clause in a query context, each matching document contains a relevance score that is stored in the **accuracy** field. This score reflects the degree of relevance of the document to the query and can be used, for example, to sort the results in descending order.


<h4 id="boolean">Boolean Query</h4>
The bool query type allows you to perform a Boolean query, which is a type of query that combines multiple conditions using logical operators like "and", "or", and "not". The bool query type is built using one or more boolean clauses, each of which can have a specific occurrence type such as <a href="#must">must</a>, <a href="#should">should</a>, <a href="#mustNot">must_not</a> or <a href="#filter">filter</a>.

As a compound query type, the bool query allows you to construct complex queries by combining several simple queries. For example, you could use a bool query to search for documents that match a specific term, but also include documents that have a similar term or fall within a certain range of values.


<h5 id="filter">Filter Context</h5>
Before applying queries, it is often beneficial to first reduce your dataset using logical operators. This can be done using a filter clause, which contains a query that is evaluated as a boolean yes or no. Documents that meet the criteria of the query are included in the results, while those that do not are excluded.

Filter queries are particularly useful for narrowing down results based on specific criteria, such as exact matches, ranges, dates, or numerical values. These queries are also cached for faster retrieval, making them a great option for filtering large datasets.
<br>

<h5 id="must">Must</h5>

The "must" operator functions as a logical **AND** operator used to combine multiple query clauses. It is used to narrow down search results by specifying that all the conditions must match for a document to be included in the search results. The relevance score of each matching document is also calculated based on how well it matches all of the query clauses.


<h5 id="mustNot">Must Not</h5>

The "must not" operator is a negation operator that functions as a logical **NOT** operator. It allows you to exclude documents from search results. This operator is used within a Boolean query as a clause to specify that the specified query or queries should not match any of the documents. The "must not" clause is executed in the filter context, which means that the query is not used to calculate the accuracy score of the documents. Instead, it is used to filter out the matching documents from the result set.


<h5 id="should">Should</h5>

In Elasticsearch, the "should" operator is used as a logical **OR** operator. It allows you to specify multiple queries within a Boolean query, and the results must match at least one of these queries. Optionally, the results can also match more than one query.

Each matching "should" clause contributes to the relevance score of the documents. This means that if a document matches more than one "should" clause, its relevance score will be higher than if it matches only one.

You can set the minimum number of "should" clauses that must match for a document to be included in the search results using the "minimum_should_match" parameter. This allows you to control the level of strictness in your search results. For example, if you set the parameter to 2, then at least two of the "should" clauses must match for a document to be included in the search results.


<h5 id="minimumShouldMatch">Minimum Should Match</h5>

The "minimum_should_match" parameter in Elasticsearch allows you to specify the minimum number or percentage of "should" clauses that a document must match in order to be included in the search results. This parameter is used with the "should" query clause and is optional.

By default, if the bool query contains only "should" clauses without any "must" or "filter" clauses, the minimum_should_match value is set to 1. However, if there are "must" or "filter" clauses present in the bool query, the default value is 0.

<table>
<thead>
<tr>
<th>Type</th>
<th>Example</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left" valign="top"><p>Integer</p></td>
<td align="left" valign="top"><p><code>3</code></p></td>
<td align="left" valign="top"><p>Indicates a fixed value regardless of the number of optional clauses.</p></td>
</tr>
<tr>
<td align="left" valign="top"><p>Negative integer</p></td>
<td align="left" valign="top"><p><code>-2</code></p></td>
<td align="left" valign="top"><p>Indicates that the total number of optional clauses, minus this number should be mandatory.</p></td>
</tr>
<tr>
<td align="left" valign="top"><p>Percentage</p></td>
<td align="left" valign="top"><p><code>75%</code></p></td>
<td align="left" valign="top"><p>Indicates that this percent of the total number of optional clauses are necessary. The number computed from the percentage is rounded down and used as the minimum.</p></td>
</tr>
<tr>
<td align="left" valign="top"><p>Negative percentage</p></td>
<td align="left" valign="top"><p><code>-25%</code></p></td>
<td align="left" valign="top"><p>Indicates that this percent of the total number of optional clauses can be missing. The number computed from the percentage is rounded down, before being subtracted from the total to determine the minimum.</p></td>
</tr>
<tr>
<td align="left" valign="top"><p>Combination</p></td>
<td align="left" valign="top"><p><code>3&lt;90%</code></p></td>
<td align="left" valign="top"><p>A positive integer, followed by the less-than symbol, followed by any of the previously mentioned specifiers is a conditional specification. It indicates that if the number of optional clauses is equal to (or less than) the integer, they are all required, but if it’s greater than the integer, the specification applies. In this example: if there are 1 to 3 clauses they are all required, but for 4 or more clauses only 90% are required.</p></td>
</tr>
<tr>
<td align="left" valign="top"><p>Multiple combinations</p></td>
<td align="left" valign="top"><p><code>2&lt;-25% 9&lt;-3</code></p></td>
<td align="left" valign="top"><p>Multiple conditional
specifications can be separated by spaces, each one only being valid for numbers greater than the one before it. In this example: if there are 1 or 2 clauses both are required, if there are 3-9 clauses all but 25% are required, and if there are more than 9 clauses, all but three are required.</p></td>
</tr>
</tbody>
</table>

<h5>Example</h5>
Let's say you want to search for all entities that meet certain criteria. Specifically, you want to find entities that are of type "malware", belong to the "pdf" malware family, are either of type "malware" or "dropper", and have a negative reputation.

We are going to use the <a>Advanced Entity Search</a>.

Criteria:

Type: "malware"
Malware family: "pdf"
Malware type: "malware" or "dropper"
Reputation: Negative
The query is going to use a "bool" query with four clauses:

"filter": Used to filter out entities that aren't of type "malware".
"must": Used to match entities that belong to the "pdf" malware family.
"must_not": Used to filter out entities with a reputation score greater than or equal to 0 and less than or equal to 3.
"should": Used to match entities that are either of type "malware" or "dropper".
To make this query, you'll need to use a "bool" query, which allows you to combine multiple query clauses together.

```json
{
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
            ],
            "must": [
                {
                    "term": {
                        "attributes.malware-family": {
                            "value": "pdf"
                        }
                    }
                }
            ],
            "must_not": [
                {
                    "range": {
                        "reputation": {
                            "gte": 0,
                            "lte": 3
                        }
                    }
                }
            ],
            "should": [
                {
                    "term": {
                        "attributes.malware-type": {
                            "value": "dropper"
                        }
                    }
                },
                {
                    "term": {
                        "attributes.malware-type": {
                            "value": "malware"
                        }
                    }
                }
            ]
        }
    }
}
```

we get a response like:
```json
{
  ...
  "results": [
    {
      "@timestamp": "2023-03-31T18:00:55.689301889Z",
      "accuracy": 3,
      "attributes": {
        "malware": "pdf dropper agent",
        "malware-family": "pdf",
        "malware-type": "dropper"
      },
      "id": "malware-20eae8ae8ec23315a7d9f07c0cbcd3651657b8604ef02e9dfcbfd6304cb824b8",
      "reputation": -3,
      "type": "malware"
    },
    ...]
}
```
<h4 id=termLevelVsFullText>Term-level Queries vs FullText Query</h4>

Our APIs provide two types of queries for searching text: term-level and full-text queries. Term-level queries are ideal for searching structured data such as numbers, dates, or tags, where you want to find documents that match exact values. On the other hand, full-text queries are designed for full-text search and are best suited for analyzing and searching unstructured text data.

The key difference between the two query types is that term-level queries search for an exact specified term in a document, while full-text queries analyze the query string and return documents based on relevance scores. In other words, term-level queries simply match documents that contain the exact search term, whereas full-text queries match documents based on their textual content, considering factors such as word frequency, proximity, and relevance.

To help you understand the differences between these query types, the following table summarizes their key characteristics:

<table>
  <thead>
    <tr>
      <th style="text-align: left">&nbsp;</th>
      <th style="text-align: left">Term-level queries</th>
      <th style="text-align: left">Full-text queries</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left"><strong>Description</strong></td>
      <td style="text-align: left">Term-level queries aim to find documents based on precise values in structured data. They match exact terms stored in a field and search an index for documents that contain an exact search term. Term-level queries answer which documents match a query and are typically used for fields mapped as keyword only.</td>
      <td style="text-align: left">Full-text queries aim to search for text data using search terms that are analyzed. They are used for full-text search and analyze the query string using the same analyzer that was used for the specific document field at the time it was indexed. Full-text queries answer how well the documents match a query and are typically used for searching analyzed text fields.</td>
    </tr>
    <tr>
      <td style="text-align: left"><strong>Analyzer</strong></td>
      <td style="text-align: left">The search term isn’t analyzed, which means that the term query searches for your search term as it is. Term-level queries are suitable for matching exact values such as numbers, dates, or tags.</td>
      <td style="text-align: left">The search term is analyzed by the same analyzer that was used for the specific document field at the time it was indexed. This means that your search term goes through the same analysis process as the document’s field. Full-text queries are suitable for matching text fields and take into account factors like casing and stemming variants.</td>
    </tr>
    <tr>
      <td style="text-align: left"><strong>Relevance</strong></td>
      <td style="text-align: left">Term-level queries simply return documents that match without sorting them based on the relevance score. They still calculate the relevance score, but this score is the same for all the documents that are returned.</td>
      <td style="text-align: left">Full-text queries calculate a relevance score for each match and sort the results by decreasing order of relevance.</td>
    </tr>
    <tr>
      <td style="text-align: left"><strong>Use Case</strong></td>
      <td style="text-align: left">Use term-level queries when you want to match exact values such as numbers, dates, or tags and don’t need the matches to be sorted by relevance.</td>
      <td style="text-align: left">Use full-text queries to match text fields and sort by relevance after taking into account factors like casing and stemming variants.</td>
    </tr>
  </tbody>
</table>
<h4 id="termLevel">Term Level Query</h4>
Term-level queries search for elements that contain an exact search term. 

Types of Term Level Queries: 

<table>
  <thead>
    <tr>
      <th style="text-align: left">Query type</th>
      <th style="text-align: left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left"><a href="#term">term</a></td>
      <td style="text-align: left">Searches for documents with an exact term in a specific field.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#terms">terms</a></td>
      <td style="text-align: left">Searches for documents with one or more terms in a specific field.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#terms-set">terms_set</a></td>
      <td style="text-align: left">Searches for documents that match a minimum number of terms in a specific field.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#ids">ids</a></td>
      <td style="text-align: left">Searches for documents by document ID.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#range">range</a></td>
      <td style="text-align: left">Searches for documents with field values in a specific range.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#prefix">prefix</a></td>
      <td style="text-align: left">Searches for documents with terms that begin with a specific prefix.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#exists">exists</a></td>
      <td style="text-align: left">Searches for documents with any indexed value in a specific field.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#fuzzy">fuzzy</a></td>
      <td style="text-align: left">Searches for documents with terms that are similar to the search term within the maximum allowed Levenshtein distance. The Levenshtein distance measures the number of one-character changes needed to change one term to another term.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#wildcard">wildcard</a></td>
      <td style="text-align: left">Searches for documents with terms that match a wildcard pattern.</td>
    </tr>
    <tr>
      <td style="text-align: left"><a href="#regexp">regexp</a></td>
      <td style="text-align: left">Searches for documents with terms that match a regular expression.</td>
    </tr>
  </tbody>
</table>

<h4 id=term>Term Query</h4>

The term query allows you to search for documents that contain an exact term in a specific field.

To return a document, one or more terms must exactly match a field value, including whitespace and capitalization. This query is useful when you need to match exact values and don't want to allow for any variations.

The terms query is the same as the term query, except you can search for multiple values. A document will match if it contains at least one of the terms.

Example:

For example, we need to construct an Elasticsearch query to retrieve all registered malware that belongs to the 'txt' and 'pdf' malware families.

```json
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        },
        {
          "terms": {
            "attributes.malware-family": [
              "pdf",
              "txt"
            ]
          }
        }
      ]
    }
  }
}
```

In this example, we're using the bool query to combine two different queries: the term query and the terms query. The term query searches for documents where the "type" field exactly matches the value "malware". The terms query searches for documents where the "malware-family" field contains any of the values "pdf" or "txt".

<h4 id="ids">IDs Query</h4>
The IDs query in Elasticsearch allows you to search for one or more documents based on their IDs. This query works by searching for document IDs stored in the "id" field.

Example:

```json
{
  "query": {
    "ids": {
      "values": [
        "md5-386b3f6e7b3e1830fdc34d6fa85c5b243942ae6110f9a0cd43d1a330ca56e8ad",
        "email-address-8ea1b632284536493a72c3745b77ae5f19a1e096026b88f0cfcd52475711c7ad"
      ]
    }
  }
}
```

<h4 id="range">Range Query</h4>

To search for a range of values in a specific field, you can use the range query. This query returns elements that contain terms within a provided range, and you can further specify date formats or relation operators such as "contains" or "within". The range query offers a variety of optional parameters.


| Field data type | Description                                                                                                      |
|----------------|------------------------------------------------------------------------------------------------------------------|
| integer_range  | A range of integer values.                                                                                        |
| long_range     | A range of long values.                                                                                           |
| double_range   | A range of double values.                                                                                         |
| float_range    | A range of float values.                                                                                          |
| ip_range       | A range of IP addresses in IPv4 or IPv6 format. Start and end IP addresses may be in different formats.          |
| date_range     | A range of date values. Start and end dates may be in different formats. Internally, all dates are stored as unsigned 64-bit integers representing milliseconds since the epoch.                                              |

Parameter    | Description
-------------|-------------
| gte       | Greater than or equal to.    |
| gt        | Greater than.                |
| lte       | Less than or equal to.       |
| lt        | Less than.                   |
format       | A format for dates in this query. Default is the field’s mapped format.
relation     | Provides a relation between the query’s date range and the document’s date range. There are three types of relations that you can specify:<br>-**intersects** matches documents for which there are dates that belong to both the query’s date range and document’s date range. This is the default.<br>- **contains** matches documents for which the query’s date range is a subset of the document’s date range.<br>- **within** matches documents for which the document’s date range is a subset of the query’s date range.
|time_zone|  used to convert date values in the query to UTC.|
|boost | Floating point number used to decrease or increase the relevance scores of a query. Defaults to 1.0. You can use the boost parameter to adjust relevance scores for searches containing two or more queries. Boost values are relative to the default value of 1.0. A boost value between 0 and 1.0 decreases the relevance score. A value greater than 1.0 increases the relevance score.

For Example:

To retrieve all IP addresses that have a negative reputation and were reported in the last 24 hours, you can use the range query to search for entities that contain a timestamp field and a reputation field within their respective specified ranges. Additionally, you can use the term query to filter documents by the "type" field which has the value "ip". It's important to note that the range query needs to take into account the timezone in which the data was stored. Here's an example:

```json
{
    "query": {
        "bool": {
            "must": [
                {
                    "term": {
                        "type": {
                            "value": "ip"
                        }
                    }
                },
                {
                    "range": {
                        "@timestamp": {
                            "gte": "now-24h",
                            "lt": "now",
                            "time_zone": "-04:00"
                        }
                    }
                },
                {
                    "range": {
                        "reputation": {
                            "gte": "-3",
                            "lt": "0"
                        }
                    }
                }
            ]
        }
    }
}
```

Date math supports the following time units:

**y**: Years
**M**: Months
**w**: Weeks
**d**: Days
**h** or **H**: Hours
**m**: Minutes
**s**: Seconds

<h5>Example expressions</h5>
The following example expressions illustrate using date math:

**now+1M**: The current date and time in milliseconds since the epoch, plus 1 month.
**2022-05-18||/M**: 05/18/2022, rounded to the beginning of the month. Resolves to 2022-05-01.
**2022-05-18T15:23||/h**: 15:23 on 05/18/2022, rounded to the beginning of the hour. Resolves to 2022-05-18T15.
**2022-05-18T15:23:17.789||+2M-1d/d**: 15:23:17.789 on 05/18/2022 plus 2 months minus 1 day, rounded to the beginning of the day. Resolves to 2022-07-17.

<h4 id="prefix">Prefix Query</h4>

Use the prefix query to search for terms that begin with a specific prefix.

The optional parameter "case_insensitive" allows for ASCII case-insensitive matching of the value with indexed field values when set to true, with false being the default which depends on the case sensitivity of the underlying field's mapping.

Example:
```json
{
  "query": {
    "prefix": {
      "attributes.malware": "pdf"
    }
  }
}
```

<h4 id="exists">Exists</h4>
Use the exists query to search for documents that contain a specific field.

Although a field in JSON is considered non-existent when its value is null or [], it is considered to exist if it contains empty strings like "" or "-", arrays with a combination of null and other values like [null, "foo"], or a custom null-value specified in field mapping.

Example:

```json
{
  "query": {
    "exists": {
      "field": "attributes.country"
    }
  }
}
```

<h4 id="fuzzi">Fuzzi Query</h4>
A fuzzy query searches for documents with terms that are similar to the search term within a specified Levenshtein distance, which measures the number of one-character changes needed to change one term to another. These changes include replacements, insertions, deletions, and transpositions, and a list of all possible expansions of the search term within the specified distance is created. The max_expansions field specifies the maximum number of such expansions, and documents that match any of the expansions are returned.

Examples: 
Replacements: IPV4 to IPV6
Insertions: ip to ips
Deletions: "newiphone.pdf" to "newiphone"
Transpositions:  "netstat" to "ntestat"

For Example:

```json
{
  "query": {
    "fuzzy": {
      "type": {
        "value": "maldare"
      }
    }
  }
}
```


<h4 id="wildcard">Wildcard</h4>
Wildcard queries are a type of query that can be used to search for terms that match a specific pattern.

The wildcard feature in Elasticsearch includes two special characters: ``*`` and ``?``. The ``*`` character specifies all valid values, while the ``?`` character specifies a single valid value. 

It's important to note that wildcard queries can be slow since they need to iterate over a large number of terms. Therefore, it's best to avoid placing wildcard characters at the beginning of a query because it could be an expensive operation in terms of both resources and time.

When "case_insensitive" is set to true, it enables case-insensitive matching of patterns with indexed field values. By default, this feature is set to false, meaning the matching's case sensitivity depends on the underlying field's mapping.


For example:
```
{
  "query": {
    "wildcard": {
      "attributes.malware.keyword": {
        "value": "pdf*agent"
      }
    }
  }
}
```

For example, we can use a wildcard query to find all malware that begins with "pdf" and ends with "agent".


<h4 id="regexp">RegExp</h4>

The regexp query is used in Elasticsearch to search for terms that match a regular expression. Regular expressions can be applied to the terms in a field, but not to the entire field. The regular expression syntax used in Elasticsearch is based on Lucene, which may differ from other standard implementations. Therefore, it is important to test the regular expression thoroughly to ensure the expected results. Lucene documentation can be consulted for further details.

It's important to note that regexp queries can be expensive operations, and may require the "search.allow_expensive_queries" setting to be set to true. Before making frequent regexp queries, it is recommended to test their retriever time and consider alternative queries for achieving similar results.

This example retrieves all Gmail email addresses that have a negative reputation:

```json
{
    'query': {
        "bool": {
            "must": [
                {"regexp": {
                        "[\w.%+-]+@gmail+\.[A-Za-z]{2,}"
                    }
                },
                {
                    "range": {
                        "reputation": {
                            "gte": "-3",
                            "lt": "0"
                        }
                    }
                }
            ]
        }
    }
}
```

<h4 id=fullText>Full-Text Query</h4>

<h4 id=match>Match Query</h4>
The match query in ElasticSearch is used to perform a full-text search on a specific field of a document. It analyzes the search string and retrieves documents that match any of its terms. By using Boolean query operators, you can combine multiple match queries to create complex searches.

The match query also provides options for fuzzy matching, which can help to retrieve relevant documents even if the search terms are misspelled or have slight variations. This makes it a powerful tool for text-based searches in ElasticSearch.

The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "match": {
      "type": {
        "query": "malware",
        "fuzziness": "AUTO",
        "fuzzy_transpositions": true,
        "operator":  "or",
        "minimum_should_match": 1,
        "analyzer": "standard",
        "zero_terms_query": "none",
        "lenient": false,
        "prefix_length": 0,
        "max_expansions": 50,
        "boost": 1
      }
    }
  }
}
```
<h4 id="multiMatch">Multi Match Query</h4>
The multi_match query type allows you to search multiple fields simultaneously in a manner similar to the match operation. You can use the ^ symbol to "boost" certain fields, giving them greater weight in the search results. Boosts are multipliers that allow you to weigh matches in one field more heavily than matches in other fields.<br><br>

The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "multi_match": {
      "query": "malware",
      "fields": ["malware-type^4", "type"],
      "type": "most_fields",
      "operator": "and",
      "minimum_should_match": 3,
      "tie_breaker": 0.0,
      "analyzer": "standard",
      "boost": 1,
      "fuzziness": "AUTO",
      "fuzzy_transpositions": true,
      "lenient": false,
      "prefix_length": 0,
      "max_expansions": 50,
      "auto_generate_synonyms_phrase_query": true,
      "zero_terms_query": "none"
    }
  }
}
```

<h4 id="matchBooleanPrefix">Match Boolean Prefix Query</h4>

When using the match_bool_prefix query, the search string is analyzed and converted into a bool query, using all terms except the last one as whole words for matching. The last term is treated as a prefix, and the query returns documents that contain either the whole-word terms or terms that begin with the prefix term, in any order.

The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "match_bool_prefix": {
      "attributes.malware": {
        "query": "win malware",
        "fuzziness": "AUTO",
        "fuzzy_transpositions": true,
        "max_expansions": 50,
        "prefix_length": 0,
        "operator":  "or",
        "minimum_should_match": 2,
        "analyzer": "standard"
      }
    }
  }
}
```

<h4 id="matchPhrase">Match Phrase</h4>
In ElasticSearch, the match_phrase query is used to retrieve documents that contain an exact phrase in a specified order. It analyzes the text and creates a phrase query, which matches terms up to a configurable slop value (by default, slop is set to 0) in any order. If the terms are transposed, the slop value is automatically set to 2.

The match_phrase query provides flexibility to phrase matching by allowing you to set the slop parameter, which determines the maximum number of positions between each term in the matched phrase. By increasing the slop value, you can retrieve documents that contain the phrase even if the terms are not in the exact order specified in the query.

The match_phrase query is useful for finding exact phrases in text-based searches, such as when searching for names or specific quotes.

The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "match_phrase": {
      "attributes.malware": {
        "query": "win malware agent",
        "slop": 3,
        "analyzer": "standard",
        "zero_terms_query": "none"
      }
    }
  }
}
```

<h4 id="matchPhrasePrefix">Match phrase prefix</h4>

The match_phrase_prefix query in ElasticSearch allows you to search for a specified phrase in a specific order. It returns documents that contain phrases beginning with the same terms as provided, treating the last term as a prefix. The query creates a prefix query out of the last term in the query string and matches any words that begin with that term. The match_phrase_prefix query is useful for searching for incomplete phrases, and it's faster than the match_phrase query.

For example, a search using the match_phrase_prefix query could return entities that contain phrases beginning with "malware a" in the message field. This search would match a message value of "win malware agent" or "html malware agent".


The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "match_phrase_prefix": {
      "title": {
        "query": "malware a",
        "analyzer": "standard",
        "max_expansions": 50,
        "slop": 3
      }
    }
  }
}
```

<h4 id=queryString>Query String Query</h4>

The query_string query provides a powerful tool for searching across multiple fields with complex queries. It uses a parser to break down the provided query string into individual search terms and applies the appropriate analysis to each term.

The parser supports a wide range of operators, such as AND, OR, NOT, and parentheses, allowing for complex boolean queries. It also supports wildcard characters, such as * and ?, fuzziness, Proximity searches,  regular expressions and ranges.

In addition, the query_string query allows for the specification of search fields using the syntax field:query, allowing for targeted searches across specific fields.

It is important to note that the query_string query is strict and any syntax errors in the query string will result in an error response. It is also important to thoroughly test queries and analyze their impact on cluster performance before using them frequently in production.

The query string "mini-language" is a syntax used by both the Query string and the q query string parameter in the search API. The query string is analyzed and parsed into individual terms and operators. A term can be a single word or a phrase surrounded by double quotes. The available operators provide customization options for the search.

In the searches, you can use the query syntax to search for specific fields in a document:

Search for all entities where the type field contains "malware"

**type:malware**

Search for all entities where the malware-family field contains either "email" or "html"

**malware-family:(email OR html)**

Search for all entities where the malware field contains the exact phrase "multios malware agent"

**attributes.malware:"multios malware agent"**


Search for all entities where any of the fields "malware-family", "malware" or "malware-type" contains the word "ransomware" (note how we need to escape the * with a backslash):

**attributes.\*:ransomware**

Search for all entities where the field "description" has a non-null value:

**exists:description**


The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "query_string": {
      "query": "(html OR multios) AND malware agent",
      "default_field": "title",
      "type": "best_fields",
      "fuzziness": "AUTO",
      "fuzzy_transpositions": true,
      "fuzzy_max_expansions": 50,
      "fuzzy_prefix_length": 0,
      "minimum_should_match": 1,
      "default_operator": "or",
      "analyzer": "standard",
      "lenient": false,
      "boost": 1,
      "allow_leading_wildcard": true,
      "enable_position_increments": true,
      "phrase_slop": 3,
      "max_determinized_states": 10000,
      "time_zone": "-08:00",
      "quote_field_suffix": "",
      "quote_analyzer": "standard",
      "analyze_wildcard": false,
      "auto_generate_synonyms_phrase_query": true
    }
  }
}
```

<h4 id="simpleQueryString">Simple Query String</h4>

The simple_query_string type is a way to specify multiple search arguments directly in the query string using regular expressions. This type of search will ignore any invalid portions of the string, making it a more forgiving and flexible option for complex queries. 

| Special character | Behavior |
| --- | --- |
| `+` | Acts as the `and` operator. |
| `|` | Acts as the `or` operator. |
| `*` | Acts as a wildcard. |
| `""` | Wraps several terms into a phrase. |
| `()` | Wraps a clause for precedence. |
| `~n` | When used after a term (for example, `wnid~3`), sets `fuzziness`. When used after a phrase, sets `slop`. [Advanced filter options](#advanced-filter-options). |
| `-` | Negates the term. |

The query accepts the following options. For descriptions of each, see <a href="#advancedOptions">Advanced Options Section</a>.

```json
{
  "query": {
    "simple_query_string": {
      "query": "malware agent -html",
      "fields": ["title"],
      "flags": "ALL",
      "fuzzy_transpositions": true,
      "fuzzy_max_expansions": 50,
      "fuzzy_prefix_length": 0,
      "minimum_should_match": 1,
      "default_operator": "or",
      "analyzer": "standard",
      "lenient": false,
      "quote_field_suffix": "",
      "analyze_wildcard": false,
      "auto_generate_synonyms_phrase_query": true
    }
  }
}
```


<h4 id=advancedOptions>Advanced Options Section<h4>

<h5>Wilcard Option</h5>

| Option | Valid values | Description |
| --- | --- | --- |
| `allow_leading_wildcard` | Boolean | Whether `*` and `?` are allowed as the first character of a search term. The default is `true`. |
| `analyze_wildcard` | Boolean | Whether OpenSearch should attempt to analyze wildcard terms. Some analyzers do a poor job at this task, so the default is `false`. |


<h5>Fuzzy query options</h5>

| Option                      | Valid values                                  | Description                                                                                                                                                          |
|-----------------------------|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `fuzziness`                 | `AUTO`, `0`, or a positive integer            | The number of character edits (insert, delete, substitute) that it takes to change one word to another when determining whether a term matched a value. <br><br> For example, the distance between `ipv4` and `ipv6` is 1. The default, `AUTO`, chooses a value based on the length of each term and is a good choice for most use cases. |
| `fuzzy_transpositions`      | Boolean                                       | Setting `fuzzy_transpositions` to true (default) adds swaps of adjacent characters to the insert, delete, and substitute operations of the `fuzziness` option. <br><br>For example, the distance between `malware` and `mawlare` is 1 if `fuzzy_transpositions` is true (swap “l” and “w”) and 2 if it is false (delete “w”, insert “w”). <br><br> If `fuzzy_transpositions` is false, `remalware` and `mawlare` have the same distance (2) from `malware`, despite the more human-centric opinion that `mawlare` is an obvious typo.<br><br> The default is a good choice for most use cases.                                                                                                                    |
| `fuzzy_max_expansions`      | Positive integer                              | Fuzzy queries “expand to” a number of matching terms that are within the distance specified in `fuzziness`. Then OpenSearch tries to match those terms against its indexes. |


<h5>Synonyms in a multiple terms search</h5>

You can also use synonyms with the terms query type to search for multiple terms. Use the `auto_generate_synonyms_phrase_query` Boolean field. By default it is set to true. It automatically generates phrase queries for multiple term synonyms. For example, if you have the synonym "ma, malware agent" and search for ma,” OpenSearch searches for ma OR "malware agent" when the option is true or ma OR (malware AND agent) when the option is false.

To learn more about the multiple terms query type, see Terms. For more reference information about phrase queries, see the <a href="https://lucene.apache.org/core/8_9_0/core/org/apache/lucene/search/PhraseQuery.html">Lucene documentation</a>.


<h5>Other advanced options</h5>

**boost** (_Floating-point_) Boosts the clause by the given multiplier. Useful for weighing clauses in compound queries. The default is 1.0.

**enable_position_increments** (_boolean_) When true, result queries are aware of position increments. This setting is useful when the removal of stop words leaves an unwanted “gap” between terms. The default is true.

**fields** (_string array_) The list of fields to search. If unspecified, defaults to the index.query.default_field setting, which defaults to ["*"].

**flags** (_string_): A |-delimited string of flags to enable (e.g., AND|OR|NOT). The default is ALL. You can explicitly set the value for default_field. 


**lenient**(_boolean_) Setting lenient to true lets you ignore data type mismatches between the query and the document field. For example, a query string of “8.2” could match a field of type float. The default is false.

**low_freq_operator**(_and, or_) The operator for low-frequency terms. The default is or. See also operator in this table.

**max_determinized_states** (_Positive Integer_) The maximum number of “states” (a measure of complexity) that Lucene can create for query strings that contain regular expressions (e.g. "query": "/wind.+?/"). Larger numbers allow for queries that use more memory. The default is 10,000.

**max_expansions** (_Positive Integer_) max_expansions specifies the maximum number of terms to which the query can expand. The default is 50.

**minimum_should_match** (_Positive or negative integer, positive or negative percentage, combination_) If the query string contains multiple search terms and you used the or operator, the number of terms that need to match for the document to be considered a match. For example, if minimum_should_match is 2, “wind often rising” does not match “The Wind Rises.” If minimum_should_match is 1, it matches.

**operator** (_or, and_) If the query string contains multiple search terms, whether all terms need to match (and) or only one term needs to match (or) for a document to be considered a match.

**phrase_slop** (_0 (default) or a positive integer_) See slop.

**prefix_length** (_0 (default) or a positive integer_) The number of leading characters that are not considered in fuzziness.

**quote_field_suffix** (_String_) This option lets you search different fields depending on whether terms are wrapped in quotes. For example, if quote_field_suffix is ".malware" and you search for "pdf" (in quotes) in the attributes field, OpenSearch searches the attributes.malware field. This second field might use a different type (e.g. keyword rather than text) or a different analyzer. The default is null.

**rewrite** (_constant_score, scoring_boolean, constant_score_boolean, top_terms_N, top_terms_boost_N, top_terms_blended_freqs_N_) Determines how it rewrites and scores multi-term queries. The default is constant_score.

**slop** (_	0 (default) or a positive integer_) Controls the degree to which words in a query can be misordered and still be considered a match. From the <a href="https://lucene.apache.org/core/8_9_0/core/org/apache/lucene/search/PhraseQuery.html#getSlop--">Lucene documentation</a>: “The number of other words permitted between words in query phrase. For example, to switch the order of two words requires two moves (the first move places the words atop one another), so to permit re-orderings of phrases, the slop must be at least two. A value of zero requires an exact match.”

**tie_breaker**(_0.0 (default) to 1.0_) Changes the way of scores searches. For example, a type of best_fields typically uses the highest score from any one field. If you specify a tie_breaker value between 0.0 and 1.0, the score changes to highest score + tie_breaker * score for all other matching fields. If you specify a value of 1.0, OpenSearch adds together the scores for all matching fields (effectively defeating the purpose of best_fields).

**time_zone** (_UTC offset hours_) Specifies the number of hours to offset the desired time zone from UTC. You need to indicate the time zone offset number if the query string contains a date range. For example, set time_zone": "-08:00" for a query with a date range such as "query": "wind rises release_date[2012-01-01 TO 2014-01-01]"). The default time zone format used to specify number of offset hours is UTC.

**type** (_best_fields, most_fields, cross_fields, phrase, phrase_prefix_) Determines how it executes the query and scores the results. The default is best_fields.

**zero_terms_query** (_none, all_) 	If the analyzer removes all terms from a query string, whether to match no documents (default) or all documents. For example, the stop analyzer removes all terms from the string “an but this.”