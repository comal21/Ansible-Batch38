## Ansible Vault
```
cd ~/ansible-labs/
```
Now using the vault utility, encrypt the playbook that we created. Give the passwd of your choice
```
ansible-vault encrypt implement-vars.yml
```

Check if the encrypted file can be viwed using the cat command  
```
cat implement-vars.yml
```
Using below command, view the encrypted playbook by giving password that is set
```
ansible-vault view implement-vars.yml
```

For executing the encrypted playbook following command is used
```
ansible-playbook --ask-vault-pass implement-vars.yml
```

Now edit the playbook, add the below line to it and save the playbook
```
ansible-vault edit implement-vars.yml
```
Now if we want to change the password, we can do that with below command
Provide the old and new password to reset the password
```
ansible-vault rekey implement-vars.yml
```

Open the encrypted file and see the content using 
```
cat implement-vars.yml
```

decrypt the file
```
ansible-vault decrypt implement-vars.yml
```
```
cat implement-vars.yml
```
