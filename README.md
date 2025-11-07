# 🚀 Ansible Configuration Management – Docker Automation Task

## 📘 Project Overview
This project demonstrates the use of **Ansible** for configuration management to automatically install **Docker**, pull a **custom Docker image** from Docker Hub, and run it as a container on an **AWS EC2 instance**.

The goal of this task is to automate the deployment process using an **Ansible Playbook**, eliminating manual setup and ensuring consistency across environments.

---

## 🧰 Tools & Technologies Used
| Tool / Technology | Purpose |
|--------------------|----------|
| **Ansible** | Automation and configuration management |
| **AWS EC2 (Ubuntu)** | Cloud server for deployment |
| **Docker** | Containerization platform |
| **GitHub** | Version control and project hosting |
| **YAML** | Used to write Ansible playbooks |

---

## ⚙️ Step-by-Step Implementation

### 🖥️ 1. Setup EC2 Instance
- Launched an **Ubuntu EC2 instance** on AWS.  
- Configured **Security Group inbound rules** to allow:
  - Port `22` → for SSH connection  
  - Port `80` → for web application access  
- Connected to the instance using:
  ```bash
  ssh -i "your-key.pem" ubuntu@<your-ec2-public-ip>

## ⚙️ Step 2: Install Ansible

Updated and installed **Ansible** on the EC2 instance:

```bash
sudo apt update
sudo apt install ansible -y
```
### 🖥️ Output Example:

✅ **Ansible installed successfully.**
```bash
  ansible [core 2.x]\
  python version = 3.x
```
✅ **Ansible installed successfully.**

## 📜 Step 3: Configure Inventory File

Created an inventory file that defines the host (local EC2 instance):

```bash
cat > inventory.ini << 'EOF'
[appservers]
localhost ansible_connection=local
EOF
```
🟢 The inventory is ready — Ansible will run the playbook locally on this EC2.

## 🧾 Step 4: Create Ansible Playbook
Created a YAML playbook to automate Docker installation and container setup:
```bash
cat > deploy_docker.yml << 'EOF'
---
- name: Install Docker, pull custom image, and run container
  hosts: appservers
  become: true
  vars:
    docker_image: "amanrajraw0/mywebsite:latest"
    container_name: "mywebsite"
    bind_ports:
      - "80:80"

  tasks:
    - name: Install required packages
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
        state: present
        update_cache: yes

    - name: Add Docker GPG key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Install Docker
      apt:
        name: docker-ce
        state: latest
        update_cache: yes

    - name: Start and enable Docker service
      systemd:
        name: docker
        enabled: yes
        state: started

    - name: Pull Docker image
      command: docker pull {{ docker_image }}

    - name: Run Docker container
      command: docker run -d -p {{ bind_ports[0] }} --name {{ container_name }} {{ docker_image }}
EOF
```
## ▶️ Step 5: Execute the Playbook
Run your playbook:
```bash
ansible-playbook -i inventory.ini deploy_docker.yml
```
Check if the container is running:
```bash
docker ps
```
### ✅ Expected Output:
```bash
CONTAINER ID   IMAGE                           COMMAND   STATUS   PORTS
abcd1234efgh   amanrajraw0/mywebsite:latest    ...       Up      0.0.0.0:80->80/tcp
```
#### 🌐web app is now live at:
```bash
http://http://13.48.196.154/
```
## 🧠 What I Learned
- Automating server configuration using Ansible

- Writing and executing YAML-based playbooks

- Installing and managing Docker containers via - automation

- Deploying workloads on AWS EC2

- Using GitHub PAT tokens for secure repository access



## 📂 Repository Structure

The following files make up the **Ansible Docker Automation Project**:

| File Name | Description |
|------------|--------------|
| 🧩 **inventory.ini** | Defines the target host(s) where Ansible will deploy and manage configurations. |
| 📜 **deploy_docker.yml** | Main Ansible Playbook that automates Docker installation, image pulling, and container setup. |
| 📘 **README.md** | Project documentation and detailed report explaining setup, commands, and outputs. |

---

### 📁 Directory Overview
```bash
ansible-docker-task/
├── inventory.ini
├── deploy_docker.yml
└── README.md
```

## 🌍 How to Run This Project
Clone this repository:
```bash
git clone https://github.com/Amanrajraw0/task01-ansible-docker-config.git
cd task01-ansible-docker-config
```
Run the Ansible playbook:
```bash
ansible-playbook -i inventory.ini deploy_docker.yml
```
Verify Docker container:
```bash
docker ps
```

### 🧾 Summary
This project showcases **Infrastructure as Code (IaC)** using Ansible.
It simplifies repetitive setup processes, ensures consistent deployments, and demonstrates how automation can streamline DevOps pipelines.

By automating Docker installation and container deployment, this project proves the power of **Ansible + Docker + AWS** EC2 integration.

### 🏁 Conclusion
✅ Automated Docker installation using Ansible\
✅ Pulled and deployed custom Docker image on AWS EC2\
✅ Gained experience in configuration management and continuous deployment

#### 👨‍💻 Author
Aman Raj Raw\
🎓 B.Tech CSE | 💻 DevOps & Cloud Enthusiast\
📍 AWS | Ansible | Docker | CI/CD | Automation\
📧 Email: raj362256@gmail.com\
🌐 GitHub: Amanrajraw0







