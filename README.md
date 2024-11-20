# ZabbixConferenceJapan2024

- Zabbix Conference Japan 2024 で行ったセッション「Zabbix運用をもっと便利にするAnsibleの活用法」資料内のサンプルや補足資料置き場です。

# サンプル

## 資料内のPlaybookサンプル

### inventory

```ini
[zabbix]
zabbix_1 ansible_host=172.31.16.110
zabbix_2 ansible_host=172.31.16.112
```

### playbook

```yaml
---
- name: Retrieve API information
  gather_facts: false
  hosts: zabbix_1

  vars:
    ansible_network_os: community.zabbix.zabbix
    ansible_connection: ansible.netcommon.httpapi
    ansible_httpapi_port: 80
    ansible_user: "Admin"
    ansible_password: "zabbix"

  tasks:
  - name: Add new_host
    community.zabbix.zabbix_host:
      host_name: "test_host"
      host_groups:
        - "Virtual machines"
        - "Linux servers"
```

### 実行結果

```text
(ansible) $ ansible-playbook -i inventory.ini zabbix_host.yml 

PLAY [Retrieve API information] **************************************************************************************************

TASK [Add new_host] **************************************************************************************************************
changed: [zabbix_1]

PLAY RECAP ***********************************************************************************************************************
zabbix_1               : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

(ansible) $ 
```

# 環境構築

- 環境
  - RockyLinux9(x86_64)
  - Zabbix7.0.0

## 手順例

```text
$ python3 -V
Python 3.9.18
$ mkdir venv_base
$ python3 -m venv venv_base/ansible
$ source venv_base/ansible/
$ source venv_base/ansible/bin/activate
(ansible)$ 
(ansible)$ pip3 install --upgrade pip
(ansible)$ pip3 install ansible
(ansible)$ ansible --version
ansible [core 2.15.13]
:省略
(ansible)$
(ansible)$ ansible-galaxy collection list | grep zabbix
community.zabbix              2.2.0
community.zabbix              2.2.0  
(ansible)$
(ansible)$ ansible-galaxy collection install community.zabbix==3.2.0 --force
(ansible)$
(ansible)$ ansible-galaxy collection list | grep zabbix
community.zabbix              3.2.0  
community.zabbix              2.2.0  
community.zabbix              2.2.0 
(ansible)$ 
```

- 補足
  - venv_base: Ansible用Python仮想環境を配置するディレクトリ 

# 参考資料

## Zabbix Ansibleモジュール

- Ansible-Galaxy zabbixモジュール
  - [community.zabbix](https://galaxy.ansible.com/ui/repo/published/community/zabbix/)
  - [zabbix.zabbix](https://galaxy.ansible.com/ui/repo/published/zabbix/zabbix/)

## Ansibleに関する有益なLINK

- [てくなべ](https://tekunabe.hatenablog.jp/archive/category/ansible)
- [Ansibleユーザー会](https://ansible-users.connpass.com/)
  - [Ansible Night 2024.3 これから始めるAnsible 小さく始めるAnsible](https://speakerdeck.com/stopendy/how-to-start-ansible-small)
