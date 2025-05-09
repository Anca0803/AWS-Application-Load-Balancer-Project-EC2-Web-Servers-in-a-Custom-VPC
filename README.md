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

**What I Learned**

- Designing a VPC from scratch with subnets, routing, and security

- Launching and configuring EC2 instances with automated scripts

- Creating and using SSH key pairs for secure access

- Setting up and testing an Application Load Balancer with target group

- Troubleshooting network access, Apache configuration, and load balancing behavior

**Note**: This project is currently paused to avoid AWS charges. 

Architecture Diagram:  https://lucid.app/lucidchart/48b3594f-a71a-48e4-99a8-798d397ff804/view 

![ALB diagram](https://github.com/user-attachments/assets/21282d00-d2fb-4558-8b16-9394f5587813)

Please find the Print screens of my work:

![EC2 -1 updated](https://github.com/user-attachments/assets/0cb3b7ca-d6c7-4c3f-b3e4-3d62ce44af53)


![EC2 - 2 updated](https://github.com/user-attachments/assets/415be22f-b927-4edf-bb03-6cb48839cbaa)
![ALB- Network mapping updated](https://github.com/user-attachments/assets/1abd1190-36cf-4308-a3b5-192896f1ea67)
![ALB- Details updated](https://github.com/user-attachments/assets/9fd897cc-ab11-47b2-8cef-a7d345a97714)

![ALB- Listeners and rules updated](https://github.com/user-attachments/assets/647e15f3-f896-4ba9-bbe2-9917dbbcb4d3)
![ALB- Resource map updated](https://github.com/user-attachments/assets/1f484739-d198-46d7-b1ba-7bf26e3c1c3d)
![ALB- Security updated](https://github.com/user-attachments/assets/7dd9ead4-b4b7-4f8d-a6ca-27104496db26)
![test ALB 1 updated](https://github.com/user-attachments/assets/937f831c-fc10-4d53-b9e6-ba8ebbc3403c)
![test ALB 2 updated](https://github.com/user-attachments/assets/a3cc6c7f-2fbf-44ad-b493-c5ddf9c33cc1)
