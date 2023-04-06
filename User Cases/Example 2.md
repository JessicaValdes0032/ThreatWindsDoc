<h4>Investigate a file Hash</h4>

A developer is tasked with developing a DNS filtering module for an enterprise network to block access to malicious websites. To achieve this, the developer needs to programmatically identify all domains with negative reputation and block access to them.

To accomplish this, the developer can use the Advanced Search Endpoint to search for all entities of domain type with negative reputation and only return the domain field.


**STEP 1** 

To retrieve domains with negative reputation, we can use the Advanced Search endpoint available in the Search API. The following query can be used:

```json
{
    "query": {
        "bool": {
            "filter": [
                {
                    "term": {
                        "type": {
                            "value": "domain"
                        }
                    }
                }
            ],
            "must": [
                {
                    "range": {
                        "reputation": {
                            "gte": -3,
                            "lte": -1
                        }
                    }
                }
            ]
        }
    }
}

```

The response would contain the domain entities, as shown below:

```json
{
    "pages": 1000,
    "items": 10000,
    "results": [
      {
        "@timestamp": "2023-04-04T21:14:20.646191184Z",
        "accuracy": 1,
        "attributes": {
          "domain": "maibuzhao.cn"
        },
        "id": "domain-104e5241cc5d93323cafc288f8e902047934e4e9b33df15e7ddcdc4103ddf3e6",
        "reputation": -2,
        "type": "domain"
      },
      {
        "@timestamp": "2023-04-04T21:14:20.599285563Z",
        "accuracy": 2,
        "attributes": {
          "domain": "sunglassesone.com"
        },
        "id": "domain-afa28112d9ccf125dc60a93a9e2b51c13789c1ed3c52dc29ae77ebbd25697db3",
        "reputation": -2,
        "type": "domain"
      },...
    ]
}
```

After retrieving the domains with negative reputation through the Advanced Search endpoint query, we can iterate over the resulting entities and extract the domain field from each entity.

Here is the final list of domains that were obtained:
```txt
departmentstores-onlineshopping.com
bellezane.info
pbmz.net
brokenbeside.net
rainingcats.net
inrgroups.com
gtc3595.com
tnjdkfi7wvqrc7tfa.com
tnbtvrirc.info
armanfze.com
gotnewedgein.info
...
```