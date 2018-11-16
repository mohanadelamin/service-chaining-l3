- This is an OpenStack heat template that deploys Palo Alto Networks PA-VM in a service chain as per the following diagram.
![Topology](https://raw.githubusercontent.com/mohanadelamin/service-chaining-l3/master/other/topology.png)

- The template deploy all the component in the blue box plus the related configuration for the traffic steering on contrail side (i.e. Service Template, Service Instance, and policy)
- This template is based on Palo Alto Networks official [openstack templates](https://github.com/PaloAltoNetworks/openstack-templates)
- In this template, the PA-VM will contact Panorama management server for the configuration after the bootstrapping. If Panorama is not needed in the setup, then modify the bootstrapping package. (Check step 2 below for more information)
- The bootstrapping package is pushed to the PA-VM via user-data and not through Personality file injection.

## Usage:
1. Modify the environment yaml file (service_chaining_env_l3.yaml) with your information (i.e. web and DB image, VM names, and IP subnets).

2. Modify the bootstrapping package to match your requirements. Check Palo Alto Networks [Offical Guides](https://www.paloaltonetworks.com/documentation/81/virtualization/virtualization/bootstrap-the-vm-series-firewall) for more details. (Note: the bootstrapping configuration file bootstrap.xml use the default username and password admin/admin)

3. compress the bootstrapping package using the following command.
```bash
# tar -cvzf bts.tgz config/ license/ software content
config/
config/bootstrap.xml
config/init-cfg.txt
license/
license/authcodes
software/
content/
```

4. Upload the bts package to a web server, then update the URL on the environment file.
5. Finally, deploy the stack using the following command:
```bash
# openstack stack create -e ./service_chaining_env_l3.yaml -t service_chaining_template_l3.yaml stack1
```

## Example output

- Create the stack
```bash
 # openstack stack create -e ./service_chaining_env_l3.yaml -t service_chaining_template_l3.yaml stack1
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| id                  | 9116d3f7-55cf-4409-9ffb-6e00747c8475 |
| stack_name          | stack1                               |
| description         | No description                       |
| creation_time       | 2018-11-16T13:59:11                  |
| updated_time        | None                                 |
| stack_status        | CREATE_IN_PROGRESS                   |
| stack_status_reason |                                      |
+---------------------+--------------------------------------+
```

- List the stack
```bash
# openstack stack list
+--------------------------------------+------------+-----------------+---------------------+--------------+
| ID                                   | Stack Name | Stack Status    | Creation Time       | Updated Time |
+--------------------------------------+------------+-----------------+---------------------+--------------+
| 9116d3f7-55cf-4409-9ffb-6e00747c8475 | stack1     | CREATE_COMPLETE | 2018-11-16T13:59:11 | None         |
+--------------------------------------+------------+-----------------+---------------------+--------------+
```

- Check the stack information
```bash
# openstack stack show stack1
+-----------------------+--------------------------------------------------------------------------------------------------------------------------+
| Field                 | Value                                                                                                                    |
+-----------------------+--------------------------------------------------------------------------------------------------------------------------+
| id                    | 9116d3f7-55cf-4409-9ffb-6e00747c8475                                                                                     |
| stack_name            | stack1                                                                                                                   |
| description           | No description                                                                                                           |
| creation_time         | 2018-11-16T13:59:11                                                                                                      |
| updated_time          | None                                                                                                                     |
| stack_status          | CREATE_COMPLETE                                                                                                          |
| stack_status_reason   | Stack CREATE completed successfully                                                                                      |
| parameters            | IPAM_addr_from_start_true: 'True'                                                                                        |
|                       | IPAM_ip_prefix_left: 172.16.201.0                                                                                        |
|                       | IPAM_ip_prefix_len_left: '24'                                                                                            |
|                       | IPAM_ip_prefix_len_mgmt: '24'                                                                                            |
|                       | IPAM_ip_prefix_len_right: '24'                                                                                           |
|                       | IPAM_ip_prefix_mgmt: 172.16.200.0                                                                                        |
|                       | IPAM_ip_prefix_right: 172.16.202.0                                                                                       |
|                       | OS::project_id: 7513f66f570247f39c6168ed4466cbb0                                                                         |
|                       | OS::stack_id: 9116d3f7-55cf-4409-9ffb-6e00747c8475                                                                       |
|                       | OS::stack_name: stack1                                                                                                   |
|                       | SI_fqdn_name: default-domain:admin:PAN_NGFW_SVM_Instance_L3                                                              |
|                       | SI_name: PAN_NGFW_SVM_Instance_L3                                                                                        |
|                       | ST_flavor: vm-100                                                                                                        |
|                       | ST_image_name: PA-VM-KVM-8.1.3                                                                                           |
|                       | ST_interface_type_left: left                                                                                             |
|                       | ST_interface_type_mgmt: management                                                                                       |
|                       | ST_interface_type_right: right                                                                                           |
|                       | ST_name: PAN_NGFW_SVM_template_L3                                                                                        |
|                       | ST_service_mode: in-network                                                                                              |
|                       | ST_service_type: firewall                                                                                                |
|                       | ST_version: '2'                                                                                                          |
|                       | direction: <>                                                                                                            |
|                       | domain: default-domain                                                                                                   |
|                       | dst_port_end: '-1'                                                                                                       |
|                       | dst_port_start: '-1'                                                                                                     |
|                       | flavor: m1.tiny                                                                                                          |
|                       | left_vm_image: CirrOS-0.4.0                                                                                              |
|                       | left_vm_name: WEB_VM_L3                                                                                                  |
|                       | left_vnet: WEB_net                                                                                                       |
|                       | left_vnet_fqdn: default-domain:admin:WEB_net                                                                             |
|                       | mgmt_vnet: Management_net                                                                                                |
|                       | policy_fqdn_name: default-domain:admin:PAN_NGFW_SVM_policy-L3                                                            |
|                       | policy_name: PAN_NGFW_SVM_policy-L3                                                                                      |
|                       | port_tuple_name: port_tuple_L3                                                                                           |
|                       | protocol: any                                                                                                            |
|                       | right_vm_image: CirrOS-0.4.0                                                                                             |
|                       | right_vm_name: DB_VM_L3                                                                                                  |
|                       | right_vnet: DB_net                                                                                                       |
|                       | right_vnet_fqdn: default-domain:admin:DB_net                                                                             |
|                       | route_target: target:64512:20000                                                                                         |
|                       | service_vm_name: PAN_NGFW_SVM_L3                                                                                         |
|                       | simple_action: pass                                                                                                      |
|                       | src_port_end: '-1'                                                                                                       |
|                       | src_port_start: '-1'                                                                                                     |
|                       |                                                                                                                          |
| outputs               | - description: IP address of the Left VM instance                                                                        |
|                       |   output_key: Left_VM_ip                                                                                                 |
|                       |   output_value: 172.16.201.3                                                                                             |
|                       | - description: IP address of the Right VM instance                                                                       |
|                       |   output_key: Right_VM_ip                                                                                                |
|                       |   output_value: 172.16.202.3                                                                                             |
|                       | - description: Name of the Right VM instance                                                                             |
|                       |   output_key: Right_VM_name                                                                                              |
|                       |   output_value: DB_VM_L3                                                                                                 |
|                       | - description: Name of the PAN Sevice instance                                                                           |
|                       |   output_key: Pan_NGFW_Svm_instance_name                                                                                 |
|                       |   output_value: PAN_NGFW_SVM_L3                                                                                          |
|                       | - description: Name of the Left VM instance                                                                              |
|                       |   output_key: Left_VM_name                                                                                               |
|                       |   output_value: WEB_VM_L3                                                                                                |
|                       | - description: IP address of the network interfaces on Pan_NGFW_Svm_instance                                             |
|                       |   output_key: Pan_NGFW_Svm_instance_networks                                                                             |
|                       |   output_value:                                                                                                          |
|                       |     1a7a3562-ec47-4acd-adb3-2eca2e1c894a:                                                                                |
|                       |     - 172.16.201.5                                                                                                       |
|                       |     20028494-fbb1-482a-ac60-72115ed2862f:                                                                                |
|                       |     - 172.16.202.5                                                                                                       |
|                       |     8312104a-55be-4264-80ea-79025b12ac08:                                                                                |
|                       |     - 172.16.200.4                                                                                                       |
|                       |     DB_net:                                                                                                              |
|                       |     - 172.16.202.5                                                                                                       |
|                       |     Management_net:                                                                                                      |
|                       |     - 172.16.200.4                                                                                                       |
|                       |     WEB_net:                                                                                                             |
|                       |     - 172.16.201.5                                                                                                       |
|                       |                                                                                                                          |
| links                 | - href: http://10.193.86.170:8004/v1/7513f66f570247f39c6168ed4466cbb0/stacks/stack1/9116d3f7-55cf-4409-9ffb-6e00747c8475 |
|                       |   rel: self                                                                                                              |
|                       |                                                                                                                          |
| disable_rollback      | True                                                                                                                     |
| parent                | None                                                                                                                     |
| tags                  | null                                                                                                                     |
|                       | ...                                                                                                                      |
|                       |                                                                                                                          |
| stack_user_project_id | 58ca4026a5974cc29832f677e0871529                                                                                         |
| capabilities          | []                                                                                                                       |
| notification_topics   | []                                                                                                                       |
| timeout_mins          | None                                                                                                                     |
| stack_owner           | None                                                                                                                     |
+-----------------------+--------------------------------------------------------------------------------------------------------------------------+
```