Anlibe Role: WKD
================

[![Build Status](https://travis-ci.org/znerol/ansible-role-wkd-gpg.svg?branch=master)](https://travis-ci.org/znerol/ansible-role-wkd-gpg)

Provides tasks to export GPG keys into a [Web Key Directory][1] structure.

Requirements
------------

None

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

Usage of `znerol.wkd_gpg` role:

    - hosts: localhost
      vars:
        wkd_gpg_uids:
          - "Joe.Doe@Example.ORG"
          - "joe.doe@Example.com"
          - "test-wkd@example.org"
          - "me@example.com"
          - "äëöüï@example.org"
          - "foo@example.com"
        wkd_gpg_key_directory: "/tmp/{{ wkd_gpg_uid | wkd_dir(wkd_method='advanced') }}"

      tasks:
        - name: Role znerol.wkd_gpg imported
          import_role:
            name: znerol.wkd_gpg

        - name: WKD directory present
          loop: "{{ wkd_gpg_uids | list }}"
          loop_control:
            loop_var: wkd_gpg_uid
          file:
            state: directory
            path: "{{ wkd_gpg_key_directory }}"
            recurse: true

    - name: GPG key exported
      loop: "{{ wkd_gpg_uids | list }}"
      loop_control:
        loop_var: wkd_gpg_uid
      include_role:
        name: znerol.wkd_gpg
        tasks_from: wkd_key

License
-------

MIT

[1]: https://wiki.gnupg.org/WKD
