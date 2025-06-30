## Overview:

This lab will demonstrate how to use the CloudWatch alarm, we will stress the CPU utilization of an EC2 instance and the CloudWatch alarm should alert us that the CPU utilization of the EC2 is too high.

Pre-requisites:

- Create a t2.micro EC2 instance running on Amazon Linux 2023 AMI
- Place it in a public subnet
- Enable auto-assign public IP
- Allow SSH traffic in the security group
- Make sure you have a copy of the SSH key-pair on your local machine.

Once you have created the EC2, follow the instructions below.


## 1. Create an SNS topic.

1-a. Navigate to the SNS console and click on topics.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-1.png)

1-b. Click create topic.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-2.png)

1-c. Select standard, and name your topic.
![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-3.png)
Click create.


1-d. Once the topic is created, click on create subscription.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-4.png)

1-e. Select the email protocol and provide your own email email address. Make sure it’s a working email address as the alerts will be sent to the email address that you will provide

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-5.png)

Click create subscription.


1-f. Check your email, and you will receive an email asking to confirm your subscription to SNS, click confirm subscription.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-6.png)

Once you click that it will direct you to a page that confirms your subscription

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-7.png)


## 2. Create a CloudWatch Alarm

2-a. Navigate to the EC2 console, select your instance and click on:      actions > monitor and troubleshoot > manage cloudwatch alarms

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-8.png)

2-b. Make sure that create an alarm is selected, and for the alarm notification select the SNS notification you created earlier.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-9.png)



2-c. Select CPU utilization, and set >= 50 percent for a period of 5 minutes.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-10.png)

Click create.



2-d. To confirm if your CloudWatch alarm was created, navigate to the CloudWatch console and click on alarms.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-11.png)


## 3. Crank up the EC2 CPU utilization

To test the CloudWatch alarm, we need to force the EC2 instance to have a CPU utilization of at least 50 percent. To do that follow the steps below.


3-a. SSH into the EC2 instance or EC2 Connect to it. 

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-12.png)



3-b. Enable the EPEL repository by running: 

```bash
sudo amazon-linux-extras install epel -y
```
![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-13.png)


3-c. Then install stress by running this command: 

```bash
sudo yum install stress -y
```
![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-14.png)


3-d. Let’s crank up the CPU utilization by running this command: 

```bash
stress --cpu 20 --io 4 --vm 3 --vm-bytes 256M --timeout 300s
```
![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-15.png)

Wait for 5 minutes, once stress is done running it will notify on the terminal session.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-16.png)

3-e. Navigate back to EC2 and check the CPU utilization.

There might be a slight delay so try to wait for a while, then it will show that the CPU utilization hit the 50 percent mark.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-17.png)

3-f. Check your email, you should have received an email from SNS regarding the CPU utilization of the EC2 instance.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session4/Lab14/image14-18.png)

Well done!


Please copy the JSON to the textbox below and supply the necessary information.

```
{
 "ec2_instance_id": "",
 "cloudwatch_alarm_arn": ""
}
```

**Reminder: Kindly terminate the ec2 instance and delete the SNS Topic**
