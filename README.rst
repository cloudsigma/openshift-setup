OpenShift Setup
===============
This script helps you setup OpenShift v3; which is the latest version at the moment of writing; in 3 or more virtual guests.

To start using this script, you need to have:

* CenOS 7 x86_64 (3x)
* 2 NICs (public and private)
* All hosts should have DNS A records pointing at them
* A wildcard A record pointing at the host with the service router (*.apps.cloudsigma.com)

To start installing, execute the following on each server destined to be an OpenShift master/node: 

.. code:: bash

    yum install -y git
    git clone https://github.com/cloudsigma/openshift-setup.git
    cd openshift-setup
    GIT_WORK_TREE="/" git checkout -f

Then, the master server, take your time to edit ``/root/.os-setuprc``. This file contains the settings for the openshift
setup scripts. It is essential to write the correct values there.

Now, just run the scripts, one by one:

.. code:: bash

    # These are to be run in all hosts; master and nodes
    origin-all-00.0-prerequisite_installations
    origin-all-00.1-docker-configure
    origin-all-00.2-docker-setup_persistent_storage

    # These are to be ran on the master only
    origin-master-00.3-ssh-setup

    # Before running this one, take your time to rename /etc/ansible/hosts.example to /etc/ansible/hosts and edit it carefully.
    # It is catastrophic to fail to enter the right values here. Please, read the following for reference on the matter.
    # https://docs.openshift.org/latest/install_config/install/advanced_install.html#single-master
    origin-master-01.0-ansible-setup

    # Once finished, continue with these.
    origin-master-02.0-authentication-setup
    origin-master-03.0-registry-deploy
    origin-master-04.0-router-deploy

