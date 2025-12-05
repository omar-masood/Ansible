### Partitioning disk, physical volume, volume group, logical volume, create filesystem, mounting directory

```

---                                                                                                             
- name: Test Playbook
  hosts: dev
  become: yes
  tasks:
    - name: create disk partition
      ansible.builtin.parted:
        device: /dev/sdb
        number: 1
        state: present
        part_type: primary
        part_start: 0%
        part_end: 100%
    - name: create physical volume
      ansible.builtin.command: 
        cmd: pvcreate /dev/sdb1
      args:
        creates: /dev/sdb1
    - name: create volume group
      ansible.builtin.command: 
        cmd: vgcreate data_vg /dev/sdb1
      args:
        creates: /dev/data_vg
    - name: create logical volume
      ansible.builtin.command: 
        cmd: lvcreate -L 1G -n data_lv data_vg
      args:
        creates: /dev/data_vg/data_lv
    - name: create filesystem
      ansible.builtin.filesystem:
        fstype: xfs
        dev: /dev/data_vg/data_lv
    - name: create mount directory
      ansible.builtin.file:
        path: /data
        state: directory
    - name: mount logical volume
      ansible.builtin.mount:
        path: /data
        src: /dev/data_vg/data_lv
        fstype: xfs
        state: mounted

```
