---
title:  "Streaming data with Kafka Apache - Design"
date:   2022-05-26 9:15:43
categories: [data]
tags: [data]	
---
If you are considering setting up a secondary data source for reporting or other purposes, Kafka can assist in streaming data from the primary data source to the secondary data source. The following steps outline how to transfer data from a SQL Server to Elastic:

<img src="{{ site.baseurl }}/images/blog/streaming-data-with-kafka/kaffa-design.png" class="fullsize-image" alt="Image">

<h4>1</h4>
Enable change data capture on a SQL Server, you can set up event notifications for create, update, and delete operations. Additionally, you can customize which tables trigger change data capture events.

<h4>2</h4>
Use Debezium's SQL Server source connector to recored events for each table in a separate Kafka topic, where they can be easily consumed by applications and services.

<h4>3</h4>
You could create a consumer that subscribes to a Kafka topic and forwards processed data to Elastic. Alternatively, you may utilize an Elastic sink connector to transmit data from Kafka to Elastic. 

<h4>4</h4>
To access reports, you could use a tailored client application. Alternatively, you have the option to visualize data via Tableau or Power BI.

* Note about Kafka connectors, they come in two flavors, source connectors and sink connectors. The connector that takes data from a Producer and feeds them into a topic is called source connector. The connector that takes data from a Topic and delivers them to a Consumer is called Sink Connector.