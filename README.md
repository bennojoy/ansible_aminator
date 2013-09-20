ansible_aminator
================

### To create a base vm image we first deploy a vm in ec2 and then copy the netoss repo there, and then run the shell scripts
which creates the base image and registers it as an AMI.

        ansible-playbook -i hosts create_ami.yml -e id=abcd
Note: The task of running the shell script may hang, if so run the last command manually ---- needs fixing.

### Once the ami is created , use that ami to create aminator instance. Edit the playbook to add the newly created ami id.

        ansible-playbook -i inventory/dev create_aminator.yml -e id=amin6

### After Creating the instance for Aminator, run the playbook which install aminator.

         ansible-playbook playbooks/aminator-ubuntu.yml -i inventory/ec2.py -l 'tag_Name_Aminator' -vv

Note: it uses ec2 inventory script so make sure credentials are there in the environment.

### Once the aminator instance is created log on to the machine and as root run following commands.

Make sure euca credentials are provided.
To create a AWX 1.3 release AMI

        aminate -e ec2_ansible_linux -B ami-c7a3e8ae awx_13.yml

Note: -B is the base AMI created -n can be given to give it a name.

To make the image public issue the following command.

        euca-modify-image-attribute -l -a all ami-010e4468

To distribute the ami to all regions parallley.

        distami -p ami-abcd1234
 

        
