# ansible-role-sysmon

[![GitHub license](https://img.shields.io/github/license/j91321/ansible-role-sysmon?style=flat-square)](https://github.com/j91321/ansible-role-sysmon/blob/master/LICENSE)
[![GitHub last commit](https://img.shields.io/github/last-commit/ebsd/ansible-role-sysmon.svg?style=flat-square)](https://github.com/ebsd/ansible-role-sysmon/commit/master)

An Ansible role that installs Sysmon with selected configuration. Included configurations are [SwiftOnSecurity sysmon config](https://github.com/SwiftOnSecurity/sysmon-config) or [olafhartong sysmon-modular config](https://github.com/olafhartong/sysmon-modular). You can also supply your own config.

Supported platforms:

- Windows 10
- Windows Server 2019
- Windows Server 2016

## Requirements

None

## Functionalities

1. Upload and Install Sysmon if not already installed or if version is lower than `{{sysmon_version}}` variable.

2. If version is lower, uninstall old version and then install new one.

3. Update configuration when config file changed on source directory (files/).

   To only run configuration file update without install, use "configure" tag :

   ```bash
   ansible-playbook install_sysmon.yml -i hosts --tags configure
   ```

## Usage

1. Place Symon.zip from sysinternals inside files/ directory.
2. Place your specific xml configuration file inside files/ direcory.
3. Write a playbook (see example).
4. Run it ! `$ ansible-playbook install_sysmon.yml -i hosts`

## Role Variables

Ansible variables from defaults/main.yml

```
sysmon_install_path: "C:\\Program Files\\Sysmon"
sysmon_version: "11.11"
sysmon_config: swiftonsecurity-sysmonconfig.xml
```

## Dependencies

None

## Example Playbook

```
- name: Install sysmon to sysmon_validation group
  hosts:
    - sysmon_validation
  vars:
    sysmon_install_path: 'C:\Windows'
    sysmon_version: "13.02"
    sysmon_config: sysmonconfig-export.xml
    sysmon_config_path: 'C:\programdata\sysmon'
  roles:
    - ansible-role-sysmon
```

## Example hosts file

```
[sysmon_validation]
host1
host2
host3

[sysmon_validation:vars]
# use `$ kinit admin@domain.lan` to get a kerberos ticket
ansible_user=admin@domain.lan
#ansible_password=password
ansible_connection=winrm
ansible_port=5985
ansible_winrm_transport=kerberos
ansible_winrm_server_cert_validation=ignore
```

## License

MIT

## Author Information

- j91321
- [viktor0x53](https://github.com/viktor0x53)
