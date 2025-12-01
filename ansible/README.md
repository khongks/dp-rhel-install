# Instructions on how to use Ansible

## Install Ansible 

1. Get a Red Hat Linux machine as Ansible Controller.

1. Install pip first
    ```
    dnf install pip -y
    ```

1. Install pipx, assuming python3 is already installed
    ```
    python3 -m pip install --user pipx
    pipx ensurepath --global
    ```

1. If upgrade is needed
    ```
    python3 -m pip install --user --upgrade pipx
    ```

1. Install Ansible
    ```
    pipx install --include-deps ansible
    ```

## Setup Ansible

1. Clone the project
    ```
    git clone https://github.com/khongks/dp-rhel-install.git
    cd dp-rhel-install/ansible
    ```

1. Install requirements
    ```
    pip install -r requirements.txt
    ```

1. (If SSH is not present), generate one for the Ansible Controller.
    ```
    cd ~/.ssh
    ssh-keygen
    ```

1. Copy the public SSH key from the Ansible Controller to all the target RHEL servers for DataPower
    ```
    ssh-copy-id -i ~/.ssh/id_rsa.pub [user]@[hostname]
    ```

## Install using playbook

1. Go to the `dp-rhel-install/ansible/playbooks` folder

1. Under this folder, create a file called `inventory.ini` and add lines for each of the target DataPower RHEL servers.
    ```
    [dpservers]
    datapower001 ansible_host=datapower001.example.com  ansible_ssh_user=root
    datapower002 ansible_host=datapower002.example.com  ansible_ssh_user=root
    datapower003 ansible_host=datapower003.example.com  ansible_ssh_user=root
    datapower004 ansible_host=datapower004.example.com  ansible_ssh_user=root
    datapower005 ansible_host=datapower005.example.com  ansible_ssh_user=root
    ```

1. Run the sample playbook
    ```
    ansible-playbook dp-install-10.5.0.2.yml -i inventory.ini
    ```