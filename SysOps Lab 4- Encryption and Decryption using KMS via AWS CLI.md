## Overview:
This activity will demonstrate how to encrypt and decrypt data using AWS KMS via AWS CLI. 

### Prerequisites:

An Ubuntu or Amazon Linux 2 environment with AWS CLI installed and configured with appropriate credentials.

Permissions to create and manage KMS keys in your AWS account.

Ensure you have the openssl and base64 utilities installed.

## 1. Create KMS Customer Managed Key (CMK)

1-a. Create a CMK

Run the following command to create a Customer Managed Key (CMK) in AWS KMS: 

`aws kms create-key --description "apper-demo" --region ap-southeast-1`

<img width="696" alt="Screenshot 2025-06-30 at 3 26 55 AM" src="https://github.com/user-attachments/assets/59477423-0f86-434e-868e-0f25eb77ee74" />

1-b. Create an Alias for the CMK

To make the CMK easier to reference, create an alias:

`aws kms create-alias --target-key-id <key-id-here> --alias-name "alias/apper-demo" --region ap-southeast-1`

1-c. Verify the Alias

Navigate to the AWS KMS console to confirm the alias (alias/apper-demo) is associated with the CMK

<img width="1132" alt="Screenshot 2025-06-30 at 3 33 34 AM" src="https://github.com/user-attachments/assets/606cd015-27be-4cbe-8322-2a8d07619028" />

## 2. Generate a Data Key

2-a. Generate a Data Key

Run the following command to generate a data key using the CMK:

`aws kms generate-data-key --key-id alias/<your alias here> --key-spec AES_256 --region ap-southeast-1`

The response includes:

- Plaintext: The unencrypted data key (base64-encoded).

- CiphertextBlob: The encrypted data key (base64-encoded).

- KeyId: The ID of the CMK.

*Important: The Plaintext value is only shown once. Do not clear your terminal until you save it.*

<img width="692" alt="Screenshot 2025-06-30 at 3 36 28 AM" src="https://github.com/user-attachments/assets/12544905-9cea-4d85-bac9-88b0f865a6ba" />

2-b. Store the CiphertextBlob

Save the CiphertextBlob to a file:

`echo "<ciphertext-blob>" > datakey_ciphertext_base64.txt`

Replace <ciphertext-blob> with the CiphertextBlob value from the previous step.

2-c. Store the Plaintext

Save the Plaintext to a file:

`echo "<plaintext>" > datakey_plaintext_base64.txt`

Replace <plaintext> with the Plaintext value from step 2-a.

## 3. Decode the Plaintext

3-a. Decode the Plaintext

`cat datakey_plaintext_base64.txt | base64 --decode > datakey_plaintext_decoded.txt`

This creates a file named datakey_plaintext_decoded.txt containing the decoded plaintext key.

## 4. Encrypt a Text File

4-a. Encrypt a Message

Use the decoded plaintext key to encrypt a message:

`echo "This is a confidential message and should be encrypted" | openssl enc -e -aes-256-cbc -pbkdf2 -iter 100000 -k "$(cat ~/datakey_plaintext_decoded.txt)" > secret_message.txt`

5. Delete Plaintext Files

5-a. Remove Plaintext Files

For security, delete the plaintext files:

`rm -rf datakey_plaintext_*`

## 6. Retrieve the Plaintext

6-a. Decode the Ciphertext

Decode the base64-encoded ciphertext:

`cat datakey_ciphertext_base64.txt | base64 --decode > datakey_ciphertext_decoded.txt`

This creates datakey_ciphertext_decoded.txt containing the decoded ciphertext.

6-b. Decrypt the Data Key

Retrieve the plaintext key by decrypting the ciphertext:

`aws kms decrypt --ciphertext-blob fileb://$(pwd)/datakey_ciphertext_decoded.txt --region ap-southeast-1`

<img width="692" alt="Screenshot 2025-06-30 at 3 55 36 AM" src="https://github.com/user-attachments/assets/ba8eda01-721a-4182-b475-ef6938d644bb" />

6-c. Store and Decode the Retrieved Plaintext

Repeat steps 2-c and 3-a to store and decode the plaintext:

1. Save the Plaintext from step 6-b to datakey_plaintext_base64.txt:

`echo "<plaintext-from-decrypt>" > datakey_plaintext_base64.txt`

2. Decode it:

`cat datakey_plaintext_base64.txt | base64 --decode > datakey_plaintext_decoded.txt`

## 7. Decrypt the Secret Message

7-a. Decrypt the Message

Decrypt the encrypted message using the decoded plaintext key:

`openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000 -k "$(cat ~/datakey_plaintext_decoded.txt)" -in secret_message.txt`

<img width="692" alt="Screenshot 2025-06-30 at 4 01 08 AM" src="https://github.com/user-attachments/assets/901ccceb-b3ae-4420-9b93-91223908e9c8" />

This outputs the original message: "This is a confidential message and should be encrypted".

## 8. Clean Up

8-a. Schedule CMK Deletion

To delete the CMK, schedule it for deletion (7-day waiting period):

`aws kms schedule-key-deletion --key-id <key-id-here> --pending-window-in-days 7 --region ap-southeast-1`

<img width="692" alt="Screenshot 2025-06-30 at 4 02 30 AM" src="https://github.com/user-attachments/assets/7e2db9d1-3cb3-417f-b92a-622a039a6775" />

8-b. Delete the Alias

Before deleting the key, remove the alias:

`aws kms delete-alias --alias-name alias/apper-demo --region ap-southeast-1`

And we have successfully decrypted the secret message! Well done mate!
