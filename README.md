# aws-provider

To create a new __AWS EC2 stack__, run the playbook `create.yml`
		
This playbook needs 2 parameters:

- **input_dir**: folder which contains yaml files with EC2 parameters
- **output_dir**: folder which contains yaml files with EC2 parameters


> If **proxy** is needed to connect to AWS, export **proxy** parameters in environment variables (http\_proxy, https\_proxy, no\_proxy).


__Example__:

```
ansible-playbook create.yml -e "input_dir=/opt/input output_dir=/opt/output" -i inventory/
```

__EC2 configuration file example__:

```yaml
instances: 3
params:
  instance_type: "t2.micro"
  ami_id: "ami-f90a4880"
  vpc_id: "vpc-432eb90"
  region: "eu-west-1"
  security_groups:
  - name: group1
    rules:
    - proto: tcp
      ports:
      - 22
      cidr_ip: 0.0.0.0/0
      rule_desc: allow all on port 22
  - name: group2
    rules:
    - proto: tcp
      ports:
      - 80
      cidr_ip: 0.0.0.0/0
      rule_desc: allow all on port 80
  - name: swarm
    rules:
    - proto: tcp
      ports:
      - 2377
      cidr_ip: 0.0.0.0/0
      rule_desc: cluster management communications
    - proto: tcp
      ports:
      - 7946 
      cidr_ip: 0.0.0.0/0
      rule_desc: communication among nodes
    - proto: udp
      ports:
      - 7946 
      cidr_ip: 0.0.0.0/0
      rule_desc: communication among nodes
    - proto: udp
      ports:
      - 4789 
      cidr_ip: 0.0.0.0/0
      rule_desc: overlay network traffic
volumes:
- path: /data
  params:
    device_name: xvdf
    volume_type: gp2
    volume_size: 10
    delete_on_termination: true
labels:
  label1: value1
```

## EC2 Parameters for Instances

All variables which can be overridden as in table below (under **params** tag).

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `instance_type` |  True |   | Instance type to use for the instance, see http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html |
| `ami_id` |  True |  | AMI ID to use for the instance |
| `vpc_id` |   |  | VPC ID to use for the security groups |
| `vpc_subnet_id` |   |  | vpc_subnet ID to use for the instance |
| `assign_public_ip` |   | True | when provisioning within vpc, assign a public IP address |
| `security_groups` |  |  | To define new security groups, see EC2 parameters for Security Groups |
| `security_groups_ids` |  |  | List of existing security groups ids |
| `instance_tags` |  |  | List of tags to add for the instance |


## EC2 parameters for Security Groups 

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `name` |  True |   | Security group's name |
| `rules` |  True |  | See https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html |


## EC2 parameters for Volumes 

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `path` |  True |   | Mount point's name |
| `params` |  True |  | See https://docs.ansible.com/ansible/latest/modules/ec2_vol_module.html |

## EC2 parameters for Placement Group 

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `name` |  True |   | Placement Group's name |
| `strategy` |  |  | See https://docs.ansible.com/ansible/latest/modules/ec2_placement_group_module.html |

## EC2 parameters for Labels (tags) 

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `key` |  True |   | Label's key / value |
