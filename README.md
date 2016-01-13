# fc3-camel-indexer
## Purpose
Replace the aging GSearch indexer with a simple camel route that could be extended easily.

## Deployment
1. Clone the repository.
1. Change into the directory.
1. Run ```mvn clean install```
1. Copy the fc3-camel-indexer.war file to your webapp container.
1. Configure as below.

## Configuration
Uses several overridable java options.

* fedora3.indexer.fedoraUrl - The URL of your Fedora instance (default: localhost:8080/)
* fedora3.indexer.fedoraPath - The base path at the above URL for fedora (default: /fedora)
* fedora3.indexer.fedoraUser User with API-M privileges to Fedora (default: fedoraAdmin)
* fedora3.indexer.fedoraPass - Password for above user (default: fedoraAdmin)
* fedora3.indexer.solrUrl - The URL of your Solr instance (default: localhost:8080/solr)
* fedora3.indexer.jmsBroker - The JMS Broker (default: tcp://localhost:61616)
* fedora3.indexer.jmsQueue - The JMS queue to watch (default: queue:fedora.apim.update)
* fedora3.indexer.xslt - The location of the XSLT to apply (default: classpath://test.xslt)
* fedora3.indexer.concurrent - Number of concurrent processes to perform on Solr insert/delete routes (default: 5)

You will almost certainly need to override the ```fedora3.indexer.xslt``` option.

You can specify a file path with ```-Dfedora3.indexer.xslt=file:///full/path/to/xslt```

## Reindexer
This webapp also deploys a REST endpoint at /&lt;context&gt;/reindex/. Where &lt;context&gt; is the deployed name (default: fedora3-solr-indexer).

This endpoint accepts GET requests of the form /&lt;context&gt;/reindex/&lt;PID&gt;

The PID supplied is passed onto the configured JMS topic/queue to be indexed. No response is given.

For example:
```
curl -XGET http://localhost:8080/fedora3-solr-indexer/reindex/islandora%3A200
```


## Credit
All credit to [acoburn](https://github.com/acoburn) for this is just an implementation of his camel route wrapped in a war deployment.