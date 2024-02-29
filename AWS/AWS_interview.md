# Question 1

Can we change the ssh port in ec2 from 22 to some other port?

Yes, it can be changed. 

Yes, you can change the SSH port from the default port 22 to another port on an EC2 instance. Here's how you can do it:

1. **Connect to Your EC2 Instance:** Use SSH to connect to your EC2 instance using port 22.

2. **Edit SSH Configuration File:** Once connected, edit the SSH configuration file located at `/etc/ssh/sshd_config` using a
   text editor like `vi` or `nano`.

4. **Change SSH Port:** Look for the line in the configuration file that specifies the port number (`Port 22`) and change it to your
   desired port number. For example, you can change it to `Port 2222`.

6. **Save Changes:** Save the changes you made to the configuration file.

7. **Restart SSH Service:** Restart the SSH service to apply the changes. You can do this by running the following command:
   ```
   sudo service ssh restart
   ```

8. **Update Security Group:** If you have a security group associated with your EC2 instance, make sure to update the inbound
   rules to allow traffic on the new SSH port. Add a new rule allowing inbound traffic on the custom SSH port (e.g., TCP port 2222).

10. **Test Connection:** Once the SSH service is restarted and the security group rules are updated, you should be able to connect
     to your EC2 instance using the new SSH port. For example:
   ```
   ssh -p 2222 username@ec2-instance-ip
   ```

Remember to replace `username` with your actual username and `ec2-instance-ip` with the public IP address or DNS name of your EC2 instance.

Changing the SSH port adds an extra layer of security by making it harder for unauthorized users to discover 
and attempt to connect to your instance. However, ensure that you remember the custom port number as you will need to specify 
it whenever you connect via SSH.

# Question 2

How can we VPC peering? Can we do peering between multiple AZ or region?

Virtual Private Cloud (VPC) peering in AWS allows you to connect two VPCs so that they can communicate with each
other securely using private IP addresses. Here's how you can set up VPC peering:

Create VPCs: First, create the VPCs that you want to peer. Each VPC should have a unique CIDR block.

Enable DNS resolution and DNS hostnames: Ensure that DNS resolution and DNS hostnames are enabled for both VPCs.

Create a VPC peering connection:

In the AWS Management Console, navigate to the VPC dashboard.
Choose "Peering Connections" from the left navigation pane and then click "Create Peering Connection."
Specify the details such as the requester VPC and accepter VPC, and optionally provide a VPC peering connection name.
Once the peering connection is created, an acceptance request will be sent to the owner of the accepter VPC.
Accept the peering connection:

In the VPC dashboard, under "Peering Connections," select the pending peering connection and choose "Actions" > "Accept Request."
Update route tables:

Update the route tables for each VPC to route traffic destined for the CIDR block of the peer VPC through the peering connection.
Test the connection: Verify that instances in each VPC can communicate with instances in the other VPC using private IP addresses.

Yes, VPC peering can be established between VPCs that are located in different availability zones (AZs) within the same AWS region. 
VPC peering operates at the network level and is not constrained by AZ boundaries within a region.

When you set up VPC peering between VPCs in different AZs, the peering connection allows communication between instances in those 
VPCs using private IP addresses regardless of their AZ placement.
This can be useful for creating highly available and fault-tolerant architectures within a single region.

# Question 3

What are the different storage classes in AWS?

In AWS, storage classes are a way to classify different types of storage based on their performance, availability, and cost characteristics. 
The choice of storage class allows users to optimize their storage solutions based on their specific requirements. 
Some commonly used storage classes in AWS include:

1. **Amazon S3 Storage Classes**:
   - Standard: Provides high durability, availability, and performance for frequently accessed data.
   - Standard-IA (Infrequent Access): Suitable for less frequently accessed data but requires rapid access when needed.
   - One Zone-IA: Similar to Standard-IA but stores data in a single availability zone, offering cost savings.
   - Intelligent-Tiering: Automatically moves data between Standard and Standard-IA classes based on access patterns, optimizing costs.
   - Glacier: Suitable for data archiving and long-term storage with retrieval times ranging from minutes to hours.
   - Glacier Deep Archive: Lowest-cost storage class for long-term retention of data that is rarely accessed.

