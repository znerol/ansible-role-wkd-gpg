Anlibe Role: WKD
================

[![Build Status](https://travis-ci.org/znerol/ansible-role-wkd-gpg.svg?branch=master)](https://travis-ci.org/znerol/ansible-role-wkd-gpg)

Provides tasks to export GPG keys into a [Web Key Directory][1] structure.


Requirements
------------

Requires Python 3 on the ansible controller machine.


Required Role Variables
-----------------------

* `wkd_gpg_uids`: List of GPG uids to export. Note that the playbook loops over
  this list using the `loop_var` set to `wkd_gpg_uid`.
* `wkd_basedir`: Path to the base directory where keys are exported to. This is
  typically set to the webserver document root.


Optional Role Variables
-----------------------

* `wkd_method`: Either `direct` or `advanced` (see section Key Discovery in
  [draft standard][4]). Defaults to `advanced`.
* `wkd_gpg_export_dest`: Path where the GPG keys will be exported to. Defaults
  to a heavily templetad string, see the source.
* `wkd_gpg_export_params`: A hash of additional parameters passed to
  [znerol.gpg\_export][3] lookup plugin. Especially useful is `homedir` in
  order to set the gnupg home to a directory with a source controlled public
  keyring and no private keys.


Dependencies
------------

- [znerol.wkd][2]
- [znerol.gpg\_export][3]


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
        wkd_basedir: "/var/www"

      tasks:
        - name: Role znerol.wkd_gpg imported
          import_role:
            name: znerol.wkd_gpg

License
-------

MIT

[1]: https://wiki.gnupg.org/WKD
[2]: https://galaxy.ansible.com/znerol/wkd
[3]: https://galaxy.ansible.com/znerol/gpg_export
[4]: https://tools.ietf.org/html/draft-koch-openpgp-webkey-service
