# Molecule : To test Ansible Roles
Molecule is designed to aid in the development and testing of Ansible roles.
More info can be found at https://molecule.readthedocs.io/en/stable/.

# Repo Structure:

The directory structure for the molecule testing is based on which driver you selected will be as following

- **docker**: 
    As the name states, this folder is used to contains all info related to docker driver of molecule.

If docker driver is selected then directory structure inside molecule folder will be as following:

```
docker
├── Dockerfile.j2
├── INSTALL.rst
├── molecule.yml
└── playbook.yml

```    


If ec2 driver is selected then then directory structure inside molecule folder will be as following:

- **ec2**: 
    As the name describes , this folder is used to contains all info related to ec2 driver of molecule.

```
ec2
├── inventory
    └── host_vars
        └── ec2-amzlinux2.yml
├── INSTALL.rst
├── create.yml
├── destroy.yml
├── molecule.yml
└── playbook.yml
└── prepare.yml
```

     
- **requirements.txt**:
    This file contains all the necessary python packages that need to be installed for given molecue driver molecule.


## Pre-Requisites for molecule test:

- python3.6 and virtualenv : To create virtual env using python3.6 for testing ansible role using molecule framework.
- Export AWS_REGION as an environment variable
- [Optional] Create a tag named `Purpose = Test` ( Purpose as Key and Testing as KeyValue) for the target subnet that will be used for the molecule testing


## Steps to prepare an `existing ansible role` for molecule test

1. Get Cli access to the AWS account

2. After successful run of okta-cli.py, run the folowing commands to export the creds.

    ```
        $ export AWS_ACCESS_KEY_ID=$(aws configure --profile default get aws_access_key_id);export AWS_SECRET_ACCESS_KEY=$(aws configure --profile default get aws_secret_access_key);export AWS_SESSION_TOKEN=$(aws configure --profile default get aws_session_token)
        $ export AWS_REGION=us-east-1
    ```

3. Get the ansible role that we want to test
   ```
      $ git clone <ansible-role-xxx>  
   ```

4. Create and activate a virtual environment for molecule testing.
    ```
      $ python3 -m venv .molecule-venv  && source molecule-venv/bin/activate
    ```


5. After creating the virtual env, run the following command to test your ansible role
    
    **Note**  : Replace {driver} by the valid molecule driver with which you want to test your role. Valid molecule drivers are ec2,docker. If we select the ec2 then it will use an ec2 instance to test our role and if we select the docker driver then it will use docker container to test our ansible role. We can also test the same role with multiple drivers at the same time, but for that we need to install the reqirements of all drivers mentioned in requirements.txt file inside molecule/shared-resources folder.

    Run the following command in `molecule-venv` virtualenv to install the requirements for molecule testing framework based on driver type.

    ```
       $ cd ansible-role-xxx/molecule/shared-resources
       $ pip install -r requirements.txt

    ```

6. Driver configuration.
-  Required if molecule driver is ec2:
   Update/Create var file to configure molecule ec2 driver  at `molecule/ec2/inventory/group_vars/all.yml`. Below is the sample of all.yml
   ```
    ---
      branch: master
      aws_resource_name_tag_value: "{{ lookup('env', 'MOLECULE_PROJECT_DIRECTORY') | basename }}-{{ branch }}-"
      security_group_description: Security group for testing Molecule
      sg_ing_rules: []
      keypair_path: "{{ lookup('env', 'MOLECULE_EPHEMERAL_DIRECTORY') }}/ssh_key"
      ssh_user: ec2-user
      ssh_port: 22
      ansible_user: "{{ ssh_user }}"
      ansible_private_key_file: "{{ keypair_path }}"
      instance_type: t2.micro
      security_group_rules:
        ingress:
          - proto: tcp
            from_port: 22
            to_port: 22
          - proto: icmp
            from_port: 8
            to_port: -1
        egress:
          - proto: -1
            from_port: 0
            to_port: 0
            cidr_ip: '0.0.0.0/0'
   ```

## To Run Molecule test with diff matrix
**Note:** All below commands must be run inside molecule-venv virtualenv

   Before running molecule test , ansible role must be prepared with molecule venv.

- To find lint related error in your ansible role run following commands
  ```
    $ cd ansible-role-xxx
    $ molecule  lint -s ${driver}
  ```
    Above command will perform testing using three tools (Yamllint, Flake8 ,Ansible Lint)

- To just run the playbook run the following commands:
  ```
    $ cd ansible-role-xxx
    $ molecule  converge -s ${driver}
  ```
    This will deploy the nginx role on given platform for Ex.(ec2/docker).

- To verify the functionality of role using unit and integration tests
  ```
    $ cd ansible-role-xxx
    $ molecule  verify -s ${driver}
  ```
    **Note:** This will only run python scripts that are placed under molecule/shared-resources/tests folder that has prefix `test_`.

- To run all the matrix for molecule test, run
  ```
    $ cd ansible-role-xxx
    $ molecule  test -s ${driver}
  ```

 - To destroy the test instance/container
   ```
     $ cd ansible-role-xxx
     $ molecule  destroy -s ${driver}
   ```

**Notes:** 
- Replace the ${driver} by valid molecule driver i.e `ec2`.
- In our case driver and scenario names are the same, which means if we select the ec2 driver then it will create a scenario called `ec2`.
- To enable the debug mode use _--debug_ argument to the molecule command: `$ molecule --debug [sub-commands]"`
