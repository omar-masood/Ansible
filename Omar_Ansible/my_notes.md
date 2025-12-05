
### 1)  Creating dir1 folder file with 755 permission
```
---
- hosts: all
  become: true
  tasks:
    - name: Create directory for daily logs
      ansible.builtin.file:
        path: /home/dir1
        state: directory
        mode: '0755'

```


### 2)  Creating file.txt with permission 777 on node1 server

```
---
- hosts: node1
  become: true
  tasks:
    - name: Create directory for daily logs
      ansible.builtin.file:
        path: /home/file.txt
        state: touch
        mode: '0777'
```

### 3) Partitioning /dev/sdb disk into 2 parts 6GB and 4GB

```
---
- name: Create partitions on a disk
  hosts: node1
  become: true
 
  tasks:
    - name: Create the first primary partition on /dev/sdb
      community.general.parted:
        device: /dev/sdb
        number: 1
        state: present
        part_start: 0%
        part_end: 6GiB   # your original size
 
    - name: Create the second primary partition on /dev/sdb
      community.general.parted:
        device: /dev/sdb
        number: 2
        state: present
        part_start: 6GiB   # start right after partition 1
        part_end: 10GiB    # or whatever size you want
```


### 4) Pinging server and printing Hello World message

```
---
- name: My first play
  hosts: node1
  tasks:
    - name: Ping my hosts
      ansible.builtin.ping:
    - name: Print message
      ansible.builtin.debug:
        msg: Hello world.

```

### 5) Copying file from local machine to server

```
---
- name: Copy file from local to remote clients
hosts: group1
tasks:
  - name: Copying file
    become: true
  - copy:
      src: /root/some.cfg
      dest: /root
      owner: root
      group: root
      mode: 0777
```

### 6) Checking our job is done or not from master server

```
---
- name: httpd
  hosts: all
  become: yes
  tasks:
 
# TAKS-1
    - name: installing httpd
      yum:
        name: httpd
        state: present
# TAKS-2
    - name: copy file from local to remote
      copy:
        src: /home/ansible/files/myfile
        dest: /var/www/html/index.html
 
     
    # - name: if httpd installed
    #   shell: rpm -qa | grep httpd
    #   register: httpd_installed
 
    # - name: get output from previous task
    #   debug:
    #     var: httpd_installed.stdout
 
    - name: ls
      shell: cat /var/www/html/index.html
      register: html_content
 
    - name: show html content
      debug:
        msg: "{{ html_content.stdout }}"
```

### 7)  Setting up Firewall, adding ports

```
---
- name: Configure firewall on server
  hosts: all
  become: yes
 
  tasks:
 
    - name: Install firewalld
      ansible.builtin.package:
        name: firewalld
        state: present
 
    - name: Ensure firewalld service is enabled and running
      ansible.builtin.service:
        name: firewalld
        state: started
        enabled: yes
 
    - name: Open required ports in firewall
      ansible.posix.firewalld:
        port: "{{ item }}"
        permanent: yes
        state: enabled
        immediate: yes
      loop:
        - "80/tcp"     # HTTP
        - "443/tcp"    # HTTPS
        - "22/tcp"     # SSH (ensure you don't lock yourself out)
 
    - name: Reload firewalld to apply changes
      ansible.posix.firewalld:
        state: reloaded
```

### 8)   Removing 443/tcp port

```
---
- name: Configure firewall on server
  hosts: node1
  become: yes
 
  tasks:
 
    - name: Ensure firewalld service is enabled and running
      ansible.builtin.service:
        name: firewalld
        state: started
        enabled: yes
 
    - name: Remove tcp port in firewall
      ansible.posix.firewalld:
        port: "443/tcp"
        permanent: yes
        state: disabled
        immediate: yes
 
    - name: Reload firewalld to apply changes
      ansible.posix.firewalld:
        state: enabled
```
