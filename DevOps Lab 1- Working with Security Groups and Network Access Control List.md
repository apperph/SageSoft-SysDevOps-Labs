## Overview
In this lab, we'll dive deeper into network security by setting up a VPC with a Public and Private Subnet. We'll create two EC2 instances: a **bastion** host in the Public Subnet and an **AppServer** in the Private Subnet. To demonstrate secure communication, we'll SSH into the bastion host and then use it to access the AppServer.

**Prerequisites:**

Before starting this lab, please ensure you have:
- A VPC with at least one Public and one Private Subnet.
- Two EC2 instances:
- *A bastion host in the Public Subnet with a Public IP address enabled.*
- *An AppServer in the Private Subnet.*
- The same key pair for both instances.
- Both instances using the Amazon Linux 2023 AMI and the t2.micro instance type.
- If you're unfamiliar with VPCs, EC2 instances, or SSH, please review the previous labs.

## 1.  Connect to the Bastion Host:

- Navigate to the EC2 dashboard and locate the **security group** associated with your **Bastion** host.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-1.png)

- Ensure that the inbound rule for port 22 allows traffic from any source (0.0.0.0/0). 

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-2.png)

This configuration permits **inbound SSH connections** from any IP address globally. 

*It's important to note that this is not considered a secure practice in real-world scenarios. We're using this approach for illustrative purposes only.*

Obtain the **public IP address** of the bastion host and establish an SSH connection to it.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-3.png)

## 2. SSH into our AppServer EC2 from the bastion EC2 using SSH forwarding using a macOS or Linux Machine

2-a. Open your **CLI** application and navigate to the directory where your **PEM** key is located. Then, type the following command and press Enter:

`ssh-add <key-pair-name>`

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-4.png)

2-b. Since the **PEM key** has already been **added** to the **SSH agent**, you can now connect to the **bastion** host by simply running the following command and pressing Enter:

`ssh -A ec2-user@<public-ip>`


![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-5.png)


2-c. At this point, you can SSH into the AppServer using its Private IP address. To find the Private IP address, go to the **EC2 dashboard** and retrieve it from there.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-6.png)

2-d. Return to your **SSH session** and connect to the **AppServer** by executing the following command:

`ssh ec2-user@<private IP address of app server EC2>`

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-7.png)

You are now connected to the AppServer via the bastion. However, there's a critical issue here that you should never implement in any environment. Go to the EC2 dashboard and review the inbound rules of the security group associated with the AppServer instance.


You’ll notice that the security group is overly permissive, allowing SSH traffic from any IP address within the VPC. The best practice in this case is to ensure that only the bastion host has permission to connect to the AppServer.


## 3. SSH into our AppServer EC2 from the bastion EC2 using SSH forwarding using a Windows machine

- If you've completed **Lab 1**, you should be familiar with using **PuTTY for SSH**. We'll add a few steps to enable **SSH tunneling**. Start by opening the **Pageant** app.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-8.png)

- If Pageant is already running, right-click on the **Pageant icon** in the system tray and select **Add Key**. Then, choose your **PPK file**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-9.png)

- When establishing an SSH connection to the bastion host, make sure to check the **Allow agent forwarding** option. This will enable secure tunneling through the bastion.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-10.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-11.png)

3-b. After successfully connecting to the bastion host, retrieve the **private IP address** of the **AppServer** EC2 instance. 

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-12.png)

3-c. SSH to the AppServer using its private IP address: 

`ssh ec2-user@<AppServer private IP address>`

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-13.png)

You have successfully established an **SSH connection** to the **AppServer using the SSH tunnel**.

## 4. Restrict SSH access to the AppServer

4-a. Access the EC2 dashboard and locate the **security group** associated with your **AppServer** instance.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-14.png)

- There are two ways to restrict SSH access from anywhere:
- 1. Replace the source with the **bastion's private IP address**.
- 2. Replace the source with the **bastion's security group ID**.
- Choose a method and apply the necessary changes to the security group. Then, try SSHing to the AppServer again from the bastion.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-15.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-16.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-17.png)

By creating a security group rule for the App Server that allows SSH traffic only from the specific IP address of the bastion host, you're effectively restricting access to the App Server. This ensures that only the bastion host can connect to the App Server, enhancing security.