2. **Amazon EBS Volume Types**:
   - General Purpose SSD (gp2): Provides baseline performance for a wide range of workloads.
   - Provisioned IOPS SSD (io1): Offers high-performance SSD storage with customizable IOPS (input/output operations per second).
   - Throughput Optimized HDD (st1): Ideal for frequently accessed, throughput-intensive workloads.
   - Cold HDD (sc1): Suitable for infrequently accessed, low-cost storage.

3. **Amazon EFS Performance Modes**:
   - General Purpose: Suitable for a wide range of workloads with moderate throughput.
   - Max I/O: Optimized for applications requiring the highest level of I/O throughput.

These storage classes offer flexibility in terms of performance, cost, and durability, allowing users to tailor their storage solutions 
to meet specific application requirements.

# Question 4

What is AWS direct connect?

AWS Direct Connect is a network service that enables you to establish a dedicated network connection between your on-premises data center 
and AWS. This dedicated connection can help to reduce network costs, increase bandwidth throughput, and provide a more consistent network 
experience compared to internet-based connections.

Here's an overview of how AWS Direct Connect works:

1. **Connection Establishment**: You establish a dedicated network connection called a "port" between your on-premises router (or network
   device) and an AWS Direct Connect location. AWS Direct Connect locations are colocation facilities provided by AWS partners where you can
   connect to AWS services.

3. **Private Virtual Interface (VIF)**: After establishing the physical connection, you create one or more virtual interfaces (VIFs) within
   the AWS Management Console. These VIFs are logical connections that define the routing information for traffic flowing between your
   on-premises network and specific resources or services within AWS.

6. **Routing Configuration**: You configure the routing tables on your on-premises router to direct traffic destined for AWS services
   through the Direct Connect connection. Similarly, you configure route tables within your Amazon Virtual Private Cloud (VPC) to route
    traffic destined for your on-premises network through the Direct Connect connection.

8. **Traffic Flow**: Once the connection is established and routing is configured, traffic between your on-premises network and AWS services
    flows through the dedicated Direct Connect connection. This traffic is not routed over the public internet, providing a more secure and
   consistent network experience.

AWS Direct Connect offers several benefits, including:

- **Reduced Network Costs**: By bypassing the public internet, AWS Direct Connect can help reduce data transfer costs and eliminate bandwidth
  charges associated with internet-based connections.
- **Improved Performance**: Direct connections can provide higher bandwidth and lower latency compared to connections over the public internet,
  which can be beneficial for latency-sensitive applications.
- **Enhanced Security**: Direct connections offer a more secure option for transferring sensitive data between your on-premises environment
  and AWS services, as traffic does not traverse the public internet.

Overall, AWS Direct Connect is a useful solution for organizations that require reliable, high-performance connectivity between their
on-premises infrastructure and AWS resources.

# Question 5

How can we make sure that my application hosted on aws is accessed only internally not on internet?

To ensure that your application hosted on AWS is accessed only internally and not from the internet, you can implement the following measures:

1. **Private Subnets**: Deploy your application in private subnets within your Amazon Virtual Private Cloud (VPC). Private subnets do not have
    a route to the internet gateway, so traffic initiated from resources within these subnets cannot reach the internet directly.

3. **Network Access Control Lists (NACLs)**: Configure network ACLs at the subnet level to control inbound and outbound traffic. By default,
   NACLs allow all traffic, but you can customize them to restrict inbound and outbound traffic based on your requirements.
   You can deny all inbound and outbound traffic except for specific IP addresses or ranges that are allowed to access your application.

5. **Security Groups**: Associate security groups with your EC2 instances or other resources to control inbound and outbound traffic at the
    instance level. Configure security groups to allow inbound traffic only from specific internal IP addresses or ranges and deny all traffic
   from the internet.

7. **Application-Level Authentication**: Implement application-level authentication mechanisms such as username/password authentication,
   API keys, or OAuth tokens to ensure that only authenticated users or services can access your application.

9. **Use Internal Load Balancers**: If your application is load balanced, use internal Elastic Load Balancers (ELBs) instead of internet-facing
     ELBs. Internal ELBs are accessible only from within the VPC and cannot be accessed from the internet.

11. **Use Private DNS Names**: Ensure that your application uses private DNS names for internal communication instead of public DNS names.
    This prevents DNS resolution of your application's domain name from outside the VPC.

