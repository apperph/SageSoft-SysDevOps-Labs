## Overview
This lab will demonstrate how to encrypt objects in S3 using KMS, we will also check what will happen with the objects once the CMK is disabled.


## Prerequisite:
This will require a private S3 bucket, if you don’t have one yet, please go ahead and create one.


## 1. Create a Customer Master Key

1-a. Navigate to the KMS console and click on create key.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-1.png)

1-b. Select symmetric and click encryption and decryption.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-2.png)

1-c. Set the alias and click next.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-3.png)

1-d. Set your IAM user as the key account administrator and click next.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-4.png)


1-e. Select your IAM user again to allow it to use the CMK.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-5.png)

1-f. Review the policy and click finish.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-6.png)



## 2. Upload and encrypt an object at rest using the newly created CMK.

2-a. Navigate to your S3 bucket and upload an object.

Then click on Properties to drop down more options.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-7.png)


2-b. Scroll down to server-side encryption settings and enable it, select SSE-KMS for the encryption type and for the AWS KMS Key, select choose from your KMS master keys, and from the drop-down, select the CMK you created earlier.

Then click upload.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-8.png)

2-c. Try to download the object, you will be able to download it because you have permission to use the CMK.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-9.png)



## 3. Disable the CMK

3-a. Navigate back to KMS, select your key, click on key actions then disable.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-10.png)


Confirm that you will disable this key, the click disable key.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-11.png)


3-b. Now we have disabled the key, navigate back to your S3 bucket and download the object.

It will show a KMS disabled exception.

![](https://sb-next-prod-image-bucket.s3.ap-southeast-1.amazonaws.com/public/CAMP/Session3/Lab12/image12-12.png)


Well done!

Please copy and paste the JSON in the textbox on the left side and supply the necessary information.


```
{
 "s3_bucket_arn":"",
 "kms_arn":"",
}
```

**Please don’t forget to delete the resources you created for this lab, in the case of KMS CMK, you need to schedule it for deletion.**
