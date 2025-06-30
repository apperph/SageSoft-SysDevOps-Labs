## Overview
This lab will guide you through creating a VPC and implementing network segmentation. By the end of the exercise, your VPC will include 2 public subnets and 2 private subnets.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session1/Lab0/image0-1.png)

##  1 Setup VPC

1-a. Open the AWS Management Console and go to the **VPC** section.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-2.png)

1-b. Under **Your VPCs**, click **Create VPC**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-3.png)

1-c. Specify **a /16 CIDR** block for the VPC. Ensure it doesn’t overlap with any existing VPCs. Optionally, **include your name** in the VPC name for easy identification.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-4.png)

1-d. Your **VPC** has been successfully created.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-5.png)

## 2. Set Up Private Subnets

2-a. Go to **Subnets** and click **Create Subnet**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-6.png)

2-b. Create a **private subnet**:
- Choose your **VPC**.
- Name the subnet "**Private Subnet A**."
- Assign it **a /24 CIDR block**.
- Pick an **Availability Zone (AZ)**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-7.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-8.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-9.png)

2-c. Your private subnet has been created.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-10.png)

2-d. Create **another private subnet**:

- Name it **Private Subnet B.**
- Assign it a different **/24 CIDR block**.
- Select an AZ different from **Private Subnet A.**
- You now have two private subnets.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-11.png)

## 3. Setup an Internet Gateway
3-a. To enable internet connectivity for public subnets, first **create an Internet Gateway (IGW)**. Go to **Internet Gateway **and click** Create Internet Gateway**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-12.png)

3-b. Name your Internet Gateway and click **Create.**

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-13.png)

3-c. Attach the IGW to your VPC:
- Select the **IGW.**
- Click **Actions**, then **Attach to VPC**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-14.png)

- Choose your **VPC** and **confirm** the attachment.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-15.png)

## 4. Set Up Public Subnets

4-a. Create two public subnets:
- Name them **Public Subnet A** and **Public Subnet B.**
- Assign both **a /24 CIDR block**.
- Ensure each is in a **separate AZ.**

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-16.png)

4-b. Create a **Route Table** for the public subnets:

- Go to **Route Tables** and click **Create Route Table.**

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-17.png)

- Name it and **associate it with your VPC**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-18.png)

4-c. Update the routes for the public route table:

- Select the **route table** and go to the **Routes tab**.
- Click the **Edit Routes** button.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-19.png)

- Add a route with:
- **Destination: 0.0.0.0/0**
- **Target: Your Internet Gateway.**
- **Save** the changes.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-20.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-21.png)

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-22.png)

4-d. Associate the public subnets with the route table:
- Go to the **Subnet Associations** tab in the route table.
- Click **Edit Subnet Associations** and select your **public subnets.**

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-23.png)

- **Save** the associations.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-24.png)

## 5. Set Up a Route Table for Private Subnets

5-a. Create a **route table** for the private subnets:

- Name it and associate it with your VPC.
- You don’t need to edit routes for the private route table.

5-b. Associate the private subnets with this route table by editing the **Subnet Associations**.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Labs2025/Session1/Lab0/image-25.png)

**Lab Completion**
- Congratulations! You’ve completed the lab.

**JSON Output**

Copy and fill in the JSON template below with the resource IDs from your setup, then paste it in the provided text box:

```
{
   "vpc_id": "",
   "private_subnet_a_id": "",
   "private_subnet_b_id": "",
   "public_subnet_a_id": "",
   "public_subnet_b_id": "",
   "internet_gateway_id": "",
   "public_route_table_id": "",
   "private_route_table_id": ""
}
```

**Resource Cleanup**

When the lab is complete, ensure you delete all the resources you’ve created.


