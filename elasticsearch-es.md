# ElasticSearch

## Useful plugins

* [ElasticSearch Inquisitor](https://github.com/polyfractal/elasticsearch-inquisitor)
    * Access it at: `<proto>//<host>:<port>/_plugin/inquisitor/#/analyzers`

`TODO for Authors:` Need to create a docker-compose file with an entrypoint script that installs this plugin for readers to play around with the most appropriate version of ES. Plugins usually can't keep up with the lightning fast progress of ES.

## Useful tips

### Analyzers
* It is possible to [define new analyzers](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/indices-update-settings.html#update-settings-analysis) for an index. But it is required to **close** the index first and **open** it after the changes are made.
* Gram-based approach:
    * http://jontai.me/blog/2013/02/adding-autocomplete-to-an-elasticsearch-search-application/
    * http://exploringelasticsearch.com/searching_usernames_and_tokenish_text.html#ch-strangetext
        * ngrams don’t attempt language heuristics such as stemming that don’t apply to strings like “cmdrtaco”. They also handle mis-spelled terms well, since a search just needs to have a plurality of matches of sub-parts of a given term
        * ngrams allow developers to trade storage for CPU
    * https://www.found.no/foundation/fuzzy-search/
        * https://www.found.no/foundation/text-analysis-part-1/#using-ngrams-for-advanced-token-searches
        * https://www.found.no/foundation/text-analysis-part-2/
* Auto-Suggester by ES: http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-suggesters-completion.html

### Rebuilding an index
* Scan / [Scroll](http://www.elasticsearch.org/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-scroll)
    * https://github.com/karussell/elasticsearch-reindex
        * cloned it
        * ran build:
            * mvn -DskipTests clean package
        * uploaded the zip from "target” directory to hosted ES
        * reindex operation errored out during trial & error
* [Word Delimiter](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-word-delimiter-tokenfilter.html)

## Examples & Exercises

`TODO for Authors:` Need to create a docker-compose file to setup and play with analyzers quickly.


`TODO for Authors:` Use `sense` chrome plugin or CURL to demonstrate.

```
GET /my_index/_analyze?field=product.image_url&text="t112_1059_Cinnamon - Incense Stick"

GET /_analyze?tokenizer=keyword&filters=lowercase&text="t112_1059_Cinnamon - Incense Stick"

GET /_analyze?token_filters=word_delimiter&text="O’Neil’s hello---there, dude SD500 PowerShot Wi-Fi"

GET /_analyze?tokenizer=standard&text="t112_1059_Cinnamon - Incense Stick"

GET /_analyze?analyzer=simple&text="t112_1059_Cinnamon - Incense Stick"

GET /_analyze?tokenizer=keyword&token_filters=word_delimiter,lowercase&text="t112_1059_Cinnamon - Incense Stick"

GET /my_index/_analyze?field=product.name&text="t112_1059_Cinnamon - Incense Stick"

POST /my_index/product/_search
{"query":{"bool":{"must":[{"query_string":{"default_field":"_all","query":"cinna"}}]}}}

POST /my_index/product/_search
{"query":{"bool":{"must":[{"query_string":{"default_field":"name","query":"cinnamon"}}]}}}

GET /my_index/_analyze?field=product.barcodes&text="['20015','20016']"

POST /my_index/product/_search
{
   "query": {
      "term": {
         "barcodes": "MANUAL:20015"
      }
   }
}
POST /my_index/product/_search
{
   "query": {
      "multi_match": {
         "query": "20015",
         "fields": [
            "barcodes"
         ]
      }
   }
}

POST /my_index/product/_search
{
   "query": {
      "match_all": {}
   },
   "facets": {
      "department_name": {
         "terms": {
            "field": "barcodes"
         }
      }
   }
}
```

