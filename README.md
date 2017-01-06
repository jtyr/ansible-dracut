dracut
======

Ansible role which helps to install and configure Dracut.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```
---

- name: Default usage with no extra modules configured
  hosts: all
  roles:
    - dracut

- name: Configure some extra options in the main config file
  hosts: all
  vars:
    # Main configuration
    dracut_config:
      # Add more drivers
      # (note the mandatory "+" sign indicating that the value is a list)
      add_drivers+: >-
        hv_netvsc
        hv_storvsc
        hv_vmbus
      # Disable adding LVM confign if not using LVM
      # (the value must be quoted to result in "no" and not "false")
      lvmconf: "no"
  roles:
    - dracut

- name: Create additional config file in the conf.d directory
  hosts: all
  vars:
    # Deleta all unknown conf.d files
    dracut_confd_manage: yes
    # Create new
    dracut_confd:
      # This is the name of the conf file in the conf.d directory
      hyperv_drivers:
        # This is the actual configuration inside the file
        add_drivers+: >-
          hv_netvsc
          hv_storvsc
          hv_vmbus
      # This is the name of another file
      virtio_drivers:
        # This is again the actual configuration inside the file
        add_drivers+: >-
          virtio
          virtio_blk
          virtio_net
          virtio_pci
          virtio_ring
      # To delete a file, set the value to null
      other_drivers: null
  roles:
    - dracut
```


Role variables
--------------

Variables used by the role:

```
# Main Dracut package (explicit version can be specified here)
dracut_pkg: dracut

# List of Dracut modules to be installed
dracut_modules: []

# Path to the Dracut config
dracut_config_file: /etc/dracut.conf

# Path to the Dracut conf.d directory
dracut_config_dir: /etc/dracut.conf.d

# Wheter to manage files in the conf.d directory
# (all files not defined in the dracut_confd variable will be deleted)
dracut_confd_manage: no

# Dracut config
# (check the README file for more info)
dracut_config: {}

# Dracut conf.d configs
# (check the README file for more info)
dracut_confd: {}

# Automatically build a new initramfs if configuration changed
dracut_initramfs_build: no

# Command used to build the new initramfs
dracut_initramfs_cmd: dracut -f -v
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
