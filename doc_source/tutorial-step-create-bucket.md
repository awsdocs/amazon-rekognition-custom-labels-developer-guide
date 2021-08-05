# Step 3: Create your project<a name="tutorial-step-create-bucket"></a>

The Amazon Rekognition Custom Labels console is where you create and manage your models\. The first time you use the console, Amazon Rekognition Custom Labels asks to create an Amazon S3 bucket in your account\. The bucket is used to store your Amazon Rekognition Custom Labels projects, datasets, and models\. You can't use the Amazon Rekognition Custom Labels console unless the bucket is created\.

Within Amazon Rekognition Custom Labels, you use projects to manage the models that you create\. A project manages the input images and labels, datasets, model training, model evaluation, and running the model\. For more information, see [Creating an Amazon Rekognition Custom Labels project](cp-create-project.md)\.

The first time you open the Amazon Rekognition Custom Labels console in a supported AWS Region, you are asked to create an Amazon S3 bucket \(console bucket\) to store your project files\. The format of the bucket name is *custom\-labels\-console\-\{region\}\-\{random value\}*\. The random value ensures that there isn't a collision between bucket names\.

**To create an Amazon Rekognition Custom Labels project**

1. Sign in to the AWS Management Console and open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose **Use Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\. If you don't see **Use Custom Labels**, check that the [ AWS Region](https://docs.aws.amazon.com/general/latest/gr/rekognition_region.html) you are using supports Amazon Rekognition Custom Labels\. 

1. Choose **Get started**\. 

1. If this is the first time that you've opened the console in the current AWS Region, the **First Time Set Up** dialog box is shown\. Do the following:

   1. Note the name of the Amazon S3 bucket that's shown\.

   1. Choose **Create S3 bucket** to let Amazon Rekognition Custom Labels create an Amazon S3 bucket \(console bucket\) on your behalf\. 

1. In the **Create Project** section, enter a name for your project\. 

1. Choose **Create project** to create your project\. The project page is shown\. A success message should be shown at the top of the page\.