*As an experiment, try using a dummy IP address or security group ID to see if you can still SSH into the AppServer from the bastion host.*


4-b. Understanding Security Groups:

- Security groups operate at the **instance level**.
- They function at **Layer 5 **of the **OSI model**, enabling **stateful** inspection of network traffic. This means that once a connection is established, subsequent traffic from the same connection is **allowed without requiring additional rules**.
- Security groups employ an **implicit deny policy**, meaning that any traffic not explicitly permitted by an inbound rule is blocked.


5. Working with Network Access Control Lists (NACLs)

Now that we've gained insights into security groups, we'll explore another crucial component of network security: Network Access Control Lists (NACLs).

**Key Characteristics of Network Access Control Lists (NACLs):**

- **Subnet-Level Security:** NACLs operate at the subnet level, providing granular control over traffic entering and leaving a specific subnet.
- **Explicit Deny Rules:** Unlike security groups, NACLs allow you to explicitly deny traffic by defining specific rules.
- **Rule Evaluation Order:** NACL rules are evaluated in ascending numerical order. The first matching rule determines the action taken.
- **Stateful vs. Stateless:** NACLs are stateless, meaning they don't track the state of a connection. Both inbound and outbound rules must be defined to allow bidirectional traffic.
- **CIDR Notation:** NACL rules only accept IP addresses in CIDR notation, unlike security groups which can also reference other security groups.

5-a. To further enhance security, let's **configure NACLs** to allow traffic from the **bastion** host's subnet to the private subnet.

When you create a VPC, a default NACL is automatically created, encompassing both the public and private subnets. 


![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-18.png)

To establish distinct security policies for the private subnet, you'll need to create a separate NACL specifically for it.


5-b. Create a **new NACL** and associate it with your **VPC**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-19.png)

5-c. Select the newly created NACL and navigate to the **Subnet Associations** tab. Associate the NACL with the specific private subnets where you want to apply the custom rules.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-20.png)

5-d. A newly created NACL has a **default deny-all** policy. To allow SSH traffic from the bastion host to the App Server, we need to create an **inbound allow rule**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-21.png)

- Click **Edit inbound rules** and add a new rule. Set the rule number to **100**, specify the protocol as **TCP**, the port range as **22**, and the source IP address as the **private IP address** of the **bastion** host in CIDR notation (e.g., 10.0.0.10/32). Click on the **Save** button. 
- 
![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-22.png)
5-e. Use SSH to connect to the AppServer from the bastion host.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-23.png)


The lack of connectivity persists because NACLs operate in a stateless manner. To establish a successful SSH connection, we need to create an outbound rule in the NACL. This rule should permit traffic originating from the private subnet (where the AppServer resides) to any destination port. As SSH connections utilize ephemeral ports on the client side, an outbound rule allowing all ports is usually sufficient.

To learn more about ephemeral ports you can refer to this: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports

5-f. Navigate back to the NACL and create a **new outbound rule**. Set the rule number to 100, specify the protocol as TCP, the **port range** as **1024-65535** (ephemeral ports), and the destination IP address as the **private IP address** of the bastion host in CIDR notation.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-24.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-25.png)

5-g. Attempt to establish an **SSH connection** to the **AppServer** from the **bastion** host once more.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-26.png)

By carefully configuring Network Access Control Lists (NACLs), we've successfully implemented a secure SSH connection from the bastion host to the AppServer.

5-h. To demonstrate how NACL rules are evaluated based on their rule numbers, we'll **add a deny rule** with a **lower priority** (smaller rule number) than the existing allow rule. This will highlight the importance of rule order in NACL configuration.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-27.png)

- Attempt to SSH to the AppServer again. Due to the **higher priority of the deny rule**, the connection will be **blocked**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab2/image-28.png)

**Great work!**

Please copy the following JSON to the textbox and fill in the required AWS resource details:


```
{
   "bastion_instance_id": "",
   "appserver_instance_id": "",
   "bastion_security_group_id": "",
   "appserver_security_group_id": "",
   "private_nacl_id": "",
}
```

## Resource Cleanup
Don't forget to terminate all the instances and delete the other resources you created during this lab.
