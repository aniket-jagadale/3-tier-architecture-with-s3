# Three-Tier Architecture on AWS

This project implements a scalable and secure three-tier web application architecture on Amazon Web Services (AWS), consisting of web, application, and database tiers.

## Architecture Overview

![Architecture Diagram](/img/Untitled%20Diagram.drawio.svg)

The three-tier architecture separates concerns into distinct layers:

- **Web Tier**: Handles HTTP requests, serves static content, and routes dynamic requests to the application tier
- **Application Tier**: Processes business logic and communicates with the database tier
- **Database Tier**: Manages data storage and retrieval using MySQL

## Infrastructure Components

### Networking Setup

The foundation of our architecture is built on a custom VPC with properly segmented subnets:

![VPC Dashboard](/img/Screenshot%20(200).png)

![Resource Map](/img/Screenshot%20(201).png)

![Subnets](/img/Screenshot%20(202).png)

![Route Tables](/img/Screenshot%20(203).png)

![NAT Gateway](/img/Screenshot%20(204).png)

### Web Tier Components

The web tier consists of:

- Auto-scaling group of EC2 instances running web servers
- Internet-facing Application Load Balancer
- Custom AMI for consistent deployment

![EC2 Instances](/img/Screenshot%20(212).png)

![Launch Templates](/img/Screenshot%20(213).png)

![Web Tier AMI](/img/Screenshot%20(211).png)

![Web Load Balancer](/img/Screenshot%20(210).png)

### Application Tier Components

The application tier includes:

- Internal Application Load Balancer
- Auto-scaling group of application servers
- Custom AMI with application code

![App Tier Load Balancer](/img/Screenshot%20(208).png)

![App Target Group](/img/Screenshot%20(209).png)

### Database Tier

The database tier uses Amazon RDS with MySQL:

![RDS Database](/img/Screenshot%20(207).png)

### Content Delivery and Storage

For performance and scalability, we implemented:

- CloudFront distribution for content delivery
- S3 buckets for image storage

![CloudFront Distribution](/img/Screenshot%20(206).png)

![Image Upload](/img/Screenshot%202025-09-17%20213228.png)

![Upload Success](/img/Screenshot%20(214).png)

![Upload Success](/img/Screenshot%20(215).png)

## Implementation Steps

### 1. Network Infrastructure
- Created a custom VPC (`mycustomvpc`) with CIDR 10.0.0.0/16
- Configured public and private subnets across two availability zones
- Set up route tables with appropriate routes
- Created a NAT Gateway for outbound internet access from private subnets

### 2. Database Tier
- Launched an RDS MySQL instance (`database-1`) in private subnets
- Configured security groups to allow access only from the application tier
- Set up the database connection string for application use

### 3. Application Tier
- Created a custom AMI (`appserver2ami`) with application code
- Configured a launch template (`appserverLT`) for consistent deployments
- Set up an internal Application Load Balancer (`appserverASG-1`)
- Configured auto-scaling for the application instances

### 4. Web Tier
- Created a custom AMI (`webserverAMI`) with web server configuration
- Configured a launch template (`webserverLT`) for web instances
- Set up an internet-facing Application Load Balancer (`webserverASG-1`)
- Configured auto-scaling for the web instances

### 5. Content Delivery
- Created S3 buckets for image storage
- Implemented image upload functionality
- Configured CloudFront distribution (`threetierCDN`) for content acceleration

## Security Considerations

- Database instances are in private subnets with no public access
- Application tier is behind an internal load balancer
- Security groups restrict traffic between tiers
- NAT Gateway provides controlled outbound internet access

## Performance Features

- Auto-scaling groups ensure capacity meets demand
- Load balancers distribute traffic evenly
- CloudFront provides global content delivery
- Multi-AZ deployment ensures high availability

## Usage

The application supports image uploads which are:
1. Stored in S3 buckets for durability
2. Metadata is saved in the MySQL database
3. Distributed via CloudFront for fast global access

## Monitoring

All components are configured with appropriate monitoring and logging to ensure operational visibility and quick troubleshooting.

## Conclusion

This three-tier architecture on AWS provides a scalable, secure, and highly available foundation for web applications, following AWS best practices for cloud infrastructure.