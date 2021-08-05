app-sbgrid-server
=========

This Ansible role configures a shared applications directory for software packaged by the [SBGrid Consortium](https://sbgrid.org/).  It makes use of a shared NFS mountpoint and is intended for GNU/Linux workstations in a networked environment.  Presently only RHEL family distributions are supported.  This particular role configures the server that will regularly download and update packages from the SBGrid Consortium.  In particular it performs a full installation with `sbgrid-admin -i` and installs a cron job that regularly runs `sbgrid-admin -c` every 15 minutes.

This role also has several important tags that can selectively narrow down the tasks that are run:

  * `sbgrid_server_crononly` will only add or update the `sbgrid-admin -c` cronjob.
  * `sbgrid_server_fullinstall` will run `sbgrid-admin -i` and perform a full installation of the SBGrid consortium packages.

If these tags are not applied, neither the task that updates the cronjob nor the task that performs the installation will be run.  It is important then that the intial run using this role uses both of these tags.  This is enforced by use of the `never` tag (see [here](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html)).

Requirements
------------

An NFS mount must be set up and exported.  The set up of this NFS mount is beyond the scope of this role and should be done in another playbook/role.

Role Variables
--------------
`sb_sitename` (string) : Your SBGrid site name as provided by the SBGrid Consortium (see [here](https://sbgrid.org/wiki/usage/installation_admin)).

`sb_sitekey` (string) : Your SBGrid site key as provided by the SBGrid Consortium (see [here](https://sbgrid.org/wiki/usage/installation_admin)).

`sbgridrc` (dict) : A dictionary containing all of the environment variables that should be defined in `.sbgridrc` (see [here](https://sbgrid.org/wiki/usage/installation_admin)).  Keys are identical to the environment variables but use lowercase instead of uppercase.

`sb_install_target` (string) : A path to where the software should be installed.

License
-------

Revised 3-Clause BSD License
