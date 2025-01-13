# OpenSUSE Cloud-Init VM Deployment for Proxmox

This Ansible playbook automates the deployment of an OpenSUSE Cloud-Init-enabled virtual machine on a Proxmox VE cluster. The automation handles multiple tasks including downloading the OpenSUSE cloud image, creating the VM, configuring cloud-init, and installing essential packages.

## Prerequisites

### Proxmox VE Configuration

Before you begin, ensure you have:

- A properly installed and accessible Proxmox VE instance
- Correctly configured Proxmox node and storage settings

### Required Software

1. **Ansible Installation**

   Install Ansible on your local machine:
   ```bash
   pip install ansible
   ```

2. **Ansible Collections**

   Install the required community collection:
   ```bash
   ansible-galaxy collection install community.general
   ```

### Access Requirements

- Configure passwordless SSH access from your local machine to the Proxmox server
- Add your SSH public key to Proxmox: `/root/.ssh/authorized_keys`

## Project Structure

```
.
├── playbooks/
│   └── deploy-vm.yml         # Main Ansible playbook
├── group_vars/
│   ├── vault.yml             # Encrypted sensitive variables
│   ├── vars.yml              # Configuration variables
└── README.md                 # This documentation
```

## Configuration

### Sensitive Variables (group_vars/vault.yml)

Create and encrypt your sensitive variables file with the following structure:

```yaml
vault_proxmox_api_password: "your_password"
vault_ssh_password: "your_password"
```

Encrypt the file using:
```bash
ansible-vault encrypt group_vars/vault.yml
```

### Configuration Variables (group_vars/vars.yml)

Set your VM configuration variables:

```yaml
proxmox_api_host: "192.168.0.21"
proxmox_api_user: "root@pam"
proxmox_node: "pve-home"
storage: "local-lvm"
vm_name: "opensuse-cloudinit"
vmid: 101
static_ip: "192.168.1.100"
gateway: "192.168.1.1"
```

## Deployment

1. Clone this repository or copy the files to your working directory
2. Run the deployment playbook:
   ```bash
   ansible-playbook playbooks/deploy-vm.yml --ask-vault-pass
   ```
3. Enter your Ansible Vault password when prompted

## Deployment Process

The playbook executes the following steps:

1. **Image Download**: Retrieves the latest OpenSUSE cloud image
2. **VM Creation**: Sets up the VM with the specified configuration
3. **Cloud-Init Integration**: Configures and attaches the cloud-init disk
4. **VM Initialization**: Boots the newly created VM
5. **Package Installation**: Installs basic utility packages

## Customization Options

### Virtual Machine Settings

Modify the following variables in `vars.yml` to customize your VM:
- `vm_name`: Virtual machine name
- `vmid`: Unique VM identifier
- `cores`: Number of CPU cores
- `memory`: RAM allocation
- `static_ip`: Fixed IP address
- `gateway`: Network gateway

### Package Configuration

Customize the package installation task in the playbook to include additional software based on your requirements.

## Troubleshooting Guide

### Common Issues

1. **Undefined Variables**
   - Verify all required variables are properly defined in `vars.yml` and `vault.yml`
   - Check variable naming consistency

2. **SSH Authentication Failures**
   - Verify SSH key path: `/Users/wakbijak/Nextcloud/DevOps/keys/arif.pub`
   - Ensure correct permissions on SSH keys
   - Check SSH key deployment on Proxmox

3. **Proxmox-Related Errors**
   - Check Proxmox logs:
     - `/var/log/syslog`
     - `/var/log/pve/tasks`
   - Verify Proxmox API accessibility
   - Confirm storage availability

## License

This project is licensed under the MIT License.
