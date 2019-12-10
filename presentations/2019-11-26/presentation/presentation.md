name: Intro
class: center, middle, bg-dark, fg-light
count: false

# Container Meetup Dresden

Powered by TU Dreden, Ansible Meetup and AWS User Group.

???

- @Martin: Welcome everybody
- @Martin: Some background about the organisational part?
- @Martin: Welcome Special Guests (Carol Chen, Thorsten Höger)
- @Martin: Intro TU DD (2-3 Minutes)
- @Steffen: Intro AWS Meetup (2-3 Minutes)
- @Daniel: Intro Ansible Meetup (2-3 Minutes)
- @Martin: next slide = Agenda for meetup

---

## Agenda Container Meetup

- Ansible + Kubernetes = <3
- Replace Docker with Podman
- Break
- Scientific Computing in Containers
- How Operators with Ansible make Kubernetes Sing
- Break
- Docker on AWS using Fargate, CDK, and Github Actions
- Snacks, Drink, Chat

???

- @Martin: only read the agenda
- @Martin: not to much explanation
- @Martin: Handover to first speaker

---

name: Title
class: center, middle, bg-dark, fg-light
count: false

# Ansible + Kubernetes = <3

Deployments are easy with Ansible.

???

-   thank the last presenter
-   thank the organisator
-   welcome everybody
-   mention the title

---
name: Agenda

## Agenda

1.   Introduction
2.   Fedora IoT
3.   Kubernetes (k3s)
4.   Ansible
5.   Propaganda

???

-   Just mention, don't explain the agenda.
-   Demos will occour in parallel

---
name: Introduction

## Introduction

Welcome to my presentation!

This is me:

**speaker**

- Daniel Schier
- born before the internet
- mostly Open Source *thingys*
- like cats, cookies, technical terms, terabyte
- inspired by "The IT Crowd" and "Mr. Robot"

???

-   Optional small talk

---
name: FedoraIoT
class: center, middle, bg-dark, fg-light

# Fedora IoT

focused on being a strong foundation for IoT ecosystems

[Project Page](https://iot.fedoraproject.org)

---

## Fedora IoT

Fedora IoT is a specialized Fedora Distribution for IoT Devices and
Edge Comuting.

-   immutable OS Image (ostree)
-   snapshots, rollbacks
-   working on aarch64, Raspberry Pi 3 Model B(+)
-   also working x86 VMs
-   IoT related Software pre-installed
-   Security Software pre-installed
-   layered packages (like cockpit)

???

-   explain rpm-ostree

---
name: k3s
class: center, middle, bg-dark, fg-light

# Kubernetes (k3s)

Lightweight Kubernetes

[Project Page](https.//k3s.io)

---

## k3s

Lightweight Kubernetes. Easy to install, half the memory, all in a binary less than 40mb.

**k8s, 5 changes = k3s**

-   removed legacy, non-default, experimental
-   removed in-tree plugins (cloud + storage)
-   sqlite as default storage, but etcd available
-   simple launcher
-   minimal to no dependencies (aside kernel and cgroups)

---
name: Ansible
class: center, middle, bg-dark, fg-light

# Ansible

simple, agentless IT Automation

[Project Page](https://www.ansible.com)

---

## Ansible

I am already using Ansible for:

-   setting a hostname
-   installing some layered packages
-   configuring services
-   configuring firewall
-   configuring podman (just because it's awesome)

???

- because of laziness
- so it was natural to "next slide"

---

## Ansible

So, it was natural to:

- create namespaces
- deploy applications
- manage services
- manage ingress

???

- basically, you can apply every kubernetes file
- kubernetes files can be created from podman

---

# Demo Time

Less talking, more coding.

---
name: Propaganda
class: center, middle, bg-dark, fg-light

# Propaganda

like, subscribe, bell ...

---

# Propaganda

**While True Do**

- <https://while-true-do.io>
- <https://github.com/while-true-do/>

**Style-Cheat**

- <https://style-cheat.io>

**Ansible**

- <https://www.ansible.com/community>
- <https://www.meetup.com/de-DE/Ansible-Meetup-Dresden/>
- <https://github.com/ansible-community>

---
name: Contact

# Contact the speaker

```yaml
---
# github.com/kudos-txt/

- name: Daniel Schier
  home: Dresden, Germany
  mail: daniel@while-true-do.io
  chat: freenode:#while-true-do,@daniel-wtd
  code: https://github.com/while-true-do
  code: https://github.com/style-cheat
  code: https://github.com/kudos-txt
```

---
name: End
class: center, middle, bg-dark, fg-light

# End

This presentation was made with
[remark](https://remarkjs.com/),
[markdown](https://daringfireball.net/projects/markdown/),
[Style Cheat](https://style-cheat.io)
and <3 <3 <3

