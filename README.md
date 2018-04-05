# ansible-logstash
Ansible role for 5.x Elastic Logstash used as a part of ELK Stack.
Tested platforms are:
* CentOS 7
Ansible version >= 2.4 must be used.

This role is based on [official Elastic Ansible role for Elasticsearch](https://github.com/elastic/ansible-elasticsearch) (exactly commit 7714c925e06e8538a09b6e9d8c1bd074813e9639)

## Usage
Create your Ansible playbook with your own tasks and include role logstash. You will have to have this repository accessible within the context of playbook, e.g.
```bash
cd /my/repos/
git clone https://github.com/hmlkao/ansible-logstash.git
cd /my/ansible/playbook
mkdir -p roles
ln -s /my/repos/ansible-logstash ./roles/logstash
```

Then create your playbook yaml adding the role logstash.

By default, the user is only required to specify **ls_instance_name**  which is used to exactly identify an instance (different paths, configurations, etc. for different instances).

In the most cases there will be only one instance, eg. "node1"

You should set correct path to config files **ls_conf_fileglob** OR create Config file in files/conf.d directory, eg. brand.conf regarding to [Logstash manual](https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html)

All configuration files in files/conf.d will be used if **ls_conf** is not specified.

### Simplest configuration
```yaml
- name: Simple Example
  hosts
    - localhost
  tasks:
    - import_role:
        name: logstash
      vars:
        ls_instance_name: "node1"
```
The above runs Logstash instance on localhost with default values (so it is accessible only from localhost).

### Extended configuration with X-pack enabled
```yaml
- name: Extended Example
  hosts
    - elk-testing
    - elk-production
  tasks:
    - import_role:
        name: logstash
      vars:
        ls_instance_name: "node1"
        ls_config: {
          server.host: 0.0.0.0,
        }
        ls_enable_xpack: true
```

The above runs one Logstash instance on all hosts in elk-testing and elk-production groups which will be listen on all interfaces
### Multiple instances
```yaml
- name: Extended Example
  hosts
    - elk.example.com
  tasks:
    - import_role:
        name: logstash
      vars:
        ls_instance_name: "devel"
        ls_enable_xpack: true
        ls_filter_fileglob: "{{ playbook_dir }}/files/logstash/devel*.conf"
    - import_role:
        name: logstash
      vars:
        ls_instance_name: "testing"
        ls_config: {
          server.host: 0.0.0.0,
        }
        ls_filter_fileglob: "{{ playbook_dir }}/files/logstash/testing*.conf"
```

The above runs two Logstash instances on host elk.example.com.

The first instance "devel" will listen only on localhost and uses files devel*.conf for plugins configuration.

The second instance "testing" will listen on all interfaces with X-pack disabled and uses only testing*.conf for plugin configuration.

## Role variables
`elastic_version`         (5.6.5) Version of Logstash which will be installed

`ls_config`               ({}) Variables from [Logstash configuration](https://www.elastic.co/guide/en/logstash/current/logstash-settings-file.html) (Paths path.data, path.config and path.logs should not be set, these are generated per instance - path.data can be affected by `ls_data_dir`)

`ls_data_dir`             (/var/lib/logstash) Path to Logstash data, default path for instance will be /var/lib/logstash/{{ hostname }}-{{ ls_instance_name }}/

`ls_filter_fileglob`      (templates/conf.d/*.conf.j2) Templates of [Logstash config files](https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html), at least one file **is required**

`ls_enable_xpack`         (false) Enable X-pack features in Logstash

`ls_xpack_custom_url`     () URL for X-pack download

`ls_plugins_reinstall`    (false) Force reinstall all plugins

`ls_plugins`              (false) An array of plugin definitions

`ls_heap_size`            (2g) Heap size of memory to used by Logstash


## TODO
* Check that examples work
* Try on Debian based OS
* Manage custom plugins
