# cv ansible

Ansible code for deploying a static html cv to nginx on debian/ubuntu.

## What does it do?

1. Installs some basic security (iptables to prevent brute force ssh)
1. Installs nginx (and disables the default site)
1. Sets up an nginx site for our website
1. Downloads a release tarball from github and symlinks the contents to nginx webroot

## Local and remote playbooks?

The local playbook is designed to be used via `ansible-pull` and so points the play at localhost.
The remote playbook is for testing the roles remotely and so determines the host using an aws tag.
You will need to have your aws creds setup correctly on your host user's
`~/.aws/credentials` (or via bash vars) for the remote playbook to work.

## usage

remotely:

    ansible-playbook -i ./inventory/ec2.py --private-key [...] -u ubuntu remote-web.yaml

locally (via `ansible-pull`):

    ansible-pull \
       "local-web.yaml" \
        --only-if-changed \
        --directory "/opt/cv-web_ansible-pull-repo" \
        --url "https://bitbucket.org/sadams/cv-ansible.git" \
        --checkout "master"

## TODO

1. Log rotation for nginx logs
1. Maybe add a single, isolated playbook for setting up `ansible-pull` target machine which
can be directly downloaded and run during bootstrap (i.e. via AWS user data) to install things like log rotation without raw bash: https://github.com/ansible/ansible-examples/blob/master/language_features/ansible_pull.yml

## Associated repos

To build the servers:
https://github.com/sadams/cv-tf

The source code for generating the html/css/js that makes up the release package:
https://github.com/sadams/cv
