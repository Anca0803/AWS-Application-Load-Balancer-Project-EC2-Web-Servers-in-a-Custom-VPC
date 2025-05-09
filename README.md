**AWS Application Load Balancer Project – EC2 Web Servers in a Custom VPC**

**Project Overview**

This project showcases a high-availability web architecture using an Application Load Balancer (ALB) to distribute traffic across two EC2 instances hosted in separate public subnets within a custom VPC. It simulates a real-world load-balanced setup for scalable and resilient web hosting.

**AWS Services and Resources Used**

- Amazon VPC: Custom Virtual Private Cloud to isolate and control networking

- Subnets: Two public subnets in separate Availability Zones

- Internet Gateway: For outbound internet access

- Route Tables: To route traffic from subnets to the Internet Gateway

- Amazon EC2: Two Ubuntu instances running Apache with custom HTML

- SSH Key Pair: Created to securely connect to the EC2 instances

- User Data Script: Used to automatically install Apache and create web pages on launch

- Target Group: Included both EC2s for load balancing

- Application Load Balancer (ALB): Managed routing of HTTP requests across instances

- Security Groups: Defined firewall rules to allow HTTP (port 80) and SSH (port 22)

**Step-by-Step Setup**

- Created a custom VPC for logical network isolation

- Create an Internet Gateway and attach to VPC
  
- Create Route Table and associate with created subnets

- Created two public subnets, each in a different Availability Zone

- Generated an SSH key pair to securely connect to the EC2 instances

- Launched two Ubuntu EC2 instances, one per subnet

- Used user data to install Apache and serve a basic HTML page showing the hostname and IP

- Created a target group and registered the EC2s

- Set up an Application Load Balancer, attached it to the VPC, and connected it to the target group

-  Accessing the public URL of the ALB, demonstrating load distribution between the two instances.


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

- Setting up and testing an Application Load Balancer with target groups

- Troubleshooting network access, Apache configuration, and load balancing behavior

**Note**: This project is currently paused to avoid AWS charges. Screenshots and architecture diagrams available upon request.
