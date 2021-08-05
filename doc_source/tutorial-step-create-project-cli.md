# Step 1: Create a project<a name="tutorial-step-create-project-cli"></a>

You use a project to manage your models\. A project contains the datasets and models that you create\. In this step, you create a project using [CreateProject](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProject) API\.

Before starting, read [Tutorial: Training a model with the Amazon Rekognition Custom Labels console](training-model-console.md)\. Specifically, do [Step 1: Decide your model type](ud-model-type.md) and [Step 2: Prepare your images](tutorial-step-prepare-images.md)\. 

**To create a project \(CLI\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` and `AmazonS3FullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

   1. Set permissions to access the Amazon Rekognition Custom Labels console\. For more information, see [Step 4: Set up Amazon Rekognition Custom Labels permissions](su-console-policy.md)\.

1. At the command prompt, enter the following command\. Replace `my_project` with a project name of your choice\.

   ```
   aws rekognition create-project --project-name my_project
   ```

1. Note the name of the project Amazon Resource Name \(ARN\) that's displayed in the response\. You'll need it to create a model\. 