# ansible-python-monasca-events

Installs the [python-monasca-events](https://github.com/hpcloud-mon/python-monasca-events) part of the [Monasca](https://wiki.openstack.org/wiki/Monasca) project.

The role install the cli into a virtualenv on the box.

## Requirements
virtualenv must be installed on the system. 

## Role Variables

monasca_virtualenv_dir: Virtual environment where the events cli is installed
pip_conf_dir: The config location for python index package. 

`defaults/main.yml`

##License
Apache

##Author Information
Angelo Mendonca
Monasca Team email monasca@lists.launchpad.net