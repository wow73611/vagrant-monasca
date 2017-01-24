#monasca-events-api
Installs the [monasca-events-api](https://github.com/hpcloud-mon/monasca-events-api) in a virtualenv.
Monasca Events API is part of the [Monasca](https://wiki.openstack.org/wiki/Monasca) project.

##Requirements
virtualenv must be installed.

- kafka_hosts - comma seperate list of kafka hosts
- mysql_host
- mysql_user
- mysql_password


Optionally if needed
- run_mode: One of Deploy, Stop, Install, Configure or Start. The default is Deploy which will do Install, Configure, then Start.

##Example Playbook

    hosts: monasca
    sudo: yes
    roles:
      - {role: monasca-events-api,
         tags: [events-api]}

##License
Apache

##Author Information
Sam Kirsch
Monasca Team email monasca@lists.launchpad.net
