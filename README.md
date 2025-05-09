# Ansible-Playbooks-Patching-GI-DB--19.27
This is the Ansible Playbooks for Patching Oracle RAC both Grid and Database Release Update 19.27
# prerequisite
  you’ll need to download the following setup files from Oracle Support and upload in ansible server
 - /installer/software/GI27
 - Grid Home Setup – LINUX.X64_193000_grid_home.zip    
 - Database home Setup – LINUX.X64_193000_db_home.zip
 - GI patching release Aprile2025 p37641958_190000_Linux-x86-64.zip
 - lasted Opatch version p6880880_190000_Linux-x86-64.zipcd 
# configure ansible passwordless to targets servers for root,grid and oracle user 
  - [ansible@ansible-server giru]$ cat hosts 

       [dbservers]
      dev-dbserver01.localdomain    ansible_python_interpreter=/usr/bin/python3.6
      dev-dbserver02.localdomain    ansible_python_interpreter=/usr/bin/python3.6

# configure root passwordless

[ansible@ansible-server giru]$ansible all -i hosts -k -u root -m authorized_key -a "user=root state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""

# configure grid passwordless

[ansible@ansible-server giru]$ ansible all -i hosts -k -u grid -m authorized_key -a "user=grid state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""


# configure oracle passwordless

[ansible@ansible-server giru]$ ansible all -i hosts -k -u oracle -m authorized_key -a "user=oracle state=present key=\"{{ lookup('file','/home/ansible/.ssh/id_rsa.pub') }}\""

# running using command  for patchng GI
ansible-playbook -i hosts apply_grid_update_main.yml -vvv
# running using command  for patchng DB
ansible-playbook -i hosts apply_db_update_main.yml -vvv
