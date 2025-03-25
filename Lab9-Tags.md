## Tags
Tags can be used for selective execution, task grouping, and debugging in a playbook.

Create the Playbook. Each task will be tagged so that you can see how tags allow for selective task execution.
```
cd ~/ansible-labs
```
```
vi tags.yaml
```
```
---
```
vi tagslabs.yml
```
```
---
- hosts: all
  become: yes
  tasks:
    - name: Create a directory
      file:
        path: /tmp/demo_dir
        state: directory
        mode: '0755'
      tags:
        - create_dir

    - name: Create a file
      copy:
        content: "This is a demo file"
        dest: /tmp/demo_dir/demo_file.txt
      tags:
        - create_file

    - name: Ensure a package is installed (curl)
      yum:
        name: curl
        state: present
      tags:
        - install_curl

```
Notice that only the tasks associated with the mentioned tags are running
```
ansible-playbook -t "create_dir" tagslabs.yml
```
```
ansible-playbook -t "create_file" tagslabs.yml
```
```
ansible-playbook --skip-tags "install_curl" tagslabs.yml
```

List all the tags
```
ansible-playbook tags.yaml --list-tags
```
List all the tasks
```
ansible-playbook tags.yaml --list-tasks
```
