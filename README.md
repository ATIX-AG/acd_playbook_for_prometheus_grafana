[![build status](https://github.com/ATIX-AG/acd_playbook_for_prometheus_grafana/workflows/lint/badge.svg)](https://github.com/ATIX-AG/acd_playbook_for_prometheus_grafana/actions)

## Ansible Playbook for Prometheus and Grafana ACD

## Operating Systems

This playbook currently supports managed hosts running CentOS 7.

## Description

This playbook demonstrates the usage of Foreman ACD by installing Prometheus and a connected Grafana.
It consists of two roles:
- prometheus - a role to set up and install prometheus

  This role also configures the firewall by opening the prometheus-corresponding ports.
- grafana - a role to set up and install a grafana scraping from prometheus

  This role also configures the firewall by opening the grafana-corresponding ports.

For a basic setup via ACD, the variables can be used as found in the `group_vars` directory:

`group_vars/all/main.yml`:
```yaml
---
pmt_port: "9090"
...
```

Name     | Description     | Default | Mandatory (y/n)
-------- | --------------- | ------- | ---------------
pmt_port | Prometheus Port | 9090    | y

`group_vars/prometheus/main.yml`:
```yaml
---
pmt_arch: "amd64"
pmt_os: "linux"
pmt_vers: "2.24.1"
pmt_dl_link_base: "https://github.com/prometheus/prometheus/releases/download/"
pmt_dl_link: "{{pmt_dl_link_base}}v{{pmt_vers}}/{{pmt_filename}}"
pmt_filename: "prometheus-{{pmt_vers}}.{{pmt_os}}-{{pmt_arch}}.tar.gz"
pmt_dir: "/home/prometheus/prometheus"
pmt_dl_dest: "{{pmt_dir}}/{{pmt_filename}}"
pmt_glbl_scrape_interval: "10s"
pmt_job_scrape_interval: "5s"
...
```

Name                     | Description              | Default                                                     | Mandatory (y/n)
------------------------ | ------------------------ | ----------------------------------------------------------- | ---------------
pmt_arch                 | Architecture             | amd64                                                       | y
pmt_os                   | Operating System         | linux                                                       | y
pmt_vers                 | Version                  | 2.24.1                                                      | y
pmt_dl_link_base         | Download Link Prefix     | https://github.com/prometheus/prometheus/releases/download/ | y
pmt_dl_link              | Download Link            | {{pmt_dl_link_base}}v{{pmt_vers}}/{{pmt_filename}}          | n
pmt_dir                  | Installation Directory   | /home/prometheus/prometheus                                 | y
pmt_dl_dest              | Download Directory       | {{pmt_dir}}/{{pmt_filename}}                                | n
pmt_glbl_scrape_interval | Global Scraping Interval | 10s                                                         | y
pmt_job_scrape_interval  | Local Scraping Interval  | 5s                                                          | y

`group_vars/grafana/main.yml`:
```yaml
---
gfn_arch: "x86_64"
gfn_vers: "7.3.7-1"
gfn_dl_link_base: "https://dl.grafana.com/oss/release/grafana"
gfn_dl_link: "{{gfn_dl_link_base}}-{{gfn_vers}}.{{gfn_arch}}.rpm"
gfn_port: "3000"
gfn_custom_dashboard: ""
gfn_admin_user: "admin"
gfn_admin_password: "atix0815!"
...
```

Name                 | Description              | Default                                            | Mandatory (y/n)
-------------------- | ------------------------ | -------------------------------------------------- | ---------------
gfn_arch             | Architecture             | x86_64                                             | y
gfn_vers             | Version                  | 7.3.7-1                                            | y
gfn_dl_link_base     | Download Link Prefix     | https://dl.grafana.com/oss/release/grafana         | y
gfn_dl_link          | Download Link            | {{gfn_dl_link_base}}-{{gfn_vers}}.{{gfn_arch}}.rpm | n
gfn_port             | Port                     | 3000                                               | y
gfn_custom_dashboard | Link to custom dashboard |                                                    | n
gfn_admin_user       | Admin User Name          | admin                                              | y
gfn_admin_password   | Admin User Password      | atix0815!                                          | y

## Documentation

* [Application Centric Deployment Guide](https://docs.orcharhino.com/or/docs/sources/usage_guides/application_centric_deployment_guide.html) in the orcharhino documentation
* [Deploying a Prometheus and Grafana Cluster Using Application Centric Deployment](https://orcharhino.com/deploying-a-prometheus-and-grafana-cluster-using-application-centric-deployment/) on the orcharhino blog
