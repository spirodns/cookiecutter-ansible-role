# {{cookiecutter.role_name}}
=========

{{cookiecutter.description}}

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

## Usage

```sh
ansible-galaxy install  git+https://github.com/stackresilient/ansible-role-{{ cookiecutter.role_name }}.git,v{{ cookiecutter.version }} 
```

To update the existing role version

```sh
ansible-galaxy install  git+https://github.com/stackresilient/ansible-role-{{ cookiecutter.role_name }}.git,v{{ cookiecutter.version }}  --force
```

For more info, pls visit to [installation page](https://docs.ansible.com/ansible/latest/reference_appendices/galaxy.html#installing-multiple-roles-from-a-file).

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: {{ cookiecutter.role_name }}, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).


## CONTRIBUTING

`git clone git@github.com:{{cookiecutter.github_user}}/{{cookiecutter.repo_name}}`

---
Copyright Â© {{cookiecutter.year}}, {{ cookiecutter.contributor_full_name }}
