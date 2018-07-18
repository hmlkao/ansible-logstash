# ansible-logstash
Ansible role for manage 6.x Logstash

## Features
 - Only one instance is supported
 - Should be as similar as instalation via package manager (allows updates via package manager)

## Role variables
```
ls_config               These values will be passed to logstash.yml; Default: {path.data: /var/lib/logstash, path.logs: /var/log/logstash}
ls_jvm_options          Values which will be passed to jvm.options; Default: {-Xms1g, -Xmx1g}

```

## Tested environment
 - CentOS 7
 
## TODO
 - Create patterns dir and set correct permissions
 - Copy patterns if set
 