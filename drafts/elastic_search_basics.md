## ElasticSearch Basics

---

### What is ElasticSearch?

[From elastic.co](https://www.elastic.co/):
>Elasticsearch is an open source distributed, RESTful search and analytics engine capable of solving a growing number of use cases.

ElasticSearch 是一个开源的，分布式的，RESTful风格的搜索和分析引擎，用以应对不断增长的用户数据。

[From wikipedia](https://en.wikipedia.org/wiki/Elasticsearch):

> Elasticsearch is a search engine based on Lucene. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and is released as open source under the terms of the Apache License. Official clients are available in Java, .NET (C#), PHP, Python, Apache Groovy,Ruby and many other languages. According to the DB-Engines ranking, Elasticsearch is the most popular enterprise search engine followed by Apache Solr, also based on Lucene.

ElasticSearch是一个基于[Lucene](https://en.wikipedia.org/wiki/Apache_Lucene)(一个开源的信息检索软件库)的搜索引擎。它提供分布式的，[多租户功能的](https://zh.wikipedia.org/wiki/%E5%A4%9A%E7%A7%9F%E6%88%B6%E6%8A%80%E8%A1%93)全文检索检索引擎，并且提供 HTTP web 接口和不需要schema的 JSON 文档格式。 ElasticSearch 使用 Java 语言开发并遵循Apache License以开源方式发布。 正式客户可以使用 Java, .NET (C#), PHP, Python, Apache Groovy,Ruby 以及其他许多语言。根据 DB-Engines 排名，ElasticSearch 是随 Apache solr（也基于Lucene） 之后的最流行的搜索引擎。

---

**resources:**

- [官方网站](https://www.elastic.co/)
- [官方github](https://github.com/elastic)
- [官方doc中关于Ruby和Rails的页面](https://www.elastic.co/guide/en/elasticsearch/client/ruby-api/current/index.html)
- gitub上ruby页面 [elasticsearch-ruby](https://github.com/elastic/elasticsearch-ruby)
- gitub上rails页面 [elasticsearch-rails](https://github.com/elastic/elasticsearch-rails)
- railscasts 上两个案例视频，版本有点旧了，但演示了一些基本用法和功能。
  - [part-1](http://railscasts.com/episodes/306-elasticsearch-part-1)
  - [part-2](http://railscasts.com/episodes/307-elasticsearch-part-2)

---

**Following Getting Started Video**

Agenda:

- Introduction to the Elastic Stack
- Installation of Elasticsearch and kibana
- Working with JSON documents
- CRUD
  - Create
    - Different ways to insert/create an index
    - Bulk indexing documents
  - Read
    - Basic searches
    - Intermediate searches
    - Facets and aggregations
    - Sample geo search
  - Update
    - Updating documents
  - Delete
    - Deleting documents
- Mappings
- Analyzers


### Introduction to the Elastic Stack
![](https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7+2018-03-17+%E4%B8%8B%E5%8D%889.42.57.png)

![](https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7+2018-03-17+%E4%B8%8B%E5%8D%889.46.51.png)
### Installation of Elasticsearch and kibana

### Working with JSON documents
### CRUD
#### Create
##### Different ways to insert/create an index
##### Bulk indexing documents
#### Read
##### Basic searches
##### Intermediate searches
##### Facets and aggregations
##### Sample geo search
#### Update
##### Updating documents
#### Delete
##### Deleting documents
### Mappings
### Analyzers
