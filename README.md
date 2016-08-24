[![Build Status](https://travis-ci.org/DemonTsai/openstack-environment.svg?branch=master)](https://travis-ci.org/DemonTsai/openstack-environment)

Role Name
=========

Deploy OpenStack basic environment with Ansible.

Requirements
------------

None

Role Variables
--------------
```
os_node_role
os_node_hostname
manage_ip
reboot_time
os_node_list
  - os_node
    os_node_ip
os_node_nic_list
  - nic_config_tmpl
  - nic_name
  - nic_ip
  - nic_netmask
  - nic_gateway
  - nic_dns
```

Dependencies
------------

None

Example Playbook
----------------

```
  - hosts: os-controller
    remote_user: openstack
    roles:
      - role: openstack-environment
        os_node_role: controller
        os_node_hostname: controller
        manage_ip: 172.16.50.129
        reboot_time: 25
        ntp_server: time.stdtime.gov.tw
        rabbit_user: rabbitos
        rabbit_pass: rabbit_pass
        mysql_root_password: db_pass

        os_node_list:
          - os_node: controller
            os_node_ip: 172.16.50.129
          - os_node: network
            os_node_ip: 172.16.50.130
          - os_node: compute
            os_node_ip: 172.16.50.131

        os_node_nic_list:
          - nic_config_tmpl: nic_static
            nic_name: eth0
            nic_ip: 172.16.50.129
            nic_netmask: 255.255.255.0
            nic_gateway: 172.16.50.2
            nic_dns: 8.8.8.8

          - nic_config_tmpl: nic_manual
            nic_name: eth1

          - nic_config_tmpl: nic_tunnel
            nic_name: eth2

  - hosts: os-network
    remote_user: openstack
    roles:
      - role: openstack-environment
        os_node_hostname: network
        manage_ip: 172.16.50.130
        reboot_time: 25
        ntp_server: controller

        os_node_list:
          - os_node: controller
            os_node_ip: 172.16.50.129
          - os_node: network
            os_node_ip: 172.16.50.130
          - os_node: compute
            os_node_ip: 172.16.50.131

        os_node_nic_list:
          - nic_config_tmpl: nic_static
            nic_name: eth0
            nic_ip: 172.16.50.130
            nic_netmask: 255.255.255.0
            nic_gateway: 172.16.50.2
            nic_dns: 8.8.8.8

          - nic_config_tmpl: nic_manual
            nic_name: eth1

          - nic_config_tmpl: nic_tunnel
            nic_name: eth2
            nic_ip: 10.0.0.1
            nic_netmask: 255.255.255.0

  - hosts: os-compute
    remote_user: openstack
    roles:
      - role: openstack-environment
        os_node_hostname: compute
        manage_ip: 172.16.50.131
        reboot_time: 25
        ntp_server: controller

        os_node_list:
          - os_node: controller
            os_node_ip: 172.16.50.129
          - os_node: network
            os_node_ip: 172.16.50.130
          - os_node: compute
            os_node_ip: 172.16.50.131

        os_node_nic_list:
          - nic_config_tmpl: nic_static
            nic_name: eth0
            nic_ip: 172.16.50.131
            nic_netmask: 255.255.255.0
            nic_gateway: 172.16.50.2
            nic_dns: 8.8.8.8

          - nic_config_tmpl: nic_tunnel
            nic_name: eth2
            nic_ip: 10.0.0.2
            nic_netmask: 255.255.255.0
```

License
-------

Apache

Author Information
------------------

demontsai@gmail.com

