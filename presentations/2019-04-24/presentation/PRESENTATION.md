class: title, middle, center

# Deploying AWS EC2 with Ansible

---
## Introduction Speaker

### <i class="fas fa-user fa-fw"></i> Daniel Schier
#### <i class="fas fa-birthday-cake fa-fw"></i> Born before the internet
#### <i class="fas fa-home fa-fw"></i> Dresden, Germany
----
#### <i class="fas fa-building fa-fw"></i> profi.com AG - business solutions
#### <i class="fas fa-graduation-cap fa-fw"></i> Watched "IT Crowd"
#### <i class="fas fa-lightbulb fa-fw"></i> Inspired by these cool "Hacker guys" in TV
#### <i class="fas fa-hammer fa-fw"></i> Having fun coding, operating and managing IT
----
#### <i class="fab fa-twitter fa-fw"></i> daniel_wtd
#### <i class="fas fa-envelope fa-fw"></i> daniel@while-true-do.io

---

## Introduction Sponsor

### <i class="fas fa-building fa-fw"></i> profi.com AG - business solutions
#### <i class="fas fa-lightbulb fa-fw"></i> we make IT work
#### <i class="fas fa-home fa-fw"></i> Stresemannplatz 3, 01309 Dresden
----
#### <i class="fas fa-birthday-cake fa-fw"></i> 2000
#### <i class="fas fa-hard-hat fa-fw"></i> 66
#### <i class="fas fa-hammer fa-fw"></i> consulting for cloud, quality, security
----

#### <i class="fas fa-envelope fa-fw"></i> kontakt@proficom.de
#### <i class="fas fa-globe fa-fw"></i> www.proficom.de

---
## <i class="fas fa-table fa-fw"></i> Table of Contents

-   Overview Cloud Modules
-   Guidance
-   Setup
-   Provision
-   Configure
-   Demo

---
class: title, middle, center

## Overview Cloud Modules

Ansible can talk to (mostly) anything.

---
## <i class="fas fa-cloud fa-fw"></i> Overview Cloud Modules

