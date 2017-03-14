#monasca-api
Installs the [monasca-api](https://github.com/stackforge/monasca-api) part of the [Monasca](https://wiki.openstack.org/wiki/Monasca) project.

##Requirements
- api_region
- influxdb_url
- influxdb_user
- influxdb_password
- kafka_hosts - comma separated list of host:port combinations
- keystone_host
- keystone_admin
- keystone_admin_password
- hibernate_support_enabled - Hibernate is used for DB connection if set for true. MySql with raw queries is used when flag set to false.
- database_host - SSL will be used if available
- database_user
- database_password

##Optional parameters
- keystone_admin_project - defaults to empty value
- monasca_api_client_port - the port the API listens on, default is 8070
- monasca_api_bind_host - if set, the port the API listens on is bound to this host or ip address
- monasca_log_level - Log level for the API log. Default to WARN
- monasca_wait_for_period - The time in seconds for how long to wait for API's port to be available after starting it. Default is 10 seconds.
- run_mode - One of Deploy, Stop, Install, Start, or Use. The default is Deploy which will do Install, Configure, then Start.


The keystore and truststore's are jks files and created by command such as these examples:

    # Change from pem to pkcs12 format
    openssl pkcs12 -export -in orig.pem -inkey orig.key -out new.p12 -name fqdn -chain -CAfile cacert.pem -password pass:password
    # Create the jks keystore
    keytool -importkeystore -deststorepass password -destkeystore ./keystore.jks -srckeystore new.p12 -srcstoretype PKCS12 -srcstorepass password

##Example Playbook

    hosts: monasca
    sudo: yes
    roles:
      - {role: tkuhlman.monasca-api,
         influxdb_user: "{{api_influxdb_user}}",
         influxdb_password: "{{api_influxdb_password}}",
         mysql_user: "{{api_mysql_user}}",
         mysql_password: "{{api_mysql_password}}",
         tags: [api]}

##License
Apache

##Author Information
Tim Kuhlman
Monasca Team email monasca@lists.launchpad.net
