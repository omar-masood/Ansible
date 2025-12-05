```
⭐ 1. Check Ansible Version
👉 “Hey Ansible, which version are you?”

ansible --version

⭐ 2. Ping Your Servers
👉 “Hello servers, are you alive?”

ansible all -m ping

Ping only webservers group:

ansible webservers -m ping

⭐ 3. Run Simple Commands (Ad-Hoc)
👉 “Do this command on my servers!”
Example: show date

ansible all -a "date"

Example: reboot servers

ansible all -a "reboot"
⭐ 4. Copy Files
👉 “Take this file and put it on servers!”

ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts"

⭐ 5. Create/Delete Files & Folders
👉 Create a folder:

ansible all -m file -a "dest=/tmp/test state=directory"

👉 Delete folder:
ansible all -m file -a "dest=/tmp/test state=absent" 
👉 Change permissions:
ansible all -m file -a "dest=/tmp/file.txt mode=600" 
⭐ 6. Install Packages
👉 “Install this app!”

ansible all -m apt -a "name=nginx state=present"

👉 Install latest version:
ansible all -m apt -a "name=nginx state=latest"

👉 Remove package:
ansible all -m apt -a "name=nginx state=absent"

⭐ 7. Manage Services
👉 Start a service:
ansible all -m service -a "name=nginx state=started"

👉 Restart:
ansible all -m service -a "name=nginx state=restarted"

👉 Stop:
ansible all -m service -a "name=nginx state=stopped" 
⭐ 8. Git Pull Repo
👉 “Get code from GitHub!”

ansible all -m git -a "repo=https://github.com/example/repo.git dest=/src/myapp"

⭐ 9. Run a Playbook
👉 Playbook = “Ansible storybook”
Run it:

ansible-playbook myplaybook.yml

Specify inventory file:

ansible-playbook -i inventory.ini myplaybook.yml

⭐ 10. Check Hosts in a Group
👉 “Show me who is inside!”

ansible webservers --list-hosts

⭐ 11. Use Become (sudo)
👉 Run as root:

ansible all -m ping --become

👉 Ask for sudo password:

ansible all -m ping --become --ask-become-pass


```
