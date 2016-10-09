Role Name
=========

This role will provision a specified number of EC2 instances and install Citilabs CUBE on them.

Requirements
------------

Check https://versent.atlassian.net/wiki/display/TRAN/High+Level+Solution+Design#HighLevelSolutionDesign-Preparation for requirements.

Role Variables
--------------
```
ami_id: AMI ID to build from, accepted as command line param
ec2_key_name: temporary key to use to decrypt the local admin password. gets deleted after deployment
ec2_instance_size: # AWS instance type to deploy, accepted as command line param
ec2_instance_tags: Tag that gets applied to new instances
instances_count: number of instances to deploy, accepted as command line param
vpc_subnet_id: name of VPC subnet to put instances in to
security_group: name of security group to apply to new instances
awsregion: name of AWS Region to deploy new instances to
awsbucket: name of S3 bucket with setup files
awsbucketfolderpath: name of folder in S3 bucket
oupath: DN of Active Directory Organisational Unit to join to 
domainname: name of domain to join
cube_iam_role: name of IAM role applied to CUBE instances
awsproxy: proxy address and port for ec2config service
cube_setup_exe: name of cube setup file
enable_monitoring: specify if EC2 monitoring should be enabled
ec2_data_disk_size: size in GB of data disk on EC2 instances
```
Dependencies
------------

AMI needs to be previously built and ID specified in default_vars

Example Playbook
----------------
```
- name: Provision EC2 instances
  gather_facts: false
  hosts:
  - 127.0.0.1
  connection: local
  force_handlers: True
  roles:
  - cube

- name: Configure CUBE instances
  gather_facts: false
  hosts:
  - cube
  tasks:
  - include: /container/ansible/roles/cube/tasks/configure-ec2.yml

- name: Cleanup Docker Container
  gather_facts: false
  hosts:
  - 127.0.0.1
  connection: local
  tasks:
  - include: /container/ansible/roles/cube/tasks/cleanup.yml
```
License
-------

BSD

Author Information
------------------

David O'Brien, david.obrien@versent.com.au
