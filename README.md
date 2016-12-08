aws-vpc-scenario2
=================

Ansible v2 role to create a multi-AZ public/private zone VPC with NAT gateways (supports VPC removal as well as buildout). Amazon Web Services describes this as a "Scenario 2" VPC.

More info: http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html

This role provides:

- a VPC across two AZs
- a public and a private subnet in each AZ
- a RDS db subnet group using the private subnets
- NAT Gateways for allowing Internet access from private subnets
- relatively secure network ACLs for public and private subnets
- two relatively secure VPC security groups for public or private instances
- consistent and ubiquitous tagging of resources

See this [blog post](https://randops.org/2016/12/08/using-ansible-roles-to-create-a-scenario2-vpc-in-aws/) for more details.

Requirements
------------

- Ansible v2.2.x (makes use of newer cloud modules)
- AWS account and credentials (OS-level environment variables were used for secrets)
- AWS CLI and Boto

Role Variables
--------------

Adjust vars/main.yml to contain variable values that make sense for your deployment

Plays also require three extra vars to be supplied on invocation via ansible-playbook:

- remote_cidr - network CIDR from which you will access the public subnets
- environment - used for resource tags and resource names
- vpc_status -  triggers either creation or deletion of VPC

Dependencies
------------

- None

Example Playbook
----------------

Note: use the appropriate real-world value for your remote_cidr

Role play:

~~~~
  ---
  - name: Create a Scenario 2 VPC with public and private subnets in two AZs
    hosts: localhost
    connection: local
    gather_facts: yes

    roles:
    - { role: rcrelia.aws-vpc-scenario2, remote_cidr: 1.2.3.4/32, aws_env: dev01, vpc_status: create }
~~~~

Exact CLI syntax:

~~~~
  $ ansible-playbook -i tests/inventory tests/site.yml -e "remote_cidr=1.2.3.4/32 aws_env=dev01 vpc_status=create"
  $ ansible-playbook -i tests/inventory tests/site.yml -e "remote_cidr=1.2.3.4/32 aws_env=dev01 vpc_status=delete"
~~~~


License
-------

MIT

Author Information
------------------

Rick Crelia
- [https://twitter.com/rcrelia](https://twitter.com/rcrelia)
- [https://randops.org](https://randops.org)

Please send support requests, bug reports or feature proposals via Github: [https://github.com/rcrelia/aws-vpc-scenario2] (https://github.com/rcrelia/aws-vpc-scenario2)
