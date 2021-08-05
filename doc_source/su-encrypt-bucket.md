# Step 5: \(Optional\) Encrypt training files<a name="su-encrypt-bucket"></a>

You can choose one of the following options to encrypt the Amazon Rekognition Custom Labels manifest files and image files that are in a console bucket or an external Amazon S3 bucket\.
+ Use an Amazon S3 key \(SSE\-S3\)\.
+ Use your AWS Key Management Service customer master key \(Customer CMK\)\. 
**Note**  
The calling [IAM principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html#intro-structure-principal%23intro-structure-principal) need permissions to decrypt the files\. For more information, see [Decrypting files encrypted with AWS Key Management Service](#su-kms-encryption)\.  
 

For information about encrypting an Amazon S3 bucket, see [Setting default server\-side encryption behavior for Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-encryption.html)\.

## Decrypting files encrypted with AWS Key Management Service<a name="su-kms-encryption"></a>

If you use AWS Key Management Service \(KMS\) to encrypt your Amazon Rekognition Custom Labels manifest files and image files, add the IAM principal that calls Amazon Rekognition Custom Labels to the CMK's key policy\. Doing this lets Amazon Rekognition Custom Labels decrypt your manifest and image files before training\. For more information, see [My Amazon S3 bucket has default encryption using a custom AWS KMS key\. How can I allow users to download from and upload to the bucket?](https://aws.amazon.com/premiumsupport/knowledge-center/s3-bucket-access-default-encryption/)

The IAM principal needs the following permissions on the CMK\.
+ kms:GenerateDataKey
+ kms:Decrypt

For more information, see [Protecting Data Using Server\-Side Encryption with CMKs Stored in AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)\.

## Encrypting copied training and test images<a name="w41aac12c21c11"></a>

To train your model, Amazon Rekognition Custom Labels makes a copy of your source training and test images\. By default the copied images are encrypted at rest with a key that AWS owns and manages\. You can also choose to use your own AWS Key Management Service customer master key \(CMK\)\. If you use your own CMK, you need the following permissions on the CMK\.
+ kms:CreateGrant
+ kms:DescribeKey

You optionally specify the CMK when you train the model with the console or when you call the `CreateProjectVersion` operation\. The CMK you use doesn't need to be the same CMK that you use to encrypt manifest and image files in your Amazon S3 bucket\. For more information, see [Step 5: \(Optional\) Encrypt training files](#su-encrypt-bucket)\. 

For more information, see [AWS Key Management Service concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys)\. Your source images are unaffected\.

For information about training a model, see [Training an Amazon Rekognition Custom Labels model](tm-train-model.md)\.