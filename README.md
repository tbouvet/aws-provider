# aws-provider

To create a new __AWS EC2 stack__, run the playbook `provisioning-stack.yml`
		
This playbook needs one parameter:

- **config_dir**: folder which contains yaml files with EC2 parameters

This provider has a dependency with the *core* module to install all *docker components*.

> If **proxy** is needed to connect to AWS, export **proxy** parameters in environment variables (http\_proxy, https\_proxy, no\_proxy).

> To use EC2 dynamic inventory, set environment variables: ANSIBLE\_INVENTORY and EC2\_INI\_PATH (python module is in *scripts* folder).


__Example__:

```
ansible-playbook provisioning-stack.yml -e "config_dir=/opt/inputs"
```

__EC2 configuration file example__:

```yaml
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
    - proto: tcp
      ports:
      - 8080
      cidr_ip: 0.0.0.0/0
      rule_desc: allow all on port 8080
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

```