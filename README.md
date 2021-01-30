# CookieCutter Template For Ansible Role

## What is cookiecutter
A command-line utility that creates projects from cookiecutters templates. It uses a templating system — Jinja2 — to replace or customize folder and file names, as well as file content.

## Why cookiecutter
Cookiecutter saves time constructing a new repository, to avoid forgetting mandatory files like Readme or Changelog etc. Also helps to enforce the standards. 

> :bulb: Complete documentation is at @ https://cookiecutter.readthedocs.io/en/latest/


## Repository Structure

```
├── cookiecutter.json
├── {{cookiecutter.role_name}}
├── hooks
│   └── post_gen_project.py
├── LICENSE
└── README.md
```

- **cookiecutter.json:**  
    The template contains the values to be replaced for the projects when triggered.

- **{{cookiecutter.role_name}}:**
    directory contains the ansible role structure with molecule.

- **hooks:**
    You can have Python or Shell scripts that run before and/or after your project is generated.
        
    Make sure your hook scripts work in a robust manner. If a hook script fails (that is, if it finishes with a nonzero exit status), the project generation will stop and the generated directory will be cleaned up.

- **LICENSE:**
    License file that contanins the authenticity , copyright and autorization info.


## Pre-Requisites:

- `python3.6` and `virtualenv` : To create virtual env using python3.6.
```
    $ python3 -m venv .cookiecutter-venv  && source .cookiecutter-venv/bin/activate
```
- `molecule` package must be installed in virtual env.
```
    $ source .cookiecutter-venv/bin/activate
    $ pip install cookiecutter
```
- `git` and `git subtree` commands must be available. You must have access to git

```
    $ source .cookiecutter-venv/bin/activate
    # go to a directory where it's ok to put temporary stuff
    $ cd ~
    $ git clone git://github.com/apenwarr/git-subtree.git
    $ cd git-subtree/
    # shell script does the job for you.
    $ sudo sh ./install.sh
    cd ..
    # remove the git cloned stuff, now that all relevant things have been copied (we hope)
    $ rm -rf git-subtree

    # test that it works
    $ git subtree
    # should print some help/usage stuff.
```


## How cookiecutter works
Cookiecutter can be used either with a Git repository, with a folder or even a Zip file. Also, if working with repositories, you can start your template from any branch.

Cookiecutter takes a source directory tree and copies it into your new project. It replaces all the names that it finds surrounded by templating tags {{ and }} with names that it finds in the file cookiecutter.json. That’s basically it.



### Generate ansible role structure using cookiecutter template:

**Note**: Below template command must be run inside your cloned ansible role repository.

```
    $ source .cookiecutter-venv/bin/activate
    $ git clone <ansible-role-xxx>
    $ cd ansible-role-xxx
    $ cookiecutter git@github.com:stackresilient/cookiecutter-ansible-role.git
```

- Molecule init template command uses the cookiecutter command internally to generate the role structure from the template.
- It will also create git subtree in your ansible cloned repository.

### Example:

```
$ source .cookiecutter-venv/bin/activate
$ cookiecutter template git@github.com:stackresilient/cookiecutter-ansible-role.git
    --> Initializing new role role_name...
    role_name [role_name]: test-role
    contributor_full_name [Spyridon Danousis]: 
    email [sdanousis@gmail.com]: 
    author [Spyridon Danousis]: 
    description [your description]: test role
    company [Spyridon Danousis]: 
    Select license:
    1 - BSD
    2 - MIT
    3 - GPLv2
    4 - GPLv3
    5 - Apache
    6 - CC-BY
    Choose from 1, 2, 3, 4, 5, 6 [1]: 1
    min_ansible_version [2.8]: 
    allow_duplicates [no]: 
    release_date [2019-06-05]: 
    year [2019]: 
    version [1.0.0]: 
    repo_name [ansible-role-xxx]: ansible-role-test-role

    ROLE CONFIGURATION:
    ===================

    Should it have tasks?  [yes]: 
    Add task name i.e (Install packages) install test pkg
    Add task name i.e (Install packages) 

    Should it have handlers? [yes]: 
    Add handler name i.e (Restart uwsgi) start test service
    Add handler name i.e (Restart uwsgi) 

    It should contain default variables?:  [yes]:
    Add variable i.e (operator: drunken_master) test: test
    Add variable i.e (operator: drunken_master) 

    Should it have meta info?  [yes]: 
    - Should it have dependencies?  [yes]: 
        Add dependency i.e ({role: aptsupercow, var: 'value'}) 

    Should it have templates?  [yes]: 

    Should it have files?  [yes]: 

    Initialized role in /Users/test/Desktop/templates/cookiecutter-ansible-role/role_name successfully.
```

## To test the ansible role using molecule framework
  We can test the cookicutter generated ansible role using molecule framework. 
    For more info go to [molecule page](docs/molecule.md).
