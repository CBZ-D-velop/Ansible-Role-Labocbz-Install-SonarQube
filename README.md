# Ansible role: labocbz.install_sonarqube

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: SonarQube](https://img.shields.io/badge/Tech-SonarQube-orange)
![Tag: PostgreSQL](https://img.shields.io/badge/Tech-PostgreSQL-orange)

An Ansible role to install and configure SonarQube on your host.

This Ansible role streamlines the installation of SonarQube, offering flexibility and customization based on the provided variables. Whether deploying from sources or a zip archive, the role automates the installation process, allowing for seamless integration into your environment.

Key parameters, such as installation path, SonarQube version, embedded web server configurations, and PostgreSQL database details, are all customizable to suit your infrastructure preferences.

The role handles the creation of necessary system users, PostgreSQL database management, and configures SonarQube as a systemd service. It provides the option to customize Elasticsearch settings while ensuring secure installation practices.

In summary, with this Ansible role, deploying SonarQube becomes efficient and tailored to your environment, simplifying and automating crucial aspects of the installation process.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---

install_sonarqube__user: "sonarqube"
install_sonarqube__group: "sonarqube"

install_sonarqube__postgres_user: "postgres"
install_sonarqube__postgres_group: "postgres"

install_sonarqube__postgres_sonarqube_login: "{{ install_sonarqube__user }}"
install_sonarqube__postgres_sonarqube_password: "KDOEKD930DKDOEK3"

install_sonarqube__lib_path: /usr/local/sonarqube
install_sonarqube__log_path: "/var/log/sonarqube"
install_sonarqube__version: "10.3.0.82913"

install_sonarqube__web_heap: "512m"
install_sonarqube__web_host: "0.0.0.0"
install_sonarqube__web_context: "/"
install_sonarqube__web_port: 9000

install_sonarqube__elasticsearch_heap: "512m"
install_sonarqube__elasticsearch_port: 9001
install_sonarqube__elasticsearch_host: "0.0.0.0"

install_sonarqube__log_level: "INFO"
install_sonarqube__log_retention: 31

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host_system_users:
  - login: "sonarqube"
    group: "sonarqube"
  - login: "postgres"
    group: "postgres"

inv_install_sonarqube__user: "sonarqube"
inv_install_sonarqube__group: "sonarqube"

inv_install_sonarqube__postgres_user: "postgres"
inv_install_sonarqube__postgres_group: "postgres"

inv_install_sonarqube__postgres_sonarqube_login: "{{ install_sonarqube__user }}"
inv_install_sonarqube__postgres_sonarqube_password: "KDOEKD930DKDOEK3"

inv_install_sonarqube__lib_path: /usr/local/sonarqube
inv_install_sonarqube__log_path: "/var/log/sonarqube"
inv_install_sonarqube__version: "10.3.0.82913"

inv_install_sonarqube__web_heap: "512m"
inv_install_sonarqube__web_host: "0.0.0.0"
inv_install_sonarqube__web_context: "/"
inv_install_sonarqube__web_port: 9000

inv_install_sonarqube__elasticsearch_heap: "512m"
inv_install_sonarqube__elasticsearch_port: 9001
inv_install_sonarqube__elasticsearch_host: "0.0.0.0"

inv_install_sonarqube__log_level: "INFO"
inv_install_sonarqube__log_retention: 31

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_sonarqube"
  tags:
    - "labocbz.install_sonarqube"
  vars:
    install_sonarqube__user: "{{ inv_install_sonarqube__user }}"
    install_sonarqube__group: "{{ inv_install_sonarqube__group }}"
    install_sonarqube__postgres_user: "{{ inv_install_sonarqube__postgres_user }}"
    install_sonarqube__postgres_group: "{{ inv_install_sonarqube__postgres_group }}"
    install_sonarqube__postgres_sonarqube_login: "{{ inv_install_sonarqube__postgres_sonarqube_login }}"
    install_sonarqube__postgres_sonarqube_password: "{{ inv_install_sonarqube__postgres_sonarqube_password }}"
    install_sonarqube__lib_path: "{{ inv_install_sonarqube__lib_path }}"
    install_sonarqube__log_path: "{{ inv_install_sonarqube__log_path }}"
    install_sonarqube__version: "{{ inv_install_sonarqube__version }}"
    install_sonarqube__web_heap: "{{ inv_install_sonarqube__web_heap }}"
    install_sonarqube__web_host: "{{ inv_install_sonarqube__web_host }}"
    install_sonarqube__web_context: "{{ inv_install_sonarqube__web_context }}"
    install_sonarqube__web_port: "{{ inv_install_sonarqube__web_port }}"
    install_sonarqube__elasticsearch_heap: "{{ inv_install_sonarqube__elasticsearch_heap }}"
    install_sonarqube__log_level: "{{ inv_install_sonarqube__log_level }}"
    install_sonarqube__log_retention: "{{ inv_install_sonarqube__log_retention }}"
    install_sonarqube__elasticsearch_port: "{{ inv_install_sonarqube__elasticsearch_port }}"
    install_sonarqube__elasticsearch_host: "{{ inv_install_sonarqube__elasticsearch_host }}"
  ansible.builtin.include_role:
    name: "labocbz.install_sonarqube"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2024-01-02: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* Role install SonarQube
* App is installed and working
* WARNING, error on install PostgreSQL when Java 17 installed before, you can install Java 11, PostgreSQL and after that install Java 17, so in some systems, 2 runs are mandatory
* PostgreSQL installed and configured
* Will need SSL

### 2024-01-03: No SSL

* Sonar doesnt allow to configure SSL, need a reverse proxy
* Need tests
* Install Java 17 fixed

### 2023-01-23-b: Tests

* Role have been tested*
* Certs removed (no SSL/TLS support for SonarQube ...)

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __
* Tested and validated on Docker DIND
* Removed docker socket local and port

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
