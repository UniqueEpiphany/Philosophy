From July 2014, I work as a software engineer at News-Distribution team in FactSet. News-Distribution team have two sub-teams, one is News-Searching, the other is News-Alerting, and I work at the News-Searching team.

My primary job here is to maintain News-Search Service, which is a HTTP service that take search requests and return financial information to clients. You can basically think News-Search Service is a specialized search engine for financial professionals. However, we manage the service and its related API only, our direct consumers are FactSet front-end applications, e.g. FactSet workstation and other Web applications. Basically, most(if not all) full text search requests those front-end applications received from clients will be forward to our News-Search service. 

### News-Search Service vs  public search engine

First of all, __data quality matters__. And it matters a lot to financial progessionals. Unlike most public search engines, since NSS focused on financial industry only, it naturally gets rid of significant amount of noises. 

Second, many financial documents, e.g. analysts reports and other research documents, they are not public which means that you can't collect those documents through web scraping. The only way to get is contact data vendor directly.  You are not able to get thoses documents by simply searching on Google or Yahoo.

### News-Search Service architecture

News-Search service is a state-less service, the stateless design simplifies the server design because there is no need to dynamically allocate storage to deal with conversations in progress. If a client session dies in mid-transaction, no part of the system needs to be responsible for cleaning up the present state of the server. The stateless feature also gives us the horizontal scalability by simply adding more machines to handle future growth.

News-Search service has two clusters right now, each clusters have two hosts, and each host have 3 running instances. 

### What should be improved for News-Search Service

1. News-Searching only have two engineers. That is far from enough to make it have more powerful features. And you will understand why I put it here first from following points.   

2. News-Searching doesn't own the expertise on full-text search, instead, it relies on Elasticsearch, which is a full-text search engine based on Apache-Lucene. I need to make it clear here, Elasticsearch is simply a distributed system built based on Lucene, it doesn't implement any full-text search features by itself. Even though Elasticsearch does a great job, I would still think having the full-text search expertise is important. And if we think big, we should build our search engine from scratch(get involved in making contributions to Lucene is a good option). Why? This is not a "rebuild a wheel" thing. Searching is not a wheel, it is an extremely powerful feature. If you don't own it, you are not able to optimize it. Sometimes we are not able to address client's issues and optimize our searching features because we simply don't own it all. We are dependent on Elasticsearch, and Elasticsearch is dependent on Lucene. This is a sad fact.

3. Many companies who have full-text search features are using machine learning and natural language processing techniques to empower their search engine. FactSet has machine-learning and natural language processing team too, but we are not working closely with them. I personally think these teams should be merged into one big team. 