You can find all of the [Cloud Modules](https://docs.ansible.com/ansible/latest/modules/list_of_cloud_modules.html)
online.

-   Amazon
-   Atomic
-   Azure
-   Cloudstack
-   Digital Ocean
-   Google
-   Heroku
-   Linode
-   Openstack

And many, many more.

---
class: title, middle, center

## Guidance

Let's see who can guide you on your journey.

---
## <i class="fas fa-book fa-fw"></i> Guidance

-   [Online AWS Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html)
-   Book: Ansible 2 Cloud Automation Cookbook
-   IRC: @freenode, #ansible, #ansible-aws

---
class: title, middle, center

## Setup

Let's setup the Ansible Environment.

---
## <i class="fas fa-wrench fa-fw"></i> Setup

You need a [virtualenv](https://virtualenv.pypa.io/en/latest/) to setup
[Ansible](https://www.ansible.com/) and
[Boto](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) via
[pip](https://pypi.org/project/pip/)

```
# Create and Activate
virtualenv .venv-aws
source .venv-aws/bin/activate

# Install Packages
pip install ansible boto3 botocore
```

---
## <i class="fas fa-wrench fa-fw"></i> Setup

Create your directory layout.

```
- AWS/
  |- inventories/
  |  |- aws/
  |  |  |- group_vars/
  |  |  |- host_vars/
  |  |  |- inventory
  |  |
  |  |- (aws2/)
  |
  |- (roles/)
  |
  |- (plays/)
  |
  |- site.yml

```

Depending on the ami, you want to use, you have to edit the invetory.

---
## <i class="fas fa-wrench fa-fw"></i> Setup

Define [Authentication](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html#authentication) details.

```
export AWS_ACCESS_KEY_ID='AK123'
export AWS_SECRET_ACCESS_KEY='abc123'
```

---
## <i class="fas fa-wrench fa-fw"></i> Setup

Prepare the playbook (site.yml).

```
- name: Provision AWS
  hosts: localhost
  connection: local
  gather_facts: false

  vars:
    - var: foo

  tasks:
    - name: bar

```

---
class: title, middle, center

## Provision

Let's interact with AWS and provision some stuff.

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Create a key: [ec2_key](https://docs.ansible.com/ansible/latest/modules/ec2_key_module.html)

```
- name: Create a keypair
  ec2_key:
    name: "{{ ec2_key_name }}"
    region: "{{ ec2_region }}"
  register: "key_info"
```

After creation, you have to save it locally.

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Create a VPC Net: [ec2_vpc_net](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_net_module.html)

```
- name: Create VPC Net
  ec2_vpc_net:
    name: "{{ ec2_vpc_name }}"
    cidr_block: "{{ ec2_vpc_cidr_block }}"
    region: "{{ ec2_region }}"
    tags:
      Name: "{{ ec2_vpc_name }}"
  register: "vpc_info"
```
---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Add a VPC Internet Gateway: [ec2_vpc_igw](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_igw_module.html)

```
- name: Create IGW
  ec2_vpc_igw:
    vpc_id: "{{ vpc_info.vpc.id }}"
    state: present
    region: "{{ ec2_region }}"
    tags:
      Name: "{{ ec2_igw_name }}"
  register: igw_info
```

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Add a VPC Subnet: [ec2_vpc_subnet](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_subnet_module.html)

```
- name: Create VPC Subnet
  ec2_vpc_subnet:
    region: "{{ ec2_region }}"
    vpc_id: "{{ vpc_info.vpc.id }}"
    cidr: "{{ ec2_sub_web_cidr }}"
    tags:
      Name: "{{ ec2_sub_web_name }}"
  register: "sub_info"
```

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Add a Routing Table: [ec2_vpc_route_table](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_route_table_module.html)

```
- name: Create Route Table
  ec2_vpc_route_table:
    vpc_id: "{{ vpc_info.vpc.id }}"
    region: "{{ ec2_region }}"
    routes:
      - dest: 0.0.0.0/0
        gateway_id: "{{ igw_info.gateway_id }}"
    subnets:
      - "{{ sub_info.subnet.id }}"
    tags:
      Name: "{{ ec2_rt_name }}"
  register: "rt_info"
```

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Add a security group: [ec2_group](https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html)

```
- name: Create Security Group
  ec2_group:
    name: "{{ ec2_sg_web_name }}"
    description: "{{ ec2_sg_web_desc }}"
    vpc_id: "{{ vpc_info.vpc.id }}"
    region: "{{ ec2_region }}"
    rules:
    - proto: tcp
      from_port: 80
      to_port: 80
      cidr_ip: 0.0.0.0/0
    - proto: tcp
      from_port: 22
      to_port: 22
      cidr_ip: 0.0.0.0/0
    - proto: icmp
      from_port: -1
      to_port:  -1
      cidr_ip: 0.0.0.0/0
  register: "sg_info"
```

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Add instances: [ec2_instance](https://docs.ansible.com/ansible/latest/modules/ec2_instance_module.html)

```
- name: Create instance (ec2_instance)
  ec2_instance:
    aws_access_key: "{{ ec2_access_key }}"
    aws_secret_key: "{{ ec2_secret_key }}"

    name: "{{ ec2_instance_name }}"
    key_name: "{{ ec2_key_name }}"
    region: "{{ ec2_region }}"
    vpc_subnet_id: "{{ sub_info.subnet.id }}"
    instance_type: t2.micro
    security_group: "{{ ec2_sg_web_name }}"
    network:
      assign_public_ip: true
    image_id: "{{ ec2_instance_image }}"
    tags:
      Usage: webserver
  register: "instance_info"
```

---
## <i class="fas fa-cloud-upload-alt fa-fw"></i> Provision

Add instances: [ec2](https://docs.ansible.com/ansible/latest/modules/ec2_module.html)

```
- name: Create instance (ec2)
  ec2:
    aws_access_key: "{{ ec2_access_key }}"
    aws_secret_key: "{{ ec2_secret_key }}"

    key_name: "{{ ec2_key_name }}"
    region: "{{ ec2_region }}"
    instance_type: t2.micro
    image: "{{ ec2_instance_image }}"
    group: "{{ ec2_sg_web_name }}"
    vpc_subnet_id: "{{ sub_info.subnet.id }}"
    assign_public_ip: true
    exact_count: "{{ ec2_instance_count }}"
    instance_tags:
      Usage: webserver
    count_tag:
      Usage: webserver
  register: ec2
```

---
class: title, middle, center

## Configure

After provisioning your infrastructure, you can configure it.

---
## <i class="fas fa-cogs fa-fw"></i> Configure

You have to prepare the playbook:

```
- name: Configure Instance(s)
  hosts: launched
  become: true
  tasks:
    - name: foo
```

---
## <i class="fas fa-cogs fa-fw"></i> Configure

And add some tasks:

```
- name: Install httpd
  package:
    name: httpd
    state: present
- name: Start and Enable httpd
  service:
    name: httpd
    state: started
    enabled: yes
```

---
class: title, middle, center

## Demo

Let's see this in action. Running the [site.yml](./material/site.yml)

---
## <i class="fas fa-hands-helping fa-fw"></i> Propaganda

#### Ansible

-   [ansible website](https://www.ansible.com)
-   [ansible community](https://www.ansible.com/community)
-   [ansible github](https://github.com/ansible)
-   [meetup dresden](https://www.meetup.com/de-DE/Ansible-Dresden/)

#### while-true-do.io

-   [WTD website](https://while-true-do.io)
-   [WTD github](https://github.com/while-true-do)
-   [WTD twitter](https://twitter.com/wtd_news)

#### Profi.com AG (Sponsor)

-   [proficom.de](https://www.proficom.de/)

---
class: title, middle, center

## Fin

Thank you so much for participating and please feel free to join us again.
