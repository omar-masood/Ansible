### Deleting lvm process

```
---
- name: Delete LVM Setup
  hosts: dev
  become: yes
 
  tasks:
    - name: Unmount /data
      ansible.builtin.mount:
        path: /data
        state: unmounted
 
    - name: Remove logical volume
      community.general.lvol:
        vg: data_vg
        lv: data_lv
        state: absent
        force: true
 
    - name: Remove volume group
      community.general.lvg:
        vg: data_vg
        state: absent
        force: true
 
    - name: Remove physical volume (partition)
      community.general.parted:
        device: /dev/sdb
        number: 1
        state: absent
 
    - name: Remove /data directory
      ansible.builtin.file:
        path: /data
        state: absent
```
