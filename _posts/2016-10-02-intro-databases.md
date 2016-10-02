---
layout: post
title: Introduction to SQL and NoSQL Databases
description: "Beginner introduction to SQL and NoSQL databases"
modified: 2016-10-02
tags: [databases,sql, nosql, XML, JSON]
image:
    feature: sql_nosql.jpg
    credit: udemy
    creditlink: https://blog.udemy.com/sql-contains/
---

**SQL** (Structured Query Language) and **NoSQL** (commonly defined as “Not only Structured Query Language”) are the 2 most-popular querying languages in database 
management systems. They are not the only query languages however, with others including: XPath, XBase++ and RDF query language (such as SPARQL – SPARQL Protocol 
and RDF Query Language). <!-- more -->A list of query languages is provided on [Wikipedia](https://en.wikipedia.org/wiki/Category:Query_languages).

A database management system can be based on different data models, with the most common being:

* Relational
* Key-value
* Column-oriented/tabular
* Document-oriented
* Graph-based

Others include: multi-model, object models, XML, multidimensional, multivalue, and time series. These are mostly NoSQL models and will be mentioned in the NoSQL section. 

## SQL Relational Databases

SQL is based on **Relational Database Management Systems (RDBMS)**, hence consists of relational data models. In relational databases, a table (called **relation**) follows a **schema**, 
i.e. a predefined structure. Relations have a set of **attributes** (represented by columns) and **tuples** (represented by rows), therefore each tuple has a set of (equal number of) 
attributes. Popular SQL databases include: MySQL, PostgreSQL, and SQLite.

<figure>
	<img src="/images/relational_database.png" alt="">
	<figcaption>Source: https://en.wikipedia.org/wiki/Category:Query_languages</figcaption>
</figure>
 
### Normalization
Data in relational databases should be in the **first normal form (1NF**), where every tuple contains exactly one value for each attribute. This is often confused and used 
interchangeably with the term **atomic form** which implies indivisible data. For example, the date attribute (2014-01-01) is not in the atomic form because it can be 
subdivided into year, month and day, but it is considered normalized because each ‘sub-attribute’ does not provide the same information. A denormalized attribute would 
be one which contains multiple values (dates in this case) for an attribute for a given tuple. Address is another example of normalized but non-atomic. While ‘date’ 
attribute can be stored in its atomic form, database management systems can manipulate non-atomic (but normalized) values to return part of the date, the whole date or 
even perform simple arithmetic (add/subtract intervals). Thus, the non-atomic form is not necessary for such attribute.

While normalization is required for SQL databases, it is not necessary for NoSQL. Normalization leads to faster queries, however updating information will be significantly 
slower since multiple records need to be modified. 

## NoSQL databases

Unlike SQL models, NoSQL databases do not require the pre-determined fixing of data types for each attributes (i.e. schema), and thus are more flexible. However, this 
may cause consistency issues. Popular NoSQL databases include: MongoDB, CouchDB, Apache Cassandra, Redis, Couchbase, Elastic, and DynamoDB. 

There are several NoSQL data models:

- Column-oriented
- Document-oriented
- Key Value / Tuple store
- Graph databases
- Multi-model databases
- Object Databases (soft NoSQL systems)
- Grid & Cloud Database Solutions
- XML Databases
- Multidimensional databases
- Multivalue databases
- Event Sourcing
- Time Series / Streaming Databases
- Other NoSQL-related
- Scientific and specialized DBs

For each data model, several solutions are currently available. A non-comprehensive list of these is below (compiled from [no-sqldatabase.org](http://nosql-database.org/)):


| Column-oriented | Document-oriented | Key-Value | Graph models | Multimodel |
|:----------------|:-----------------:|----------:|-------------:|-----------:|
| Hadoop/HBase  	| Elastic   		| DynamoDB   			| Neo4J 			| ArangoDB
| MapR   			| ArangoDB   		| Azure Table Storage   | ArangoDB			| OrientDB
| Cassandra  		| OrientDB   		| Riak   				| OrientDB			| Datomic
| Scylla   			| gunDB   			| Redis   				| gunDB				| gunDB
| Hypertable 		| MongoDB   		| Aerospike   			| Infinite Graph	| CortexDB
| Accumulo   		| Cloud Datastore   | LevelDB   			| Sparksee			| AlchemyDB
| Amazon SimpleDB   | Azure DocumentDB  | RocksDB   			| TITAN				| WonderDB
| Cloudata   		| RethinkDB   		| Berkeley DB   		| InfoGrid			| RockallDB
| MonetDB   		| Couchbase   		| Oracle NoSQL   		| HyperGraphDB		|
| HPCC   			| CouchDB   		| GenieDB   			| Trinity			|
| Apache Flink   	| ToroDB   			| BangDB   				| AllegroGraph		|
| IBM Informix   	| SequoiaDB   		| Chordless   			| BrightstarDB		|
| Splice Machine   	| RavenDB   		| Scalaris  			| Bigdata			|
| eXtremeDB   		| MarkLogic   		| Tyrant   				| Meronymy			|
| ConcourseDB   	| Clusterpoint   	| Scalien   			| WhiteDB			|
| Druid   			| JSON ODM   		| Voldemort   			| Onyx Database		|
| KUDU   			| NeDB   			| Dynomite   			| OpenLink Virtuoso	|
| Elassandra   		| Terrastore   		| KAI   				| VertexDB			|
|    				| AmisaDB   		| memcacheDB   			| FlockDB			|
|    				| JasDB   			| Faircom C-Tree   		| Weaver			|
|    				| RaptorDB   		| LSM   				| BrightstarDB		|
|    				| Djondb   			| KitaroDB   			| Execom IOG		|
|    				| EJDB   			| Upscaledb   			| Fallen 8			|
|    				| Densodb   		| STSdb   				|
|    				| SisoDB   			| Tarantool/Box   		|
|    				| SDB   			| Chronicle Map   		|
|    				| NoSQL embedded db | Maxtable   			|
|    				| ThruDB   			| Quasaradb   			|
|    				| iBoxDB   			| Pincaster   			|
|    				| BergDB   			| RaptorDB   			|
|    				| IBM Cloudant   	| TIBO Active spaces   	|
|    				| 				   	| Allegro-C   			|
|    				| 				   	| nessDB   				|
|    				| 				   	| HyperDex   			|
|    				| 				   	| SharedHashFile   		|
|    				| 				   	| Symas LMDB   			|
|    				| 				   	| Sophia   				|
|    				| 				   	| NCache   				|
|    				| 				   	| TazyGrid   			|
|    				| 				   	| PickleDB   			|
|    				| 				   	| Mnesia   				|
|    				| 				   	| LightCloud   			|
|    				| 				   	| Hibari   				|
|    				| 				   	| OpenLDAP   			|
|    				| 				   	| Genomu   				|
|    				| 				   	| BinaryRage   			|
|    				| 				   	| Elliptics				|
|    				| 				   	| DBreeze   			|
|    				| 				   	| TreodeDB   			|
|    				| 				   	| BoltDB   				|
|    				| 				   	| Serenety   			|
|    				| 				   	| Cachelot   			|
|    				| 				   	| filejson   			|
|----
{: rules="groups"}

I will not go into each database model, but document-oriented are worth noting given their relatively unique structure and increasing popularity. Elastic is one solution 
that utilizes JSON files as documents, each containing inner objects. Each document can have objects with different fields each time and hence unlike in relational databases, 
the same attributes are not required let alone a fixed schema.  For reference, below is an example taken from the [ElasticSearch documentation](https://www.elastic.co/guide/en/elasticsearch/guide/current/modeling-your-data.html):

{% highlight javascript %}
{
  "title": "Nest eggs",
  "body":  "Making your money work...",
  "tags":  [ "cash", "shares" ],
  "comments": [ 
    {
      "name":    "John Smith",
      "comment": "Great article",
      "age":     28,
      "stars":   4,
      "date":    "2014-09-01"
    },
    {
      "name":    "Alice White",
      "comment": "More like this please",
      "age":     31,
      "stars":   5,
      "date":    "2014-10-22"
    }
  ]
}
{% endhighlight %}

## SQL vs. NoSQL –Which to choose?

While both SQL and NoSQL database models serve the same function, i.e. that of storing and retrieving/querying data, this is achieved in different approaches and is optimized 
for different scenarios. SQL models such as MySQL enables fast and complex querying, but do not scale as well as NoSQL models, such as Apache Cassandra. Related to size, the 
InnoDB engine for MySQL (commonly used engine) imposes a [maximum 1017 column-limit](http://dev.mysql.com/doc/refman/5.7/en/innodb-restrictions.html). NoSQL databases often have 
larger limits, with [Cassandra having a limit of 2 billion cells (rows x columns)](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/refLimits.html).

If the data is unstructured/less structured and document-based, than document-oriented NoSQL models, such as elastic, may be a no-brainer. The problem requires further thought 
however when the data structure can fit (justifiably and appropriately) in both a relational database and a column-oriented database. The CAP triangle has been (over-)used as 
a guide to determine which database model to choose. It is based on 3 database properties:

* C: consistency = when using multiple copies, each client has same view of the data. This means that you need to make it appear as though there is only a single 
copy of the data even though there may be copies (replicas, caches) of the data in multiple places. The term “consistency” has been argued to be a misnomer. [Linearizability is more appropriate](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html).
* A: availability = all clients can read and write. All nodes provide a non-error response.
* P: partition tolerance = system works well across physical network partitions. Nowadays, this has been recognized as impossible to achieve due to internet/asynchronous networking that may drop or delay messages.

<figure>
	<img src="/images/CAP_triangle.png" alt="">
	<figcaption>Source: http://blog.nahurst.com/visual-guide-to-nosql-systems</figcaption>
</figure>

However, several disagree with the usage of the CAP theorem. [A blog post by Martin Kleppmann goes into detail about this](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html). 
This leave us without a “rule-of-thumb” (or multiple thumbs in this case) when deciding what’s the ‘best solution’, making such task certainly not a straightforward one. Even ‘worse’ when the differences between 
SQL and NoSQL are becoming more blurred with the adoption of features from one another. 

Nonetheless, some simple questions to start asking are: what’s the size and structure the data (Is a relational table or less structured documents appropriate)? How often will the records be updated 
(Should the data be normalized or not)? Will complex queries be required? 





