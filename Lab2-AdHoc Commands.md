## Understanding Ansible Ad-Hoc Commands

`Ad-hoc commands are one-time, quick execution commands used without writing playbooks in automation tools like Ansible. They are useful for immediate tasks that don’t require repeatability.` 

```
sudo vi /etc/ansible/hosts
```

Add the given line, by pressing "INSERT" 
Add localhost and add the connection as local so that it wont try to use ssh
```
localhost ansible_connection=local
```
**save the file using** `ESCAPE + :wq!`

In real life situations, one of the managed node may be used as the ansible control node. In such cases, we can make it a managed node, by adding localhost in hosts inventory file.

### Get memory details of the hosts using the below ad-hoc command
```
ansible all -m command -a "free -h"
```
### Implicit Module: When the module is not specified, Ansible uses the command module by default.
```
ansible all -a "free -h"
```
### Create a user ansible-test in the the control node. 
```
ansible all -m user -a "name=ansible-test" --become
```
### Lists all users in the machine. Check if ansible-test is present. 
```
ansible all -a "cat /etc/passwd"
```

### List all directories in /home. Ensure that directory 'ansible-test' is present in /home. 
```
ansible all -a "ls /home"
```
### Change the permission mode from '700' to '755' for the new home directory created for ansible-test
```
ansible all -m file -a "dest=/home/ansible-test mode=755" --become
```

### Check if the permissions got changed
```
ansible all -a "sudo ls -l /home"
```

### Create a new file in the new directory in node 1
```
ansible managed-node1 -m file -a "dest=/home/ansible-test/demo.txt mode=600 state=touch" --become
```

### Check if the permissions got changed
```
ansible managed-node1 -a "sudo ls -l /home/ansible-test/"
```

### Add content into the file
```
ansible managed-node1 -b -m lineinfile -a 'dest=/home/ansible-test/demo.txt line="This server is managed by Ansible"'
```
### Check if the lines are added in demo.txt
```
ansible managed-node1 -a "sudo cat /home/ansible-test/demo.txt"
```
### You can remove the line using the parameter state=absent
```
ansible managed-node1 -b -m lineinfile -a 'dest=/home/ansible-test/demo.txt line="This server is managed by Ansible" state=absent'
```
### Check if the lines are removed from demo.txt
```
ansible managed-node1 -b -a "sudo cat /home/ansible-test/demo.txt"
```

### Copy a file from ansible-control node to host managed-node1
```
touch test.txt
```
```
echo "This file will be copied to managed node using copy module" >> test.txt
```
Now Copy. --become can be replaced by -b
```
ansible managed-node1 -m copy -a "src=test.txt dest=/home/ansible-test/test" -b 
```

### Check if the file got copied to managed node.
```
ansible managed-node1 -b -a "sudo ls -l /home/ansible-test/test"
```

### Clean Up
Delete the users
```
ansible all -m user -a "name=ansible-test state=absent" --become
```
Delete the directories
```
ansible all -a "rm -rf /home/ansible-test" --become
```
Cross Verify
```
ansible all -a "id ansible-test"
```
```
ansible all -a "ls /home"
```
Now remove the localhost from the host file
```
sudo vi /etc/ansible/hosts
```
Remove the below line from hosts inventory file. 

`localhost ansible_connection=local`

**save the file using** `ESCAPE + :wq!`
