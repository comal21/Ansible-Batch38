## Blocks with Ansible Playbook
Create and edit blklab.yml in the same labs directory Notice that the “web_package” variable is an invalid package. Due to the invalid package in a block, tasks under rescue will run
```
cd ~/ansible-labs
```
```
vi blklab.yml
```
```
---
- hosts: all
  become: yes
  gather_facts: no
  tasks:
    - block:
        - name: Install web_package1 ({{ web_package1 }})
          yum:
            name: "{{ web_package1 }}"
            state: latest
      rescue:
        - name: Install web_package2 ({{ web_package2 }})
          yum:
            name: "{{ web_package2 }}"
            state: latest
      always:
        - name: Cleanup - Remove temporary file
          file:
            path: /tmp/tempfile
            state: absent
  vars:
    web_package1: http
    web_package2: nginx
```
save the file using ESCAPE + :wq!

Execute the playbook Block tasks fail and that Rescue tasks are running due to the failure of block tasks. The Always tasks run independently
```
ansible-playbook blklab.yml
```
Now fix the package name in the Playbook (web_package: httpd) and run the Playbook again
```
ansible-playbook blklab.yml
```
Notice that the tasks under rescue are not running as block tasks ran successfully.
