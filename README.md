# Elastic Stack in a Box

This repository will install the [Elastic Stack](https://www.elastic.co/products) (Elasticsearch, Logstash, Kibana, and Beats) and optionally start a trial of commercial features. You can start from scratch and configure everything with [Vagrant and Ansible](#vagrant-and-ansible).

## Features

* Filebeat `system`, `auditd`, `logstash`, `mongodb`, `nginx`, `osquery`, and `redis` modules
* Filebeat collecting Kibana JSON logs from `/var/log/kibana/kibana.log`
* Auditbeat `file_integrity` module on `/home/vagrant/` directory and `auditd` module
* Heartbeat pinging nginx every 10s
* Metricbeat `system`, `docker`, `elasticsearch`, `kibana`, `logstash`, `mongodb`, `nginx` and `redis` modules
* Packetbeat sending its data via Redis + Logstash, monitoring flows, ICMP, DNS, HTTP (nginx and Kibana), Redis, and MongoDB (generate traffic with `$ mongo /elk-stack/mongodb.js`)
* The pattern for nginx is already prepared in */opt/logstash/patterns/*

## Vagrant and Ansible

Do a simple `vagrant up` by using [Vagrant](https://www.vagrantup.com)'s [Ansible provisioner](https://www.vagrantup.com/docs/provisioning/ansible.html). All you need is a working [Vagrant installation](https://www.vagrantup.com/docs/installation/) (2.2.4+ but the latest version is always recommended), a [provider](https://www.vagrantup.com/docs/providers/) (tested with the latest [VirtualBox](https://www.virtualbox.org) version), and 3GB of RAM.

With the [Ansible playbooks](https://docs.ansible.com/ansible/playbooks.html) in the */elk-stack/* folder you can configure the whole system step by step. Just run them in the given order inside the Vagrant box:

```sh
> vagrant ssh
$ cd /elastic-stack/
$ ansible-playbook 1_configure-elasticsearch.yml
$ ansible-playbook 2_configure-kibana.yml
$ ansible-playbook 3_configure-logstash.yml
$ ansible-playbook 4_configure-auditbeat.yml
$ ansible-playbook 4_configure-filebeat.yml
$ ansible-playbook 4_configure-heartbeat.yml
$ ansible-playbook 4_configure-metricbeat.yml
$ ansible-playbook 4_configure-packetbeat.yml
$ ansible-playbook 5_configure-dashboards.yml
```

Or if you are in a hurry, run all playbooks with `$ /elastic-stack/all.sh` at once.

## Kibana

Access Kibana at [https://127.0.0.1:5601](https://127.0.0.1:5601).

## Test Data

You can use */opt/injector.jar* to generate test data in the `person` index. To generate 100,000 documents in batches of 1,000 run the following command:

```
$ java -jar /opt/injector.jar 100000 1000
```

## Logstash Parser

-	The GROK Parser for _yum.log_ is located at `elastic-stack/templates/logstash/yum-filter.log`
-	Screenshots are found at `docs/images/*.png`


# TASK

*Create a Dashboard representing Amount of Packages installed per Days in Logstash from [Yum Log](docs/files/yum.log)*


* 1 VM with ELK STACK from within Vagrant and orchestrated via Ansible

[Vagrant Log](docs/files/ubuntu-bionic-18.04-cloudimg-console.log)

[VB ELK Stack Box](docs/images/VB_ELK_STACK_BOX.png)

[Ansible Log](elastic-stack/logs/logfile.log)

* Logstash Index

[Kibana - 127.0.0.1 - Discover - Visible.png](docs/images/Kibana-127.0.0.1-Discover-Visible.png)

[Kibana - 127.0.0.1 - Discover - Full.png](docs/images/Kibana-127.0.0.1-Discover-Full.png)

* Logstash Dashboard

[Kibana - 127.0.0.1 - Visualize - YUM.png](docs/images/Kibana-127.0.0.1-Visualize-YUM.png)

[Kibana - 127.0.0.1 - Dashboard - YUM.png](docs/images/Kibana-127.0.0.1-Dashboard-YUM.png)

* CSV Export of Visualization

[YUM.log.csv](docs/files/YUM.log.csv) is based on [Yum Log](docs/files/yum.log)