# Completed Tasks
I have done all of them.

### Terraform
- I have written configuration files to automatically provision a **Virtual Machine** on **Azure** with automatic setup of **InBound** **SSH** and **HTTP** connections.
- `terraform\providers.tf` - Sets up Terraform providers like **AzureRM**
- `terraform\varibles.tf` - Sets up variables like the location of the resource group.
- `terraform\ssh.tf` - Generates **SSH Key-Pair** to access the Virtual Machine.
- `terraform\main.tf` - Provisions the Virtual Machine.
- `terraform\outputs.tf` - Declares output parameters after `terraform apply`.
  
### Ansible
I have written the following Ansible playbooks:
- `playbook_install_docker.yml` - **Installs Docker Engine** on a **Remote** or **Locally**.
- `playbook_build_images` - Builds Docker **images** and pushes them to the **Docker Hub** to a private repository.
- `playbook_deploy.yml` - **Deploys** the entire application either to **Remote** or **Locally**

### Cloud Hosting - Microsoft Azure Cloud
I have set up:
- A **Virtual Machine** in the **Azure** Cloud. Available at: http://52.168.83.77:80
- An **HTTP Reverse Proxy** using **Nginx**
- **SSH connection to the VM**, public key added.

### Docker 
I have written:
- **Dockerfiles** for frontend and backend.
- **Docker Compose** connecting frontend, backend, and **Nginx** server functioning as **HTTP Reverse Proxy**.
- **Named Volumes** and **Hot-reloading** (with the help of nodemon).

