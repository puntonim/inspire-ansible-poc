# This Makefile requires the following commands to be available:
# * virtualenv
# * python2

DEPS:=requirements.txt
VIRTUALENV=$(shell which virtualenv)
PIP:="venv/bin/pip"
CMD_FROM_VENV:=". venv/bin/activate; which"
PYTHON=$(shell "$(CMD_FROM_VENV)" "python2.7")
CURATOR=$(shell "$(CMD_FROM_VENV)" "curator")

.PHONY: venv deploy

venv:
	$(VIRTUALENV) -p $(shell which python2.7) venv
	. venv/bin/activate
	$(PIP) install -U "pip>=18.0" -q
	$(PIP) install -r $(DEPS)

_make_venv_if_empty:
	@[ -e ./venv/bin/python ] || make venv

qa: _make_venv_if_empty
	$(CURATOR) --config config-qa.yml actions.yml
	curl -XPUT -k -u $$APPMETRICS_ELASTICSEARCH_AUTH -H "Content-Type: application/json" https://inspire-qa-logs-master1.cern.ch:443/inspire*/_settings -d '{"index.blocks.read_only_allow_delete": null}'

prod: _make_venv_if_empty
	$(CURATOR) --config config-prod.yml actions.yml
	curl -XPUT -k -u $$APPMETRICS_ELASTICSEARCH_AUTH -H "Content-Type: application/json" https://inspire-prod-logs-master1.cern.ch:443/inspire*/_settings -d '{"index.blocks.read_only_allow_delete": null}'
