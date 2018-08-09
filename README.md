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
ls_restart_on_change            Service will be restarted when some changes appear; Default: true
java_update                     Update Java to the latest version from repository; Default: false
```

## Role examples

### Simple configuration

## TODO
 - Patterns are copied from files, but contain variables, copy them as templates or remove variables
 - Some variables (like `beats_host` and `beats_port`) are not part of role but are used in template
 - Firewall rules are not part of role
