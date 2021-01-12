# Create a PSQL DB server and restore DB from another master.

### It is not advisible to store password in the playbook, hence encrypt it with ansible-vault by entering below command 
Or
### Use AWS Secret Manager to store the password and lookup in Ansible.

* ## Using AWS Secret Manager
   * Make sure AWS CLI is configured in your Ansible Master.
   * Create a Secret in AWS Secret Manager and mention the type of secret.
```bash
aws secretsmanager create-secret --name psql_db_user_password --description "psql_db_user_password" --secret-string Passw0rd
```
   - Make note of the secret name "psql_db_user_password" in my case and enter the same in your task.

```yaml
password: "{{ lookup('aws_secret', 'psql_db_user_password') }}"
```


* ## Using Ansible Vault

```bash
ansible-vault encrypt <playbook_yaml_file>
-------------------------------------------
ansible-vault encrypt psql_create_db.yaml
```

## To View/Edit the content of the playbook, view by entering below command
```bash
ansible-vault view <playbook_yaml_file>
---------------------------------------
ansible-vault view psql_create_db.yaml

ansible-vault edit <playbook_yaml_file>
---------------------------------------
ansible-vault edit psql_create_db.yaml
```


## To run the playbook, enter below command to ask for vault password
```bash
ansible-playbook -i <inventory_file> <playbook_yaml_file> --ask-vault-pass
ansible-playbook -i psql.txt psql_create_db.yaml --ask-vault-pass
```