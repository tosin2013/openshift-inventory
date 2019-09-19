OpenShift Inventory Generator
=========

This role will create an OpenShift v3.11 inventory for different type of deployment .

Requirements
------------
- Ansible

Role Variables
--------------

| Variables                     | Required | Default                                      | Description                                                                                                                     |
|-------------------------------|----------|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| deployment_type               | X        | standard                                          | Defines the deployment type.                                                                                                    |
| inventory_destination         | X        | ~/workspace/openshift-inventory/inventory                                         | Defines destination of inventory file
| preappend_host_name         | X        | ocp-                                      | preappend ocp-  to the beginning of computer name example ocp1-master01.example.com                                                                                       |
| instances                     | X        |                                              | Defines the name of master, infra, nodes and load  load balancers deployed on system. The name automatically increments 01,02.  |
| - name                        | X        | - master - infra - node - lb                 | Defines the default name of the machine                                                                                         |
| - cluster_group               | X        |  - masters - infra - nodes - lbs             | Defines the cluster group for the enviornment. The is used to group the machines in the inventory.                              |
| - qty                         | X        | - 1 - 2 - 2 - 1                              | Defines the number of each vm in the inventory file                                                                             |
| domain                        | X        | example.com                                  | The default domain for the OpenShift Cluster                                                                                    |
| glusterfs                     | X        | True                                         | Red Hat Gluster Storage used for converged mode for persistent storage  and dynamic provisioning for storage.                   |
| htpasswd_file_path            | X        | /home/example/openshift-ansible/passwordFile | Path to htpasswd file                                                                                                           |
| openshift_release             | X        | 3.11                                         | OpenShift 3.11 release                                                                                                          |
| openshift_image_tag           | X        | v3.11.141                                    | Specify an exact container image tag to install or configure.                                                                   |
| openshift_pkg_version         | X        | -3.11.141                                    | Specify an exact rpm version to install or configure.                                                                           |
| openshift_deployment_type     | X        | openshift-enterprise                         |                                                                                                                                 |
| rhsm_username                 | X        |                                              | Red Hat user name required for openshift-enterprise                                                                             |
| rhsm_password                 | X        |                                              | Red Hat password required for  openshift-enterprise                                                                             |
| ssh_username                  | X        |                                              | Default usernmae to ssh into machines                                                                                           |
| glusterfs_block_host_vol_size |          | 190                                          | Sets the size of the gluster block storage if enabled                                                                           |
| metrics_storage_volume_size   |          | 10Gi                                         | Sets the size of metrics if enabled.                                                                                            |
| Features                      |          |                                              |                                                                                                                                 |
| enable_firewalld              |          | True                                         | Set to true to use firewalld instead of the default iptables.                                                                   |
| enable_openshift_operators    |          | True                                         | Enable Operator Lifecycle Manager (OLM)                                                                                         |
| enable_metrics                |          | True                                         | Enable cluster metrics during cluster installation.                                                                             |
| enable_logging                |          | False                                        | Enable cluster logging during cluster installation                                                                              |
| enable_prometheous_operator   |          | False                                        |                                                                                                                                 |
Dependencies
------------


Example Playbook
----------------
**Minimal**
```
- hosts: localhost
  gather_facts: no
  vars:
    ansible_connection: local
    remote_user: root
    deployment_type: minimal
    inventory_destination: inventory.3.11.rhel.minimal
    preappend_host_name: ocp-
    instances:
      - name: master
        cluster_group: masters
        qty: 1
      - name: infra
        cluster_group: infra
        qty: 2
      - name: node
        cluster_group: nodes
        qty: 2
      - name: lb
        cluster_group: lbs
        qty: 0
    domain: example.com # do not put a . in front of domain
    glusterfs: False
    htpasswd_file_path: "/home/example/openshift-ansible/passwordFile"
    openshift_release: "3.11"
    openshift_image_tag: v3.11.141
    openshift_pkg_version: -3.11.141
    openshift_deployment_type: openshift-enterprise
    ssh_username: example
    rhsm_username: "example"
    rhsm_password: "StrongPa22"
    enable_firewalld: True
  tasks:
    - include_role:
        name: openshift-inventory
```
**Minimal Container Native Storage**
```
- hosts: localhost
  gather_facts: no
  vars:
    ansible_connection: local
    remote_user: root
    deployment_type: minimal-cns
    inventory_destination: inventory.3.11.rhel.minimal.gluster
    preappend_host_name: ocp-
    instances:
      - name: master
        cluster_group: masters
        qty: 1
      - name: infra
        cluster_group: infra
        qty: 2
      - name: node
        cluster_group: nodes
        qty: 2
      - name: lb
        cluster_group: lbs
        qty: 0
    domain: lab.example # do not put a . in front of domain
    glusterfs: True
    glusterfs_block_host_vol_size: 190
    htpasswd_file_path: "/home/example/openshift-ansible/passwordFile"
    openshift_release: "3.11"
    openshift_image_tag: v3.11.141
    openshift_pkg_version: -3.11.141
    openshift_deployment_type: openshift-enterprise
    ssh_username: example
    rhsm_username: "example"
    rhsm_password: "StrongPa22"
    enable_firewalld: True
  tasks:
    - include_role:
        name: openshift-inventory
```

**OpenShift Container Native Storage**
```
- hosts: localhost
  gather_facts: no
  vars:
    ansible_connection: local
    remote_user: root
    deployment_type: standard
    inventory_destination: inventory.3.11.rhel.gluster
    preappend_host_name: ocp-
    instances:
      - name: master
        cluster_group: masters
        qty: 1
      - name: infra
        cluster_group: infra
        qty: 2
      - name: node
        cluster_group: nodes
        qty: 2
      - name: lb
        cluster_group: lbs
        qty: 0
    domain: example.com # do not put a . in front of domain
    glusterfs: True
    htpasswd_file_path: "/home/example/openshift-ansible/passwordFile"
    openshift_release: "3.11"
    openshift_image_tag: v3.11.141
    openshift_pkg_version: -3.11.141
    openshift_deployment_type: openshift-enterprise
    ssh_username: example
    rhsm_username: "example"
    rhsm_password: "StrongPa22"
    enable_firewalld: True
    glusterfs_block_host_vol_size: 190
    enable_openshift_operators: True
    enable_logging: False
    enable_prometheous_operator: False
    enable_metrics: True
    metrics_storage_volume_size: 10Gi
  tasks:
    - include_role:
        name: openshift-inventory
```
License
-------

GPLv3

Author Information
------------------
This role was created in 2019 by [Tosin Akinosho](http://github.com/tosin2013)
