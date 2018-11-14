# INSPIRE OPS

*THIS IS A STILL WORK IN PROGRESS, USE WITH CAUTION!*


- Firstly, get a Kerberos ticket and set the env var KERBEROS_USERNAME:
```bash
$ kinit myuser@CERN.CH
$ export KERBEROS_USERNAME=myuser
```

## SPECIFIC COMMANDS

- Note

- Run a specific command on a specific host:
```bash
$ ansible -i hosts inspire-qa-worker3-task7.cern.ch -a "date"
# A command within a dir:
$ ansible -i hosts inspire-qa-worker3-task7.cern.ch -a 'chdir=/opt/inspire/src/inspire git log -10'
# On a group of hosts:
# -b: to become super user (sudo)
$ ansible -i hosts qa-task -a "supervisorctl status" -b
```


## PLAYBOOKS

- Run a playbook:
```bash
$ ansible-playbook myplay.yml -i hosts
# Limit to a host and to a tag:
$ ansible-playbook myplay.yml -i hosts --limit inspire-prod-worker3-task1.cern.ch --tags "git-log"
```


## DEPLOYMENTS

The aim is to deploy while keeping the service running.
This is achieved with a rolling deployment.

*IMPORTANT: the deployments playbook do not execute and it is not compatible yet
with any DB migrations nor Elasticsearch ops like remap/reindex (and it does
not even properly check for that).
Thus, you have to make sure yourself that the code change you are deploying
does not include such operations before using the deployments playbook*

- Deploy to QA (git branch `master` by default):
*Note: replace qa with prod to deploy to PROD (git branch `prod` by default)8
```bash
$ make deploy/qa
# Or:
$ ansible-playbook deploy_qa.yml -i hosts
```

- Chose a specific code version:
```bash
$ make deploy/qa/v0.12.7
# Or:
$ ansible-playbook deploy_qa.yml -i hosts --extra-vars "code_version=master"
$ ansible-playbook deploy_qa.yml -i hosts --extra-vars "code_version=5b1682d10c66505c4995f24dce59bc11f8e4a4bd"
$ ansible-playbook deploy_qa.yml -i hosts --extra-vars "code_version=v0.12.7"
```

### Deployments notes

- The deployment strategy is as close as possible to the old one in:
https://gitlab.cern.ch/ai/it-puppet-hostgroup-inspire/blob/qa/code/files/inspire/tasks3.py
The only difference are that the build (pip and fronted assets) is actually
done on the nodes and the rolling deployment itself (rather than the big bang).

- The main plays (deploy_qa.yml and deploy_prod.yml) uses a single role to
maximize parallelization of tasks execution. This makes the `common` role
parametric on the actual host.
Another strategy would be write 2 roles (web_node, task_node, crawler_node),
checkout the branch `linear-deployment` to check that. But this strategy
does not maximize the parallelization of tasks execution.

- Possible improvements:
  - install (pip and fronted assets) on each node only those requirements actually
needed by the specific node.
  - checkout the code in a dir named after the version (eg. "v0.12.10"), build
    a new virtual environment in that dir, then create a symbolic link from
    a dir "current" to "v0.12.10". Celery, Gunicorn and all the stack always
    point to the "current" dir. After changing the symlink they are restarted.
