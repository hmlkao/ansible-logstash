# ansible-logstash
Ansible role for manage 6.x Logstash

## Features
 - Only one instance is supported
 - Should be as similar as instalation via package manager (allows updates via package manager)

## Supported OS
 - CentOS 7

## Role variables
```
ls_filters                      (REQUIRED) Fileglob with filter configuration templates, at least one file must match
ls_config                       These values will be passed to logstash.yml; Default: {path.data: /var/lib/logstash, path.logs: /var/log/logstash}
ls_heap_size                    Defines a heap size; Default: 1g
ls_patterns_dir                 Directory where will be patterns placed on host; Default: /etc/logstash/patterns
ls_custom_patterns              Fileglob with custom patterns; Default: empty
ls_update                       Update Logstash to the latest version from repository; Default: false
ls_version                      Version of logstash which will be installed, used only if ls_update is false; Default: empty
ls_restart_on_change            Service will be restarted when some changes appear; Default: true
java_update                     Update Java to the latest version from repository; Default: false
```

## Role usage examples
Clone the current repository to `role` folder
```
cd ansible-playbooks/roles
clone https://github.com/hmlkao/ansible-logstash.git
```

### Simplest playbook
```
- hosts:
    - log-servers
  roles:
    - ansible-logstash
  vars:
    ls_filters: "{{ playbook_dir }}/templates/logstash/*.conf.j2"
```
This configuration only set the heap size, any other required variables are taken from `ansible-logstash.git/defaults/main.yml`

### Extended playbook
```
- hosts:
    - log-servers
  roles:
    - ansible-logstash
  vars:
    ls_config:
      node.name: "{{ inventory_hostname }}"
      path.data: /opt/logstash
      xpack.monitoring.enabled: true
    ls_heap_size: 2g
    ls_filters: "{{ playbook_dir }}/templates/logstash/*.conf.j2"
    ls_custom_patterns: "{{ playbook_dir }}/files/logstash/patterns/*"
```

## TODO
- Create a tests
