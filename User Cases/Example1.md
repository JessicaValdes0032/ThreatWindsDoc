<h4>Investigate a file Hash</h4>

A researcher needs to investigate a file hash. Specifically, they want to search for the file hash and discover if it is associated with any malware.

To accomplish this, the researcher can use the Advanced Search Endpoint to search for the md5 file hash and retrieve the associated md5 entity. Once they have the md5 entity, they can use the Association Endpoint to search for any associations between the file and any known malware.


_**md5**: "2ed56a78d957dcde2db76e830f5f0741"_

**STEP 1** 

To retrieve the md5 entity associated with the given hash, we can use the Advanced Search endpoint available in the Search API. Here is an example query:

```json
{
    "query": {
        "bool": {
            "must": [
                {
                    "term": {
                        "attributes.md5": {
                            "value": "2ed56a78d957dcde2db76e830f5f0741"
                        }
                    }
                }
            ]
        }
    }
}

```

The response would contain the md5 entity, as shown below:

```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-03-31T13:08:08.220244275Z",
      "accuracy": 1,
      "attributes": {
        "descriptor": "common-file",
        "md5": "2ed56a78d957dcde2db76e830f5f0741"
      },
      "id": "md5-2bef618643db4b0f1b8631b1567abf2262dc4161e94f671c5a9197d3b22a2c52",
      "reputation": -3,
      "type": "md5"
    }
  ]
}
```

To determine the reputation of a file, we can check the "reputation" attribute in the response. The scale for reputation ranges from -3 (extremely bad) to 3 (excellent). In this case, the file has a negative reputation with a value of -3, which is the worst possible rating on the scale.

**STEP 2** Now that we have retrieved the ID of the md5 file associated with the given hash, we can proceed to the next step, which is to use the Association endpoint to retrieve the malware associated with that file.

The response would contain the malware entity, as shown below:

```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-03-31T13:08:08.29677912Z",
      "accuracy": 1,
      "attributes": {
        "malware": "win exploit cve_2023_21823",
        "malware-family": "win",
        "malware-type": "exploit"
      },
      "id": "malware-efefabf74562def7d4fba5e384896ef59a08ba45b54c98fb98a3772f96a6d3cb",
      "reputation": -3,
      "type": "malware"
    }
  ]
}
```
The provided information is about a malware named "win exploit cve_2023_21823". An exploit is a type of malware that takes advantage of vulnerabilities in software or systems to execute malicious code or commands. In this case, the exploit targets a specific vulnerability identified as "cve_2023_21823". 

It is important to note that exploits are often used as a means to deliver other types of malware such as viruses, worms, or trojans. The reputation score of this specific malware is -3, which indicates a high risk for the user. It is recommended to take immediate action to remove the malware from the affected system.

