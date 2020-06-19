## 目的
这个项目是为了在公交公司内部自动化安装配制 docker swarm 而创建。  
自动化运维工具采用 Ansible。所有Linux主机的环境安装配置均由 Ansible 完成。  
docker swarm 配置也在 Ansible中完成。但分属不同 **待填写**。

## 环境
### Ansible 环境
* Ansible version: 2.9.9
* Ansible Control node:
  * 192.168.1.7 docker-19.03.11
* Ansible Managed nodes:
  * 192.168.1.3 docker-1.13.1
  * 192.168.1.9 none
```

Ansible inventory file: /home/young/ansible/hosts
```
在 Ansible control node 上需要安装依赖的 ansible collection：
```
ansible-galaxy collection install ericsysmin.docker
```
Ansible Inventory 内容一般存放在～/ansible/hosts 中，内容为：
```
all:
  hosts:
    local_3:
      ansible_host: 192.168.1.3
    local_9:
      ansible_host: 192.168.1.9
    local_7:
      ansible_host: 192.168.1.7
      ansible_connection: local
  children:
    worker:
      hosts:
        local_3:
        local_9:
    manager:
      hosts:
        local_7:
    dockers:
      hosts:
        local_3:
        local_7:
        local_9:
  vars:
    ansible_python_interpreter: /usr/bin/python3
    ansible_ssh_private_key_file: ~/.ssh/ssh_local_rsa_key
```
Linux 环境的准备工作须执行 `dkfdkfdksl`.
docker swarm 的配置更新执行 `dfdf`.

### 执行 Ansible playbook
```
# 检查playbook语法
ansible-lint playbook.yml
# 将sudo用户密码加密
ansible-vault encrypt_string 'foobar' --name 'the_secret'
# 执行playbook vault 默认密码：123456
ansible-playbook --ask-vault-pass playbook.yml -i ~/ansible/hosts
# 执行docker swarm 工作 
ansible-playbook --ask-vault-pass playbook.yml -i ~/ansible/hosts --tags "run"
# 执行docker 安装工作  
ansible-playbook --ask-vault-pass playbook.yml -i ~/ansible/hosts --tags "build"
```