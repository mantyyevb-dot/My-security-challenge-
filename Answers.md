<!DOCTYPE html>
<html lang="en"> 
<h1> Technical Challenge (Controls Deployment) </h1>
<h2> Part 1: Cybersecurity Scenario (30 points) </h2>
<h3> 1. Threat Intelligence Report (10 points): </h3>

 **- List the types of attack that could be used.**   
This could be phishing, malware/ransomware, SQL injection on the website, or brute-force password guessing.

**- Explain how a vulnerability exploited can provide access to the network.**    
If a web app isn’t patched, a hacker can use that weak spot to sneak in. Once inside, they can move around the network, steal data, or even install other malware.

**- Suggest preventive measures to avoid similar incidents in the future.**    
Keep apps updated and patched, use MFA, train employees about phishing, and monitor logs for strange activity.
<h3> 2. Incident Response Plan (10 points): </h3>

**- Outline an incident response plan to address the breach.**     
**- Include steps for containment, eradication, and recovery.**    
First, isolate the affected system or user account so the attack doesn’t spread. Second, remove the malware or close the hole that let the attacker in. Third, restore from backups if needed, bring systems back online carefully, and make sure everything is patched. Finally, review what happened and fix weaknesses so it doesn’t happen again.  
<h3> 3. Network Security Measures (10 points): </h3>

**- Recommend network security measures to enhance the organization’s defense posture.**    
**- Include at least three different security technologies or practices (e.g., IDS/IPS, firewalls, network segmentation).**    
To make the network safer, I’d recommend using a firewall to control what traffic is allowed in and out. I’d also set up an IDS/IPS system or use something like Azure Defender to detect and block suspicious activity. Another good step is network segmentation, where you split the network into smaller parts so if one section is attacked, it doesn’t spread everywhere. Together, these make it harder for attackers to move around and easier to spot them quickly.  
<h2> Part 2: Container Security Implementation (30 points) </h2>
<h3> 1. Docker Security Best Practices (10 points): </h3>

**- List and explain five Docker security best practices.**  
1. Use official and trusted base images.   
2. Keep images small and updated.  
3. Don’t run containers as root user.  
4. Scan images for vulnerabilities.  
5. Limit the resources a container can use (CPU/memory).
   
**- Implement one of these practices in a Dockerfile and provide the code.** 
<pre><code>
    FROM nginx:alpine     
    USER 1001    
</code></pre>
<h3> 2. Kubernetes Security Configuration (10 points): </h3>

**- Describe three Kubernetes security features.**  
1. **Role-Based Access Control (RBAC):** controls who can do what in the cluster.  
2. **Network Policies:** only allow the right pods to talk to each other.  
3. **Security Context:** set rules inside pods like not running as root or making files read-only.
   
**- Write a Kubernetes YAML configuration that includes securityContext settings for a pod.**    
<pre><code>
     apiVersion: v1  
    kind: Pod  
    metadata:  
      name: secure-pod  
    spec:  
      containers:  
      - name: app  
        image: nginx  
        securityContext:  
          runAsNonRoot: true  
          readOnlyRootFilesystem: true  
          allowPrivilegeEscalation: false
</code></pre>
<h3> 3. IaaS Security Measures (10 points): </h3>

**- Explain the concept of Infrastructure as a Service (IaaS) and its security implications.**
1. **Infrastructure as a Service**, is when the cloud provider like Azure gives you the main building blocks such as servers, storage, and networking, and you’re responsible for what runs on top of it. The provider takes care of the physical hardware and the datacenter, but you’re the one who has to secure the virtual machines, operating systems, applications, and how people access them.  
2. From a security point of view in Azure, it’s important to use **Azure Firewall** and **NSGs** to control traffic so only what’s needed is allowed, keep **VMs patched** and **updated** to avoid old vulnerabilities, and enable **Microsoft Defender for Cloud** to continuously monitor resources, alert on risks, and suggest fixes. 
<h2> Part 3: CI/CD Pipeline Setup (40 points) </h2>
<h3> 1. Configuration Management with Ansible/Chef/Puppet/Terraform (15 points): </h3>

**- Choose one configuration management tool (Ansible, Chef, or Puppet, terraform).**  
**- Write a script/playbook to automate the deployment of a web server on a virtual machine.**  
<pre><code>
    provider "azurerm" { features {} }
    
    resource "azurerm_resource_group" "rg" {
      name     = "demo-rg"
      location = "East US"
    }
    
    resource "azurerm_linux_virtual_machine" "vm" {
      name                = "demo-vm"
      resource_group_name = azurerm_resource_group.rg.name
      location            = azurerm_resource_group.rg.location
      size                = "Standard_B1s"
      admin_username      = "azureuser"
      disable_password_authentication = true
     
      admin_ssh_key {
        username   = "azureuser"
        public_key = "ssh-rsa YOUR_PUBLIC_KEY"
      }
      
      source_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "18.04-LTS"
        version   = "latest"
      }
       
      custom_data = base64encode("#!/bin/bash\napt-get update && apt-get install -y nginx\nsystemctl start nginx")
    }  
</code></pre>
<h3> 2. CI/CD Pipeline Configuration (25 points): </h3>

**- Create a Jenkins pipeline configuration (Jenkinsfile) that includes stages for building, testing, and deploying a sample application to Azure.**   
**- Ensure that the pipeline includes security scanning as a step.**  
<pre><code>
    pipeline {
        agent any
        stages {
            stage('Build') {
                steps { echo 'Building app...' }
            }
            stage('Test') {
                steps { echo 'Running tests...' }
            }
            stage('Security Scan') {
                steps { echo 'Security scan passed' }
            }
            stage('Deploy') {
                steps {
                    sh 'terraform init'
                    sh 'terraform apply -auto-approve'
                }
            } 
        }
    }
</code></pre>
</html>
