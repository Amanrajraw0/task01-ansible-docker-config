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
⚙️ 2. Install Ansible
Updated system packages and installed Ansible using the following commands:

bash
Copy code
sudo apt update
sudo apt install ansible -y
Verified installation:

bash
Copy code
ansible --version
Output example:

java
Copy code
ansible [core 2.x]
python version = 3.x
🟢 Ansible is now installed successfully and ready for automation tasks.

📜 3. Configure Inventory File
Created an inventory file using the cat command to define target hosts:

bash
Copy code
cat > inventory.ini << 'EOF'
[appservers]
localhost ansible_connection=local
EOF
This tells Ansible to run the playbook locally on the EC2 instance (since we’re testing on the same machine).

🟢 Inventory file configured successfully.

⚙️ 4. Create Ansible Playbook
Created the main playbook named deploy_docker.yml using the following command:

bash
Copy code
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
🟢 Playbook successfully created.
It automates:

Installing Docker

Pulling your image from Docker Hub

Running the container automatically on port 80

▶️ 5. Execute the Playbook
Run the playbook to apply the automation:

bash
Copy code
ansible-playbook -i inventory.ini deploy_docker.yml
Check if the container is running:

bash
Copy code
docker ps
✅ Expected Output:

bash
Copy code
CONTAINER ID   IMAGE                           COMMAND   STATUS   PORTS
abcd1234efgh   amanrajraw0/mywebsite:latest    ...       Up      0.0.0.0:80->80/tcp
Your web app is now live on your EC2’s public IP at port 80.

🧠 What I Learned
How to automate server configuration using Ansible

Writing and executing YAML playbooks

Installing and managing Docker containers with Ansible

Deploying and testing on AWS EC2

Using GitHub PAT tokens for secure repository access

📂 Files in This Repository
File Name	Description
inventory.ini	Defines Ansible target host (EC2 instance)
deploy_docker.yml	Main automation playbook
README.md	Project documentation and report

🌍 How to Run This Project
Clone this repository:

bash
Copy code
git clone https://github.com/Amanrajraw0/task01-ansible-docker-config.git
cd task01-ansible-docker-config
Run the playbook:

bash
Copy code
ansible-playbook -i inventory.ini deploy_docker.yml
Verify Docker container:

bash
Copy code
docker ps
🧾 Summary
This project demonstrates how Ansible can simplify complex setup processes using Infrastructure as Code (IaC).
By automating Docker installation and deployment, we ensure fast, repeatable, and error-free setups — perfect for real-world DevOps workflows.

🏁 Conclusion
Through this task, I successfully:

Automated Docker installation using Ansible

Pulled and deployed a Docker image automatically on AWS EC2

Understood the workflow of configuration management and continuous deployment

This task highlights the real-world importance of automation in DevOps.

👨‍💻 Author
Aman Raj Raw
🎓 B.Tech CSE | 💻 DevOps & Cloud Enthusiast

📍 AWS | Ansible | Docker | CI/CD | Automation
📧 Email: amanrajraw@example.com
🌐 GitHub: Amanrajraw0