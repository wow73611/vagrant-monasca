#monasca-events-transform
Installs the [monasca-events-transform](https://github.com/hpcloud-mon/monasca-events-transform) part of the [Monasca](https://wiki.openstack.org/wiki/Monasca) project.

##Requirements
- mysql_node - SSL will be used if available
- mysql_user
- mysql_password
- mysql_db
- zoo_node - comma separated list of zookeeper host:port combinations
- kafka_node

##Optional parameters
- monasca_log_level - Log level for the API log. Default to WARN
- run_mode - One of Deploy, Stop, Install, Start, or Use. The default is Deploy which will do Install, Configure, then Start.



##Example Playbook

    hosts: monasca
    sudo: yes
    roles:
      - {role: monasca-events-transform}