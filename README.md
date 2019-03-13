# update_rhel7_ami
================

**This role is still under active development.**

Based on a RHEL7 AMI of your choosing, creates a t2.micro instance, updates packages, reboots, and applies the Mindpointgroup RHEL7 STIG remediation role.

Requirements
------------

RHEL 7 or CentOS 7 - Other versions are not supported.

`Ansible 2.4` or greater

`pip`

`boto`

`boto3`

`passlib` >= 1.5 on the control node (1.6.5 is available in RHEL and CentOS as `python-passlib`)

`jmespath` on the control node (available in RHEL and CentOS as `python2-jmespath`)

Group Variables
--------------

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `ami_id` | `ami-0b500ef59d8335eee` | Free AWS RedHat 7 AMI        |
| `access_key` | `FAKEKEYFAKEKEY`  | AWS access key       |
| `secret_key` | `yes`  | AWS secret key      |
| `url` | apigateway.us-east-2.amazonaws.com  | us east api gateway |
| `region` | `us-east-2` | region for ami to exist in |
| `security_group` | `FAKE` | Security group for instance to run in. Does not create a group. Default does not work |
| `vpcsubnet` | `subnet-FAKE` | VPCsubnet instance should exist in |
| `zone` | us-east-2a | Zone server should exist in |
| `operatingsystem` | `Rhel7` | Sets preface name of new AMI. |
| `keypairname` | temp | AWS defined keypair to use |
| `ec2_server_pem_path` | `/home/administrator/temp.pem` | path to pem file on ansible server |
| `ec2_server_ansible_ssh_user` | `ec2-user` | user to connect to AWS instance as |

Dependencies
------------

None

Example Playbook
----------------

- name: provision an ec2 instance from ami
  hosts: localhost
  roles:
    - provision-ec2

- name: update an ec2 instance
  hosts: aws
  become: yes
  roles:
    - update-ec2
    - RHEL7-STIG

- name: make ami from ec2 instance
  hosts: localhost
  roles:
    - update-ami

License
-------

MIT
