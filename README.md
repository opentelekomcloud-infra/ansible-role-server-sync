server_sync
=========

A tiny role, which establish synchronization of configured folders between hosts in the group.

Requirements
------------

Inventory group of sync_hosts must look like follow:
   sync_group:
     hosts:
       proxy1:
       proxy2:
     vars:
       sync_folders: ['/etc/ssl/nginx/', '/etc/letsencrypt/']
       post_sync_hook: 'systemctl restart nginx'

If host defines variable "server_sync_master: true" the systemd timer on the host will be enabled to execute daily ansible playbook (ansible-playbook -i /etc/server_sync/inventory /etc/server_sync/sync.yml).

Example
-------
  
  - name: Generate/detect SSH keys for servers
    hosts: sync_group
    tasks:
      - include_role:
          name: opentelekomcloud_infra.server_sync
          tasks_from: generate_ssh_key.yml
  
  - name: Gather SSH keys per groups
    hosts: sync_group
    tasks:
  
      - include_role:
          name: opentelekomcloud_infra.server_sync
        vars:
          sync_group_name: sync_group
