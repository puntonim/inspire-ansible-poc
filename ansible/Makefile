# This Makefile requires the following commands to be available:
# * virtualenv
# * python2

DEPS:=requirements.txt
VIRTUALENV=$(shell which virtualenv)
PIP:="venv/bin/pip"
CMD_FROM_VENV:=". venv/bin/activate; which"
PYTHON=$(shell "$(CMD_FROM_VENV)" "python2.7")
ANSIBLE_PLAYBOOK=$(shell "$(CMD_FROM_VENV)" "ansible-playbook")

.PHONY: venv deploy requirements pyclean clean pipclean

venv:
	$(VIRTUALENV) -p $(shell which python2.7) venv
	. venv/bin/activate
	$(PIP) install -U "pip>=18.0" -q
	$(PIP) install -U -r $(DEPS)

_make_venv_if_empty:
	@[ -e ./venv/bin/python ] || make venv

deploy/qa: _make_venv_if_empty
	$(ANSIBLE_PLAYBOOK) deploy_qa.yml -i hosts

deploy/qa/%: _make_venv_if_empty
	$(ANSIBLE_PLAYBOOK) deploy_qa.yml -i hosts --extra-vars "code_version=$*"

deploy/prod: _make_venv_if_empty
	$(ANSIBLE_PLAYBOOK) deploy_prod.yml -i hosts

deploy/prod/%: _make_venv_if_empty
	$(ANSIBLE_PLAYBOOK) deploy_prod.yml -i hosts --extra-vars "code_version=$*"


## Utilities for the venv currently active.

_ensure_active_env:
ifndef VIRTUAL_ENV
	@echo 'Error: no virtual environment active'
	@exit 1
endif

requirements: _ensure_active_env
	pip install -U -r $(DEPS)


## Generic utilities.

pyclean:
	find . -name *.pyc -delete
	rm -rf *.egg-info build
	rm -rf coverage.xml .coverage
	rm -rf .pytest_cache

clean: pyclean
	rm -rf venv
	rm -rf .tox
	rm -rf dist

pipclean:
	rm -rf ~/Library/Caches/pip
	rm -rf ~/.cache/pip
