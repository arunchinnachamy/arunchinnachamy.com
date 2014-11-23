---
layout: post
title: "How to configure SOLR for Geo-Spatial Search"
modified:
excerpts: This post explains how to configure SOLR for Geo-Spatial or Location Search
tags: [SOLR, Search]
image:
  feature:
date: 2014-11-23T13:27:50+05:30
comments: true
---
Last week, We at MySmartPrice worked on public beta for displaying offline brick & mortar store price of selected mobiles for all Hyderabad users. We rely heavily on caching for our page delivery and this feature is going to be real time data based on the user location. I ended up trying SOLR for performing the geo-spatial location search based on latitude and longitude for getting the stores close to the user location. More details about the integration will be posted shortly in [MySmartPrice Tech Blog](http://tech.mysmartprice.com).

This post focuses on configuring SOLR for location based search and how to perform queries in SOLR.

## Schema Changes
In order to add location data in documents, we need to update the schema file. We need to add a new field type.

    <fieldType name="location" class="solr.LatLonType" subFieldSuffix="_coordinate"/>

If you are using the default _schema.xml_, the definition must be already present. Now, we need to define field for holding the location data.

    <field name="location" type="location" indexed="true" stored="true" />
    <field name="location_0_coordinate" type="double" indexed="true" stored="true" />
    <field name="location_1_coordinate" type="double" indexed="true" stored="true" />

Lets take a step back and understand how the field works. In field definition we have defined an attribute  _subFieldSuffix_ as **_coordinate**. Now in the fields, we have created two fields which ends with _coordinate which will store the latitude and longitude. Note that both the fields should be defined as **double** and they are both indexed. 

## Indexing Location Data

Now to index the location data, we need to have the latitude and longitude in the following format as string.

    {latitude},{longitude}

The values are seperated by comma with no spaces between them. If the indexing is successful, We should have the location string in _location_ field and also the latitude and longitude data should be stored as part of location_0_coordinate and location_1_coordinate respectively.

## Querying with Location data
In order to query based on the location data, we need to change the SOLR query. If the mission is to fetch all the documents which falls within radius d of certain lat-long, the following URL will do the magic.

    http://{solr_ip}:{solr_port}/solr/{solr_core_name}/select?wt=json&q=*:*&start=0&rows=50&fq={!geofilt pt=17.46,78.35 sfield=location d=50}

Replace the variables based on your configuration and the query will give out all the documents with location falling within 50 Kilometers radius from the latitude longitude provided (17.46,78.35). 

## Advanced Location Queries
The above query is very basic which most of the time do not meet our requirements. Now lets take a look at different scenarios and the changes required in the query. 

- **Case 1**: Distance between the query location and document location.
    In many cases, we want SOLR to return the distance along with the document fields. For this purpose, SOLR provides [**distance function**](https://wiki.apache.org/solr/SpatialSearch#geodist_-_The_distance_function) `geodist`. To get the distance as part of the returned fields, use `fl` parameter like,<br/><br/> `fl=*,_dist_=geodist()` <br/><br/> which translates to, <br/><br/> `fl=*,_dist_:geodist(location,17.46,78.35)` <br/><br/> After adding _fl_ parameter to the previous SOLR query, we will be able to see a field called \_dist\_ which will show the distance.

- **Case 2**: Documents Sorted based on Distance
    By default, all the result documents are sorted based on the score but when performing location search, it makes sense to sort it based on the distance. We will use the same function _geodist_ for sorting. We will use the `sort` parameter.<br/><br/>`sort=geodist(location,17.460076979367578,78.35068197197522) asc` <br/><br/> or simply, <br/><br/> `sort=geodist() asc` <br/>

So the final query which will get all the documents within 20 kilometer radius along with the distance and sorted by increasing distance will be,

    http://{solr_ip}:{solr_port}/solr/{solr_core_name}/select?wt=json&q=*:*&start=0&rows=50&fq={!geofilt pt=17.46,78.35 sfield=location d=20}&fl=*,_dist_:geodist(location,17.46,78.35)&sort=geodist(location,17.46,78.35) asc

## SOLR Location search using Solarium
In case you are using Solarium for PHP to connect and execute queries, the following code snippet should help.

<script src="https://gist.github.com/arunchinnachamy/35855f01f7b430ce696c.js"></script>

In case you find something wrong or facing issues configuring SOLR for location search, Please comment below.
