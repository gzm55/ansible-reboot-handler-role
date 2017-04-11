reboot_handler
==============

Handle reboot notification, reboot remote UNIX/Linux machines and wait them back.

This handler listens a message of "reboot system",
provides variables to skip reboot or skip the confirmation before rebooting.

Requirements
------------

* Ansible >= 2.2, we are `listen` the notifications
* jinja2 >= 2.6
* permissions to `become` root
* remote machines must be rebootable via `shutdown` or `reboot` commmands
* the connections of remote machines must be in `ssh`, `smart`, `paramiko` or `funcd`

Role Variables
--------------

Variable `skip_reboot`, default `True`.
This variable should be defined in host/group vars.
The reboot action is dangerous in many situations,
so we will not reboot the machine unless the `skip_root` for a `inventory_hostname` is set to `True`.

Variable `reboot_confirm`, default `True`.
if set to `True`, the handler will trigger a prompt once for a batch,
the prompting message contains the list of rebooting machines in this batch.
Press Ctrl+C and A to abort the playbook, or press Enter key to continue.

Dependencies
------------

* role `gzm55.require_implicity_localhost`
* role `gzm55.require_disabe_become`
* role `gzm55.local_id_plugin`
* role `gzm55.require_become_root`

Example Playbook
----------------

    - hosts: servers
      roles:
      - { role: user.reboot_handler, reboot_confirm: True }
      tasks:
      - name: change system
        raw: 'true'
        notify: "reboot system"

Developing
----------

The testing progress is based on molecule with vagrant driver which is
not supported by travis-ci.
So in each release, we have to run `molecule test` on a developing machine,
and then galaxy import manually.

TODO: use `wait_for_connection` module (ansible >= 2.3) to wait machines comming back.

License
-------

BSD
