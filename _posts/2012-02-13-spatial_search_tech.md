---
layout: post
title: "Whats there for Spatial Search?"
description: ""
category: 
tags: []
---
{% include JB/setup %}

<br><br>
### Objective
To research in opensource ecosystem for GeoSpatial search for locations within a circle/rectangle/polygon


<br><br>
### Problem
There is a need for efficient geospatial search on collection of locations in the order of atleast 1+ Million. The conventional database search falls in performance for such large dataset.

<br><br>
### Solution
Indexing search would be the best solution. What could be more efficient is performing distributed indexing search. Apache Lucene/Solr could be a ideal candidate for such tasks. Let us compare or analyse different cloud platform that would help scale up lucene/solr.


<br><br><br>
### Spatial Search Technologies
<br>

<br><br>
#### Solr Spatial
[wiki.apache.org/solr/SpatialSearch](http://wiki.apache.org/solr/SpatialSearch)

* no polygon feature


<br><br>
#### Hadoop + Solr
[issues.apache.org/jira/browse/SOLR-1301](https://issues.apache.org/jira/browse/SOLR-1301)

Solr patch 1301 specifically deals with it.

* the patch is still Open (not very mature)
* Hadoop is again very much oriented for node heavy clustered environments


<br><br>
#### SolrCloud
[wiki.apache.org/solr/SolrCloud](http://wiki.apache.org/solr/SolrCloud)
This opensource project is specifically designed to make Solr work in cloud environment. Zookeeper, an another opensource is used to coordinate the cluster.

* adds the complexity of Zookeeper to that of Solr
* oriented towards multi-node/clustered environments


<br><br>
#### Hibernate Spatial
[www.hibernatespatial.org](http://www.hibernatespatial.org)

* pure Hibernate and database support for spatial entities


<br><br>
#### Neo4J Spatial
[www.github.com/neo4j/spatial](https://github.com/neo4j/spatial)
<br>[Useful Blog](http://blog.neo4j.org/2011/03/neo4j-spatial-part1-finding-things.html)

* lucene based with excellent spatial support
* only enterprise edition has 'High Availability' feature

<br><br>
#### MongoDB Spatial Indexing
[www.mongodb.org/display/DOCS/Geospatial+Indexing](http://www.mongodb.org/display/DOCS/Geospatial+Indexing)

* Polygon search not supported yet


<br><br>
#### ElasticSearch
[www.elasticsearch.org](http://www.elasticsearch.org/)

NoSQL/REST/JSON based distributed computing based on Lucene  <br/>

* relatively new support for spatial search
* oriented towards multi-node/clustered environments


<br><br>
#### Solandra
[www.github.com/tjake/Solandra](https://github.com/tjake/Solandra)

Solandra is a real-time distributed search engine built on Apache Solr and Apache Cassandra <br/>

* has very limited and immature support for spatial data
* adds the complexity of Cassandra on top of the complexity of Solr
* oriented towards multi-node/clustered environments

<br><br>
#### Amazon CloudSearch
[aws.amazon.com/cloudsearch](http://aws.amazon.com/cloudsearch/)

* still in beta
* no spatial support yet


<br><br>
#### Other index search techniques
* Database loaded on to cache - DB provides features where the whole/part of database can be loaded on to a cache. So the disk io is avoided, which could help in significant improvement in throughput
* [Sphinx - C++ based](http://sphinxsearch.com/)
* [Compass - Lucene based](http://compass-project.org/). The same author came up wih ElasticSearch
* [Katta - Lucene based cluster](http://katta.sourceforge.net/)

<br>
