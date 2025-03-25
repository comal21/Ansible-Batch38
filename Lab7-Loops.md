## Loops

```
cd ~/ansible-labs
```
```
vi looplab.yml
```
```
---
- hosts: all
  become: yes
  tasks:
   - name: creating users
     user:
       name: "{{ item }}"
       state: present
     with_items:
      - userX
      - userY
      - userZ
```
save the file using ESCAPE + :wq!

Execute the playbook
```
ansible-playbook looplab.yml
```
Verify if the users mentioned in the list were added by using an Ansible ad-hoc command
```
ansible all -a "tail -n 3 /etc/passwd"
```
