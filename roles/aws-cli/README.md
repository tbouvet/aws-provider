Role Name
=========

Role to create AWS credentials.

Requirements
------------

Any pre-requisites

Role Variables
--------------

- aws_region: 'eu-west-1'
- aws_access_key_id: 'YOUR_ACCESS_KEY_ID'
- aws_secret_access_key: 'YOUR_SECRET_ACCESS_KEY'

Dependencies
------------



Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

	- hosts: localhost
	  connection: local
	  gather_facts: false
	  tasks:
	  - include_role:
	       name: aws-cli
	    vars:
	      aws_region: "{{ lookup('env','AWS_REGION') }}"
	      aws_access_key_id: "{{ lookup('env','AWS_ACCESS_KEY_ID') }}"
	      aws_secret_access_key: "{{ lookup('env','AWS_SECRET_ACCESS_KEY') }}"
	      
License
-------

BSD

Author Information
------------------