By implementing these measures, you can restrict access to your application to only internal users or services within your AWS environment 
and prevent access from the public internet.

# Question 6

How to make you AWS cost optimized?

Optimizing AWS costs involves several strategies aimed at reducing unnecessary spending while ensuring that resources are efficiently utilized.
Here are some key practices to make your AWS infrastructure cost-optimized:

1. **Right Sizing**: Regularly analyze your resource usage and adjust instance sizes or types to match your application requirements.
   Use AWS tools like AWS Trusted Advisor or third-party solutions to identify underutilized resources.

2. **Reserved Instances (RIs)**: Purchase RIs for predictable workloads to benefit from significant cost savings compared to On-Demand pricing.
    Choose the right RI type (Standard, Convertible, or Scheduled) based on your usage pattern.

3. **Spot Instances**: Utilize Spot Instances for non-critical workloads or batch processing tasks to take advantage of significantly lower
   prices. Spot Instances are ideal for fault-tolerant applications that can handle interruptions.

4. **Auto Scaling**: Implement Auto Scaling groups to dynamically adjust the number of instances based on workload demands. This ensures that
    you have the right capacity at the right time, minimizing over-provisioning.

5. **Use Serverless Services**: Leverage serverless computing services like AWS Lambda, Amazon S3, and Amazon DynamoDB to eliminate the need
    for provisioning and managing servers. With serverless, you pay only for the resources consumed during execution.

6. **Monitor and Analyze Costs**: Continuously monitor your AWS usage and spending using tools like AWS Cost Explorer, AWS Budgets, and
    third-party cost management solutions. Analyze cost trends, identify cost drivers, and implement optimization measures accordingly.

7. **Tagging and Cost Allocation**: Implement tagging best practices to categorize resources by environment, application, or team.
    Use cost allocation tags to track spending accurately and allocate costs to the appropriate departments or projects.

8. **Data Transfer Costs**: Minimize data transfer costs by optimizing data transfer between AWS services and regions.
    Use AWS Direct Connect or Amazon CloudFront for efficient data delivery and caching.

9. **Storage Optimization**: Choose the appropriate storage class based on your data access patterns and durability requirements.
    Utilize features like Amazon S3 lifecycle policies to transition data to cheaper storage tiers or delete outdated data.

10. **Review and Optimize Architectural Design**: Regularly review your architecture and optimize resource usage based on best practices.
     Consider using AWS Well-Architected Framework principles to design cost-effective and efficient solutions.

By adopting these cost optimization strategies and continuously monitoring your AWS usage, you can maximize cost savings while ensuring 
optimal performance and scalability for your applications.

# Question 7

How can we achieve high availability in AWS DB?

To achieve high availability in an AWS database, you can follow these best practices:

1. **Multi-AZ Deployment**: Use a multi-AZ (Availability Zone) deployment for your database. AWS offers managed database services like Amazon RDS (Relational Database Service) and Amazon Aurora that support automatic failover to standby replicas in different AZs. This setup ensures that your database remains available even if one AZ goes down.

2. **Read Replicas**: Implement read replicas for read-heavy workloads. Read replicas help distribute read traffic across multiple instances, improving read performance and providing redundancy. AWS RDS and Aurora support read replicas that can be deployed across different AZs for high availability.

3. **Automatic Backups**: Enable automated backups for your database. AWS RDS and Aurora offer automated backups with configurable retention periods. This ensures that you can restore your database to a specific point in time in case of data corruption or accidental deletion.

4. **Monitoring and Alarms**: Set up monitoring and alarms to track the health and performance of your database instances. AWS CloudWatch provides metrics and alarms that can notify you of any issues or performance degradation, allowing you to take proactive measures to maintain availability.

5. **Scaling**: Configure auto-scaling for your database instances to handle fluctuations in workload demand. AWS RDS and Aurora allow you to automatically scale compute and storage resources based on predefined metrics such as CPU utilization or storage capacity.

6. **Cross-Region Replication**: Implement cross-region replication for disaster recovery and additional redundancy. By replicating your database across different AWS regions, you can ensure data availability even in the event of a regional outage or disaster.

By implementing these strategies, you can achieve high availability for your AWS database, ensuring continuous access to your data and minimizing downtime.