# ansible

Project for Ansible Automation

## Basic Setup

1. Install ansible on the machine from which you will be running Ansible.
2. Create an ssh key specifically for Ansible.\
   a. Go to the `~/.ssh` directory.\
   b. Run `ssh-keygen -t ed25519 -C "SSH key for Ansible automation" -f ansible`.

   > Note: This generates a key of type ed25519, but the `-f ansible` is giving it a name of "Ansible" so as to not overwrite the existing key.\

   c. Copy the `ansible.pub` that is created to each server you will be managing:\
   `ssh-copy-id -i ~/.ssh/ansible.pub <ip address of remote machine>`\
   d. Verify that the key copied by going to the `~/.ssh` directory of the remote machine and checking the output of the `authorized_keys` entry: `cat authorized_keys`. If successful you see an entry such as this:\

   > `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPUrX0v1GcWyIARF8v16LcDjLDsB0XVPSUweXdZmttn8 SSH key for Ansible automation`

3. Create an inventory file and put the IP addresses of the servers that Ansible will manage:\
   a. Run `nano inventory`.\
   b. Create the entries and save the file.
4. From within the local repository,, run an ansible command to make sure that everything is working:\
   a. The command `ansible all --key-file ~/.ssh/ansible -i inventory -m ping` uses the IP addresses defined in the inventory file to make a connection. If successful you see output similar to this:
   ```bash
   192.168.1.150 | SUCCESS => {
       "ansible_facts": {
           "discovered_interpreter_python": "/usr/bin/python3.11"
       },
       "changed": false,
       "ping": "pong"
   }
   192.168.1.65 | SUCCESS => {
       "ansible_facts": {
           "discovered_interpreter_python": "/usr/bin/python3.11"
       },
       "changed": false,
       "ping": "pong"
   }
   ```
5. Create an ansible config file:\
   a. Run `nano ansible.cfg`.\
   b. Add these entries to the file:\
   ```bash
   [defaults]
   inventory = inventory
   private_key_file = ~/.ssh/ansible
   ```
6. Run `ansible all -m ping` to see the same output from the command in step 4 about. Creating the ansible config file shortens the command since the ssh key and inventory file to use are specified in the file.

### Useful commands

| Command                                            | Purpose                                                          |
| -------------------------------------------------- | ---------------------------------------------------------------- |
| `ansible all --list-hosts`                         | Lists all of the IP addresses defined in the inventory           |
| `ansible all -m gather_facts`                      | Runs the gather_facts module to pull info about the server, etc. |
| `ansible all -m gather_facts --limit <ip address>` | Limits to a specific IP address                                  |
