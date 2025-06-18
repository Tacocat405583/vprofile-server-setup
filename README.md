# VProfile

VProfile is a Java-based web application demonstrating a backend stack integration using Apache Tomcat, Nginx, MySQL, RabbitMQ, and Memcached. This project serves as a foundation for building scalable and efficient web applications.

---

## Features

- **Java Web Application** running on Apache Tomcat  
- **Nginx** as frontend web server and load balancer  
- **MySQL** database for persistent data storage  
- **RabbitMQ** message broker for asynchronous communication  
- **Memcached** caching to speed up data retrieval  



## Architecture Overview

[Client] <---> [Nginx] <---> [Tomcat (Java WebApp)]
|
v
[RabbitMQ] [Memcached]
|
v
[MySQL Database]

### Jenkins Automation

This project includes a Jenkins pipeline to automate building, testing, and deploying the application.
Special thanks to the original repository from hkhcoder. Huge thanks for their contributions for their part in helpign me find what I love doing as a developer.

### Jenkinsfile Overview

The `Jenkinsfile` defines a multistage pipeline with the following stages:

- **Checkout** – Pulls the latest code from the repository  
- **Build** – Uses Maven to compile and package the application  
- **Unit Test** – Runs unit tests to ensure application correctness  
- **Code Analysis** – (Optional) Integrates tools like SonarQube for static analysis  
- **Deploy** – Deploys the WAR file to a Tomcat server or target environment  

### Prerequisites

- A Jenkins server with Maven and JDK installed  
- Jenkins configured with appropriate credentials and agent nodes (if needed)  
- Optional: SonarQube or Artifactory integration depending on CI/CD goals  

### How to Use

1. Create a new **Pipeline Job** in Jenkins.  
2. Point the job to this repository and the `Jenkinsfile`.  
3. Customize environment variables or paths as needed within Jenkins.  
4. Trigger the pipeline manually or hook it to webhooks for automation.



---

## Development Environment Setup

This project uses **Oracle VirtualBox** with **Vagrant** to manage multiple VMs, each running different components of the stack:

- **db01**: MySQL database server  
- **mc01**: Memcached caching server  
- **rmq01**: RabbitMQ message broker  
- **app01**: Apache Tomcat Java application server  
- **web01**: Nginx frontend web server  

### Vagrant Setup

- Vagrant file (`Vagrantfile`) defines all VMs with private IPs on the `192.168.56.x` subnet.  
- [Hostmanager plugin](https://github.com/devopsgroup-io/vagrant-hostmanager) is used to manage and sync `/etc/hosts` across the VMs for seamless hostname resolution and to make sure everything is connected properly.  
- Each VM runs a shell provisioner script (`mysql.sh`, `memcache.sh`, `rabbitmq.sh`, `tomcat.sh`, `nginx.sh`) to install and configure its respective service.

### Example Vagrantfile snippet

```ruby
Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  #default setting/tweak around if possible

  config.vm.define "db01" do |db01|
    db01.vm.box = "centos/stream9"
    db01.vm.hostname = "db01"
    db01.vm.network "private_network", ip: "192.168.56.15"
    db01.vm.provider "virtualbox" do |vb|
      vb.memory = "600"
    end
    db01.vm.provision "shell", path: "mysql.sh"
  end

  # ... other VMs configured similarly
  
end
```
---
### Steps

Ensure VirtualBox, Vagrant, and the hostmanager plugin are installed.

Run vagrant up to launch and provision all VMs.

Use vagrant ssh <vm-name> to connect to any VM (e.g., vagrant ssh db01).

The /src/hosts directory contains host mappings to ensure all VMs recognize each other by hostname as previously mentioned.

Getting Started
Prerequisites
Java Development Kit (JDK)

Apache Tomcat

Nginx

MySQL Server

RabbitMQ

Memcached

Maven or Gradle (for building the Java app you want)

### Installation & Running Locally
Clone the repository:

- git clone https://github.com/yourusername/vprofile.git
- cd vprofile
- Configure MySQL and create the required database and tables.

- Configure RabbitMQ and Memcached if needed (default settings worked).

- Build and deploy the Java web app to Tomcat.

- Configure Nginx to proxy requests to Tomcat.

- Start all services (MySQL, RabbitMQ, Memcached, Tomcat, Nginx) Little Tedious.

Access the application running ip addr show or straight from the vagrant file.

Project Structure
bash
Copy
Edit
/src          # Java source code for the web application
/config       # Configuration files for Tomcat, Nginx, etc.
/docs         # Documentation and diagrams (if any)
/scripts      # Helpful scripts for setup and deployment


Future Improvements

Add user authentication and authorization

Deploy on a cloud server or containerize with Docker (Coming Soon)
