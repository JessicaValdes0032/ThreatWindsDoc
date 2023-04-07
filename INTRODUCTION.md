---
description: >-
  Learn how to use ThreatWinds, the APIs that provide comprehensive information
  on Cybersecurity threats and their interrelationships.
---

# Welcome to ThreatWinds APIs Documentation

<figure><img src=".gitbook/assets/119631617.png" alt=""><figcaption><p>• Homepage • <a href="VERSIONHISTORY.md">Releases </a>• News</p></figcaption></figure>

{% code overflow="wrap" lineNumbers="true" %}
```bash
curl -X 'POST' \ 'https://intelligence.threatwinds.com/api/search/v1/entity' \ -H 'accept: application/json' \ -H 'Content-Type: application/json' \ -d '{ "kind": "object", "value": "testing" }'
```
{% endcode %}

#### Getting started

* [Introduction](INTRODUCTION.md#introduction)
* [Quickstart](QUICKSTART.md)
* [Version history](VERSIONHISTORY.md)

### Introduction <a href="#introduction" id="introduction"></a>

Welcome to the documentation for our API, the powerful tool that allows you to integrate our services with your own application seamlessly. With our API, you can unlock a world of possibilities, leveraging the power of our technology to enhance your own products and services.

This documentation is designed to guide you through every step of the integration process, from getting started with authentication to understanding the various endpoints and parameters available to you. We've worked hard to ensure that our API is easy to use, with straightforward and intuitive interfaces that will help you get up and running quickly.

Our API offers a range of features that can be tailored to your specific needs, from simple data retrieval to complex querying capabilities that allow you to extract valuable insights from our data. If you encounter any issues or have any questions, our support team is always available to help you out.

### Get involved

We believe that collaboration and community involvement are key to creating a safer online environment for everyone. That's why we're excited to offer our users the opportunity to contribute to our threat intelligence database using our ingest API.

By adding your own threat data, you can help make our system more accurate and robust, and contribute to the greater goal of creating a more secure online world.

We welcome all contributions, no matter how big or small. Whether you have information on a specific threat or a general trend, your contribution can help us build a more comprehensive and accurate picture of the cybersecurity landscape.

See more about [Ingestion API](INTRODUCTION.md#introduction)

\
Thank you for choosing our API, and we look forward to seeing the innovative ways in which you use our technology to enhance your own products and services.
