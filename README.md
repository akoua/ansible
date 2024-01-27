# ANSIBLE

# Setup machine
### Create machine (Macbook chip M1/M2)
- Download UTM (https://mac.getutm.app)
- After that install a distribution (https://mac.getutm.app/gallery/). I chose Archlinux ARM, because it's very light

### Configure ssh
- On your virtual machine(Managed Node), you could create a new user likewise root use, and configure a ssh for his home folder like this:
  -  ```
     $ adduser new_username
     $ passwd new_username
     $ mkdir - /home/new_username/.ssh
     $ su - new_username 
     $ ssh-keygen -t ed25519 -b 521 (install ssh before, if it's not the case)
     $ cat .ssh/userkey.pub | tee /home/new_username/.ssh/authorized_keys
     $ chown -R nom_user:new_username /home/new_username/.ssh/
     $ journalctl --since "15 minutes ago" _COMM=sshd (to inspect ssh connexion log)
     ```
- On your Control Node
  - Create too ssh, like on the Managed Node
  ```
    $ ssh-keygen -t ed25519 -b 521
    ```
  - after that you can check that your Control Node, communicate with your Managed Node
  ```
  $ ssh new_username@Managed_node_IP (you must specify the user new_username password) 
  ```
  - If it's fine you could export your ssh key, from Control Node to Managed Node
  ```
  $ ssh-copy-id new_username@Managed_node_IP
  ```
  - You could verify in Managed Node, that the public key is add in file ***/home/new_username/.ssh/authorized_keys*** like this
  ```
  $ cat /home/new_username/.ssh/authorized_keys
  ```
- Verify that from your Control Node, you can communicate with ansible to a Managed Node
  - Install python ton Managed Node, if it's not the case
  - Use this command, from Control Node
  ```
  $ ansible -i "Managed_node_IP," all -m ping
  ```
- You can named the Managed Node, by modifying a ssh config file
  - verify that, file ***.ssh/confg*** exist
  - Modifying it, like this
    ```
    HOST node_name
    Hostname Managed_node_IP
    USER new_username
    ```
  - Now you can, connect to a Managed Node with ssh like this
  ```
  $ ssh new_username@node_name
  ```
- 