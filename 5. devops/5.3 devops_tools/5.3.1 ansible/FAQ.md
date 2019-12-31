### 1. 在模板中如何遍历某一组内的所有主机？

（1）获取hosname

```shell
{% for host in groups['db_servers'] %}
  {{ host }}
{% endfor %}
```

（2）获取IP

```shell
{% for host in groups['db_servers'] %}
  {{ hostvars[host].ansible_host }}
{% endfor %}
```

### 2. 如何快速构建一个ansible测试task？

```shell
- name: test
  hosts: kube-deploy
  tasks:
    - debug:
        msg: "{% for host in groups['etcd'] %}https://{{ hostvars[host].ansible_host }}:2379,{% endfor %}"
```

