**AWS Application Load Balancer Project – EC2 Web Servers in a Custom VPC**

**Project Overview**

- This project showcases a high-availability web architecture using an Application Load Balancer (ALB) to distribute traffic across two EC2 instances.
- These are hosted in separate public subnets within a custom VPC.
- It simulates a real-world load-balanced setup for scalable and resilient web hosting.

**AWS Services and Resources Used**

- Amazon VPC: Custom Virtual Private Cloud to isolate and control networking

- Subnets: Two public subnets in separate Availability Zones (A and B)

- Internet Gateway: For outbound internet access

- Route Tables: To route traffic from subnets to the Internet Gateway

- Amazon EC2: Two Ubuntu instances running Apache with custom HTML

- SSH Key Pair: Created to securely connect to the EC2 instances

- User Data Script: Used to automatically install Apache and create web pages on launch

- Target Group: Included both subnets for load balancing

- Application Load Balancer (ALB): Managed routing of HTTP requests across instances

- Security Groups: Defined firewall rules to allow HTTP (port 80) and SSH (port 22)

**Step-by-Step Setup**

- Create a custom VPC for network isolation

- Create an Internet Gateway and attach it to the VPC
  
- Create 2 public subnets attached to our VPC => each in a different Availability Zone (A and B) .I chose the IP ranges for each of them
  
- Create Route Table to allow internet access from the public subnets through the Internet Gateway.
      => Link it with the VPC
      => Associate it with the subnets created
      => Associate the route with Internet Gateway in order to be accesible via Internet

- Create 2 EC2 instances (AMI- Ubuntu)
      => I create a RSA Key Pair for security
      => attach them with the VPC created & subnets (EC2 1: in AZ A, EC2 2:in AZ B)
      => assign public IP
      => create Security Groups (type: ssh -source type: Anywhere  & type: HTML -source type: Anywhere)
      => use user data to install Apache and serve a basic HTML page showing the hostname and IP:

      #!/bin/bash
      yes | sudo apt update
      yes | sudo apt install apache2
      echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -       I | cut -d' ' -f1)</p>" | sudo tee /var/www/html/index.html
      sudo systemctl restart apache2

   => I launch the instances , copy the public IPs and check if the Apache worked => OK

- Create a Target Group, Instance type =>  ALB will point to this target group
    => Protocol version : HTTP1
    => register targets: both EC2 instances
 

- Set up an Application Load Balancer (ALB)
   => scheme: Internet facing
   => attached it to the VPC created
   => mapp it with the subnets created
   => create a Security Group for ALB: HTTP , Source- Anywhere in order to allow the access via Internet
   => Listener: HTTP - Forward to => Target group created

-  Accessing the public DNS name of the ALB, demonstrating load distribution between the two instances.


**Why did I use an Application Load Balancer?**

- Evenly distributes incoming HTTP traffic

- Ensures high availability by rerouting if a server fails

- Simplifies scaling by supporting additional targets without DNS changes

- Works at the application layer (Layer 7) for smart routing based on content

 **Project Output**
- Accessing the ALB public DNS results in alternating web pages from each EC2 instance, each showing the server hostname and internal IP

**What I've learned**

- Designing a VPC from scratch with subnets, routing, and security

- Launching and configuring EC2 instances with automated scripts

- Creating and using SSH key pairs for secure access

- Setting up and testing an Application Load Balancer with target group

- Troubleshooting network access, Apache configuration, and load balancing behavior

**Note**: This project is currently paused to avoid AWS charges. 

 **Architecture Diagram**:  https://lucid.app/lucidchart/48b3594f-a71a-48e4-99a8-798d397ff804/view 

![ALB diagram](https://github.com/user-attachments/assets/21282d00-d2fb-4558-8b16-9394f5587813)

**Screenshots illustrating the architecture and configuration steps:**


**- ALB- Overview:**

![ALB- overview](https://github.com/user-attachments/assets/1da6a344-1d98-49e3-82cc-4541f0a693e7)


**- ALB- Details:**
  
![ALB- Details updated](https://github.com/user-attachments/assets/d2f8bfee-5059-43ac-a0ec-f2675472f4ef)


**- ALB- Listeners and rules:**

![ALB- Listeners and rules updated](https://github.com/user-attachments/assets/ae40aa09-90ea-4785-a67d-624dfaed5692)


**- ALB- Resource map:**
 

![ALB- Resource map updated](https://github.com/user-attachments/assets/3bef6a22-4490-4f13-898f-47d05843d7c6)


**- ALB- Security:**

![ALB- Security updated](https://github.com/user-attachments/assets/19c2993b-daaf-42eb-a201-16d27fba54e6)


**- EC2 - Instance 1:**

![EC2 -1 updated](https://github.com/user-attachments/assets/520e005b-b07f-4ca4-8829-b3d63c3f46a8)

**- EC2 - Instance 2:**

![EC2 - 2 updated](https://github.com/user-attachments/assets/18f197fb-625e-4c8d-b652-5254c8ad11dc)

**- Test 1  ALB (result: Instance 1)**
 
![test ALB 1 updated](https://github.com/user-attachments/assets/d130bc96-2306-4888-9f52-711433b9f2be)

**-  Test 2  ALB (result: Instance 2)**
  
![test ALB 2 updated](https://github.com/user-attachments/assets/887d401a-817f-4cef-a2b6-21c6639ae420)

