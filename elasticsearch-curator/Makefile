

qa:
	curator --config config-qa.yml actions.yml
	curl -XPUT -k -u $$APPMETRICS_ELASTICSEARCH_AUTH -H "Content-Type: application/json" https://inspire-qa-logs-master1.cern.ch:443/inspire*/_settings -d '{"index.blocks.read_only_allow_delete": null}'

prod:
	curator --config config-prod.yml actions.yml
	curl -XPUT -k -u $$APPMETRICS_ELASTICSEARCH_AUTH -H "Content-Type: application/json" https://inspire-prod-logs-master1.cern.ch:443/inspire*/_settings -d '{"index.blocks.read_only_allow_delete": null}'
