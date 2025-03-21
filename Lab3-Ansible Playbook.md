## Implementing Ansible Playbook

`An Ansible Playbook is a YAML-based automation script that defines a set of tasks to configure, deploy, and manage systems in a repeatable and declarative manner.`

```
mkdir ansible-labs && cd ansible-labs
```

### Task 1: Write a Playbook to Install & Setup Apache (Web Server)
Create a custom inventory file
```
vi inventory.ini
```
```
# inventory file: hosts

ec2-user@<IP Adress of managed-node1>
ec2-user@<IP Adress of managed-node2>
```
Create the playbook
```
vi install-httpd-pb.yml
```
Add the given content, by pressing "INSERT"
```
- name: This play will install apache web servers on all the hosts
  hosts: all
  become: yes
  tasks:
    - name: Task1 will install httpd using yum
      yum:
        name: httpd
        #local cache of the package information available from the repositories configured on the system
        update_cache: yes
        state: latest
    - name: Task2 will upload custom index.html into all hosts
      copy:
        src: /home/ec2-user/ansible-labs/index.html
        dest: /var/www/html
    - name: Task3 will setup attributes for file
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode:  0644
    - name: Task4 will start the httpd
      service:
        name: httpd
        state: started
```
Save the file using `ESCAPE + :wq!`

State as 'Present' and 'Installed' are used interchangeably. They both do the same thing i.e. It 
will ensure that a desired package is installed. Whereas State as 'Latest' means in addition
to installation, it will go ahead and update if it is not of the latest available version.

Now lets create an index.html file to be used as the landing page for the web server.
In task2 of the above playbook, this 'index.html' will be copied to the document root of the 
httpd server.
```
vi index.html
```

Add the given content, by pressing "INSERT" 
```
<html>
  <body>
  <h1>Welcome to Ansible Training from CloudThat</h1>
  </body>
</html>
```
Save the file using `ESCAPE + :wq!`

Now run the playbook so that it installs httpd to all the hosts and index.html is copied from 
the ansible-control
```
ansible-playbook install-httpd-pb.yml -i inventory.ini
```

```
curl <private_ip of node1> 
```

```
curl <private_ip of node2>
```

### Task 2: Uninstall httpd web server

With slight modification, we can change the playbook to uninstall httpd 
```
cp install-httpd-pb.yml uninstall-httpd-pb.yml
```
```
vi uninstall-httpd-pb.yml
```
Retain only first task. Replace 'state: latest' with 'state: absent'

Check if the playbook is ok
```
ansible-playbook uninstall-httpd-pb.yml --check
```
Now run the playbook and check in the browser if the web page exists.
```
ansible-playbook uninstall-httpd-pb.yml
```
Check the browser with the corresponding IPv4 DNS name and ensure webpage is removed.
