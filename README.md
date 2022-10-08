# ROLE

Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

## Role Directory Structure
```
site.yml
webservers.yml
fooservers.yml
roles/
   common/
     tasks/
     handlers/
     files/
     templates/
     vars/
     defaults/
     meta/
   webservers/
     tasks/
     defaults/
     meta/
```

tasks - contains the main list of tasks to be executed by the role.

handlers - contains handlers, which may be used by this role or even anywhere outside this role.

defaults - default variables for the role (see Using Variables for more information).

vars - other variables for the role (see Using Variables for more information).

files - contains files which can be deployed via this role.

templates - contains templates which can be deployed via this role.

meta - defines some meta data for this role. See below for more details.

Roles expect files to be in certain directory names. Roles must include at least one of these directories, however it is perfectly fine to exclude any which are not being used. When in use, each directory must contain a main.yml file, which contains the relevant content:

```
  roles/
      common/               # this hierarchy represents a "role"
          tasks/            #
              main.yml      #  <-- tasks file can include smaller files if warranted
          handlers/         #
              main.yml      #  <-- handlers file
          templates/        #  <-- files for use with the template resource
              ntp.conf.j2   #  <------- templates end in .j2
          files/            #
              bar.txt       #  <-- files for use with the copy resource
              foo.sh        #  <-- script files for use with the script resource
          vars/             #
              main.yml      #  <-- variables associated with this role
          defaults/         #
              main.yml      #  <-- default lower priority variables for this role
          meta/             #
              main.yml      #  <-- role dependencies
          library/          # roles can also include custom modules
          module_utils/     # roles can also include custom module_utils
          lookup_plugins/   # or other types of plugins, like lookup in this case

      webtier/              # same kind of structure as "common" was above, done for the webtier role
      monitoring/           # ""
      fooapp/               # ""
```



# Directory layout
## Sample directory layout
This layout organizes most tasks in roles, with a single inventory file for each environment and a few playbooks in the top-level directory:

```
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1.yml             # here we assign variables to particular groups
   group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # main playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier
tasks/                    # task files included from playbooks
    webservers-extra.yml  # <-- avoids confusing playbook with task files
roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```

>>Note

By default, Ansible assumes your playbooks are stored in one directory with roles stored in a sub-directory called roles/. As you use Ansible to automate more tasks, you may want to move your playbooks into a sub-directory called playbooks/. If you do this, you must configure the path to your roles/ directory using the roles_path setting in ansible.cfg.

## Alternative directory layout
Alternatively you can put each inventory file with its group_vars/host_vars in a separate directory. This is particularly useful if your group_vars/host_vars don’t have that much in common in different environments. The layout could look something like this:


```
inventories/
   production/
      hosts               # inventory file for production servers
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         hostname1.yml    # here we assign variables to particular systems
         hostname2.yml

   staging/
      hosts               # 
      +++++++++++++++++++++++++++++++++inventory file for staging environment
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         stagehost1.yml   # here we assign variables to particular systems
         stagehost2.yml

library/
module_utils/
filter_plugins/

site.yml
webservers.yml
dbservers.yml

roles/
    common/
    webtier/
    monitoring/
    fooapp/
```

This layout gives you more flexibility for larger environments, as well as a total separation of inventory variables between different environments. However, this approach is harder to maintain, because there are more files. 

# Project Layout
The Project propose:

host_vars - contains host configuration ansible specific values for ansible_connection, ansible_user, ansible_password (GPG value), ansible_sudo_pass etc
roles - based on Sample structure
.ansible-lint
ansible.cfg
hosts.ini
playbook1.yml


References:
- https://docs.ansible.com/ansible/2.8/user_guide/playbooks_reuse_roles.html
- https://docs.ansible.com/ansible/latest/user_guide/sample_setup.html
