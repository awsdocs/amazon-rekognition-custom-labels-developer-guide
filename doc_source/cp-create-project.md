# Creating an Amazon Rekognition Custom Labels Project<a name="cp-create-project"></a>

An Amazon Rekognition Custom Labels project is a grouping of the resources that are needed to create and manage an Amazon Rekognition Custom Labels model\. You use a project to manage your datasets and models:

Dataset –The labeled images used to train a model\. There can be multiple datasets in a project\. To train a model, you need a training dataset and a test dataset\. For more information, see [Creating an Amazon Rekognition Custom Labels Dataset](cd-create-dataset.md)\.

Model – A machine learning model created by Amazon Rekognition Custom Labels that analyzes images for objects, scenes, and concepts\. For more information, see [Training an Amazon Rekognition Custom Labels Model](tm-train-model.md)\. 

You can create a project with the Amazon Rekognition Custom Labels console or with the API\. When you first use the console, you specify the Amazon S3 bucket where Amazon Rekognition Custom Labels stores your project files\. If you're using the API you can also use Amazon S3 buckets external to the Amazon Rekognition Custom Labels console\.

**Topics**
+ [Create a Project \(Console\)](cp-console.md)
+ [Create a Project \(SDK\)](cp-sdk.md)