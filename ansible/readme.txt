THIS IS A WIP PROJECT!!!!
USE WITH CAUTION!!!


- Get a Kerberos ticket first:
$ kinit pcoffett@CERN.CH

- Then
$ ansible-playbook play.yml -i hosts -v

To limit to a single host:
$ ansible-playbook play.yml -i hosts --limit inspire-prod-worker3-task1