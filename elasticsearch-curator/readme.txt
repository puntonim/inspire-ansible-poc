Elasticsearch Curator helps you curate, or manage, your Elasticsearch indices.
https://www.elastic.co/guide/en/elasticsearch/client/curator/current/about.html

The aim of this repo is to automate the cleaning of old indices in ES.
When there is no disk space available in some ES machines (or better when it
reaches 90% usage) ES goes in read-only mode.
You can check that with:
$ curl -k -u inspire:inspire https://inspire-prod-logs-master1.cern.ch:443/_all/_settings | jq
In th output you will get:
read_only_allow_delete: true


Install:
$ pip install -r requirements.txt

Usage:
- in qa:
$ DAYS=30 APPMETRICS_ELASTICSEARCH_AUTH=user:pass make qa
- in prod:
$ DAYS=30 APPMETRICS_ELASTICSEARCH_AUTH=user:pass make prod


Notes:
Some useful ES queries:
- All indices:
GET /_cat/indices?v

- All nodes:
GET _nodes/stats/fs?pretty=true

- Settings and read only state:
GET _all/_settings
GET inspirelogs-*/_settings

- Usage stats:
GET _all/_stats


Future developments:
Eventually this can be an ansible playbook.
