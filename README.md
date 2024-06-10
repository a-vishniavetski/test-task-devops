# Completed Tasks
All of the tasks are completed. The application is available at: http://52.168.83.77:80 . It consists of **3 Docker Containers** running on a **Virtual Machine** on **Azure Cloud**, and can be automatically installed on a Remote or Locally using **Terraform** and **Ansible**.

### Terraform
I have written configuration files to automatically provision a Virtual Machine on Azure with an automatic setup of **InBound** **SSH** and **HTTP** connections using Terraform through **Azure Cloud Shell**. The configuration files:
```
terraform\providers.tf - Sets up Terraform providers like AzureRM.
terraform\varibles.tf - Sets up variables like the location of the resource group.
terraform\ssh.tf - Generates SSH Key-Pair to access the Virtual Machine.
terraform\main.tf - Provisions the Virtual Machine.
terraform\outputs.tf - Declares output parameters after `terraform apply` like the public IP.
```
### Ansible
I have written the following Ansible playbooks:
- `playbook_install_docker.yml` - **Installs** Docker Engine on a **Remote** or **Locally**.
- `playbook_build_images` - **Builds** Docker images and pushes them to the **Docker Hub** to a private repository.
- `playbook_deploy.yml` - **Deploys** the entire application either to Remote or Locally.

### Cloud Hosting - Microsoft Azure Cloud
I have set up:
- A Virtual Machine in the Azure Cloud. Available at: http://52.168.83.77:80
- An **HTTP Reverse Proxy** using **Nginx**
- **SSH connection** to the VM, public key added.

### Docker 
I have written:
- **Dockerfiles** for frontend and backend.
- **Docker Compose** connecting frontend, backend, and **Nginx** server functioning as **HTTP Reverse Proxy**.
- **Named Volumes** and **Hot-reloading** (with the help of nodemon).

# Instructions for the Client
### Prerequisites
- Docker Engine installed
- Docker Hub Repository
- Ansible installed
- Azure Subscription

### Deployment
1. Clone this repository using `git clone https://github.com/a-vishniavetski/test-task-devops.git` and enter it.
2. Login into **Azure Portal** using your browser and open **Azure Cloud Shell**.
3. Upload the Terraform Configuration files from the folder `terraform` into the Cloud Shell.
4. Execute the following commands to provision the Virtual Machine:

```
terraform init -upgrade
terraform plan -out main.tfplan
terraform apply main.tfplan
```

The shell will output the **Public IP** of the Virtual Machine and the **Private SSH Key**.

5. Copy the private key into a new file **`<YOUR_KEY>.pem`** in the same folder with ansible playbooks. Execute `sudo chmod 0600 <YOUR_KEY>.pem` .
6. Create a file `inventory.ini` and paste into it the following configuration, fill in the `<PLACEHOLDERS>` with your information:

```
[azure]
<PUBLIC_IP> ansible_user=azureadmin ansible_ssh_private_key_file=./<YOUR_KEY>.pem
[local]
localhost ansible_connection=local
[azure:vars]
STATIC_IP=<PUBLIC_IP>
azure_host_username=azureadmin
[local:vars]
STATIC_IP=localhost
azure_host_username=<YOUR_LOCAL_USER_WITH_SUDO_PRIVELEGES>
[all:vars]
dockerhub_login=<YOUR_DOCKERHUB_LOGIN>
dockerhub_password=<YOUR_DOCKERHUB_PASSWORD>
dockerhub_repository=<YOUR_DOCKERHUB_PRIVATE_REPOSITORY>
backend_appid=<API_KEY_FROM_OPENWEATHERMAPS>
```

7. To **install Docker**, **build the images** of the application, and **to deploy** it either to a **Remote** or **Locally**, execute the following commands:
```
ansible-playbook -i ./inventory.ini --extra-vars "target_hosts=<TARGET_HOST>" playbook_install_docker.yml
ansible-playbook -i ./inventory.ini --extra-vars "target_hosts=local" --become-user <YOUR_LOCAL_USER_WITH_SUDO_PRIVELEGES> playbook_build_images.yml
ansible-playbook -i ./inventory.ini --extra-vars "target_hosts=<TARGET_HOST>" playbook_deploy.yml
```
Where: **<TARGET_HOST>** is either **`local`** or **`azure`** respectively.

## Possible Further Improvements
- In theory, using Terraform in a local Powershell, and Bash scripting, the deployment process can be wrapped in a single Bash script with a single configuration file, which would be convenient for the client.
- HTTPS through SSL certificates.
