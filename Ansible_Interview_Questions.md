1. Write a Simple YAML file To deploy an package?

```
---
- name: Deploy a package on remote servers
  hosts: webservers
  become: yes  # run with sudo privileges

  tasks:
    - name: Update package cache
      ansible.builtin.apt:
        update_cache: yes

    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Ensure Nginx is running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes
```

2. What are the Parameter to use run the script as root user?

We can use -b option: ansible-playbook -i hosts.ini deploy_package.yml -b

3. What is the Roles in ansible and How to call it?

/etc/ansible/roles/httpfullinstall/tasks/main.yml

```
- name: Basic install
  hosts: testservers
  roles:
   - httpfullinstall
```

4. If you Want to run the Ansible task simultaneously to 10 servers and what is the parameter to use for it?

By deafult, Ansible will deploy for 5 servers and using fork option we adjust it and run it 10 or 15 servers as well. 

Command: ansible-playbook playbook.yml -f 10

5. Where to store cresentials in ansible?

We use Ansible vault to store secrets and will be call them in our playbook.

ansible-vault create secrets.yml 
ansible-vault encrypt secrets.yml
Before we use ansible passwords in yaml file, we need to install a module from galaxy.
ansible-galaxy collection install community.docker
ansible-playbook pull_docker_image.yml --ask-vault-pass

6. How to enable the historical log in ansible?
[defaults]
log_path = /var/log/ansible.log
no_log = True (Not storing sennstive data in log)

7. Command to check the Syntax of the ansible-playbook and start from particulat Task?
ansible-playbook playbook.yml --check & ansible-playbook playbook.yml --start-at-task="Install package"

8. Latest ansible version?
2.18.0 and it supported upto 2026.

9. How to Recover ansble vault Password?
We can do rekay option and recvover the password.

10. How to Exclude the Software packages when updating ansible?
vars:
  exclude_packages: "kernel*,docker*,httpd*,postgresql*"
tasks:
  - name: Run yum update with exclusions
    yum:
      name: "*"
      state: latest
      exclude: "{{ exclude_packages }}"

11. How to Ignore particular server explicitly even if it is present in under /etc/ansible/hosts alias name?
Use exclametory symbol and do it.
