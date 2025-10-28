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

    Path: /etc/ansible/roles/httpfullinstall/tasks/main.yml

```
    - name: Basic install
      hosts: testservers
      roles:
      - httpfullinstall
```


4. If you Want to run the Ansible task simultaneously to 10 servers and what is the parameter to use for it?

    By deafult, Ansible will deploy for 5 servers even the task to be deployed on 50 servers also. After finishes 5 servers, it will take next 5. To speed up this process fork option will be useful. 
    ```
    ansible-playbook -i hosts.ini deploy_package.yml --forks 20  (or)  ansible-playbook playbook.yml -f 10
    ```
    Either we directly add -f when running playbook or we can edit in the Conf file for permanent. (/etc/ansible/ansible.cfg)

    forks = 20  


5. Where to store cresentials in ansible?

    We use Ansible vault to store secrets and will be call them in our playbook.

    ```
    ansible-vault create secrets.yml 
    ansible-vault encrypt secrets.
    ```
    Before we use ansible passwords in yaml file, we need to install a module from galaxy.
    ```
    ansible-galaxy collection install community.docker
    ansible-playbook pull_docker_image.yml --ask-vault-pass
    ```

6. How to enable the historical log in ansible?

```
    [defaults] 
    log_path = /var/log/ansible.log 
    no_log = True (Not storing sensitive data in log)
```


7. Command to check the Syntax of the ansible-playbook and start from particulat Task?

ansible-playbook playbook.yml --check & ansible-playbook playbook.yml --start-at-task="Install package"


8. Latest ansible version?

2.18.0 and it supported upto 2026.


9. How to Recover ansble vault Password?

    you cannot recover the vault password. Once it is stored.


10. How to Exclude few packages when you do yum update?

    ```
    vars:
      exclude_packages: "kernel*,docker*,httpd*,postgresql*"
    tasks:
      - name: Run yum update with exclusions
        yum:
          name: "*"
          state: latest
          exclude: "{{ exclude_packages }}"
    ```


11. How to Ignore particular server explicitly even if it is present in under /etc/ansible/hosts alias name?

    We use exclamation mark to exclude a server.

    ```
---
- name: Deploy packages on all webservers except server3
  hosts: webservers
  become: yes

  tasks:
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
      when: inventory_hostname != "server3"

    - name: Start Nginx service
      ansible.builtin.service:
        name: nginx
        state: started
      when: inventory_hostname != "server3"
```
