THIS IS A STILL WIP PROJECT, USE WITH CAUTION!!!


- First, get a Kerberos ticket:
$ kinit pcoffett@CERN.CH


** SPECIFIC COMMANDS

- Run a specific command on a specific host:
$ ansible -i hosts inspire-qa-worker3-task7.cern.ch -a "date"

- A command within a dir:
$ ansible -i hosts inspire-qa-worker3-task7.cern.ch -a 'chdir=/opt/inspire/src/inspire git log -10'

- On a group:
# -b: to become super user (sudo)
$ ansible -i hosts qa-task -a "supervisorctl status" -b


** PLAYBOOKS

- Run a playbook:
$ ansible-playbook myplay.yml -i hosts

- Limit to a host and to a tag:
$ ansible-playbook myplay.yml -i hosts --limit inspire-prod-worker3-task1.cern.ch --tags "git-log"


** DEPLOYMENTS

The aim is to deploy while keeping the service running.
This is achieved deploying in batches.

* IMPORTANT: the deployments playbook do not execute and it is not compatible
with any DB migrations nor Elasticsearch ops like remap/reindex (and it does
not even properly check for that).
Thus, you have to make sure yourself that the codechange you are deploying
does not include such operations before using the deployments playbook *

- Deploy to QA (git branch `master` by default):
$ ansible-playbook deploy_qa.yml -i hosts

- Deploy to PROD (git branch `prod` by default):
$ ansible-playbook deploy_prod.yml -i hosts

- Chose a specific code version:
$ ansible-playbook deploy_qa.yml -i hosts --extra-vars "code_version=master"
$ ansible-playbook deploy_qa.yml -i hosts --extra-vars "code_version=5b1682d10c66505c4995f24dce59bc11f8e4a4bd"
$ ansible-playbook deploy_qa.yml -i hosts --extra-vars "code_version=v0.12.7"
