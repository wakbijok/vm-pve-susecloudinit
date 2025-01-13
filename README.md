This Ansible playbook automates the deployment of an OpenSUSE Cloud-Init-enabled virtual machine on a Proxmox VE cluster. It handles downloading the OpenSUSE cloud image, creating the VM, configuring cloud-init, and installing basic packages.
Prerequisites

    Proxmox VE Configuration:
        Ensure Proxmox VE is installed and accessible.
        Verify the Proxmox node and storage are properly configured.

    Ansible Installation:
        Install Ansible on your local machine:

    pip install ansible

Ansible Collections:

    Install the required Ansible collections:

        ansible-galaxy collection install community.general

    SSH Access:
        Ensure passwordless SSH access is set up from your local machine to the Proxmox server.
        Add your public SSH key to Proxmox: /root/.ssh/authorized_keys.

    Configuration Files:
        Create the required group_vars files:
            group_vars/vault.yml (for sensitive variables like passwords)
            group_vars/vars.yml (for other configuration variables)

File Structure

.
├── playbooks/
│   └── deploy-vm.yml         # The Ansible playbook
├── group_vars/
│   ├── vault.yml             # Sensitive variables (encrypted with Ansible Vault)
│   ├── vars.yml              # Non-sensitive variables
└── README.md                 # Documentation

Variables
group_vars/vault.yml

Store sensitive information securely. Example:

vault_proxmox_api_password: "your_password"
vault_ssh_password: "your_password"

Encrypt the file using Ansible Vault:

ansible-vault encrypt group_vars/vault.yml

group_vars/vars.yml

Define your VM configuration variables. Example:

proxmox_api_host: "192.168.0.21"
proxmox_api_user: "root@pam"
proxmox_node: "pve-home"
storage: "local-lvm"
vm_name: "opensuse-cloudinit"
vmid: 101
static_ip: "192.168.1.100"
gateway: "192.168.1.1"

Usage

    Clone the repository or place the files in your working directory.

    Run the playbook:

    ansible-playbook playbooks/deploy-vm.yml --ask-vault-pass

    Provide the Ansible Vault password when prompted.

Workflow

    Download OpenSUSE Cloud Image:
        Downloads the latest OpenSUSE cloud image to the Proxmox VE server.

    Create the Cloud-Init VM:
        Creates a VM with the specified configuration and attaches the cloud-init disk.

    Start the VM:
        Boots the newly created VM.

    Install Basic Packages:
        Installs utilities like wget, curl, vim, etc., on the VM.

Customization

    VM Configuration: Modify variables like vm_name, vmid, cores, memory, static_ip, and gateway in vars.yml to customize the VM.

    Packages: Update the list of packages in the Install basic packages task to include any additional software you need.

Troubleshooting

    Playbook Fails with Undefined Variables: Ensure all required variables are defined in vars.yml and vault.yml.

    SSH Key Errors: Verify that your public SSH key is correctly configured in /Users/wakbijak/Nextcloud/DevOps/keys/arif.pub.

    Proxmox Errors: Check Proxmox logs (/var/log/syslog or /var/log/pve/tasks) for detailed error messages.

License

This playbook is licensed under the MIT License.