# Training an Amazon Rekognition Custom Labels model<a name="training-model"></a>

You can train a model by using the Amazon Rekognition Custom Labels console, or by the Amazon Rekognition Custom Labels API\. If model training fails, use the information in [Debugging a failed model training](tm-debugging.md) to find the cause of the failure\.

**Note**  
You are charged for the amount of time that it takes to successfully train a model\. Typically training takes from 30 minutes to 24 hours to complete\. For more information, see [Training hours](https://aws.amazon.com/rekognition/pricing/#Amazon_Rekognition_Custom_Labels_pricing)\. 

A new version of a model is created every time the model is trained\. Amazon Rekognition Custom Labels creates a name for the model that is a combination of the project name and the timestamp for when the model is created\. 

To train your model, Amazon Rekognition Custom Labels makes a copy of your source training and test images\. By default the copied images are encrypted at rest with a key that AWS owns and manages\. You can also choose to use your own AWS KMS key\. If you use your own KMS key, you need the following permissions on the KMS key\.
+ kms:CreateGrant
+ kms:DescribeKey

For more information, see [AWS Key Management Service concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys)\. Your source images are unaffected\.

You can use KMS server\-side encryption \(SSE\-KMS\) to encrypt the training and test images in your Amazon S3 bucket, before they are copied by Amazon Rekognition Custom Labels\. To allow Amazon Rekognition Custom Labels access to your images, your account needs the following permissions on the KMS key\.
+ kms:GenerateDataKey
+ kms:Decrypt

For more information, see [Protecting Data Using Server\-Side Encryption with KMS keys Stored in AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)\.

Optionally, you can manage your models by using tags\. For more information, see [Tagging a model](tm-tagging-model.md)\.

After training a model, you can evaluate its performance and make improvements\. For more information, see [Improving a trained Amazon Rekognition Custom Labels model](improving-model.md)\.

**Topics**
+ [Training a model \(Console\)](#tm-console)
+ [Training a model \(SDK\)](#tm-sdk)

## Training a model \(Console\)<a name="tm-console"></a>

You can use the Amazon Rekognition Custom Labels console to train a model\.

Training requires a project with a training dataset and a test dataset\. If your project doesn't have a test dataset, the Amazon Rekognition Custom Labels console splits the training dataset during training to create one for your project\. The images chosen are a representative sampling and aren't used in the training dataset\. We recommend splitting your training dataset only if you don't have an alternative test dataset that you can use\. Splitting a training dataset reduces the number of images available for training\.

**Note**  
You are charged for the amount of time that it takes to train a model\. For more information, see [Training hours](https://aws.amazon.com/rekognition/pricing/#Amazon_Rekognition_Custom_Labels_pricing)\. 

**To train your model \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project that contains the model that you want to train\. 

1. On the **Project** page, choose **Train model**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/tutorial-train-model.jpg)

1. \(Optional\) If you want to use your own AWS KMS encryption key, do the following:

   1. In **Image data encryption** choose **Customize encryption settings \(advanced\)**\.

   1. In **encryption\.aws\_kms\_key** enter the Amazon Resource Name \(ARN\) of your key, or choose an existing AWS KMS key\. To create a new key, choose **Create an AWS IMS key**\.

1. \(Optional\) if you want to add tags to your model do the following:

   1. In the **Tags** section, choose **Add new tag**\.

   1. Enter the following:

      1. The name of the key in **Key**\.

      1. The value of the key in **Value**\.

   1. To add more tags, repeat steps 6a and 6b\.

   1. \(Optional\) If you want to remove a tag, choose **Remove** next to the tag that you want to remove\. If you are removing a previously saved tag, it is removed when you save your changes\.

1. On the **Train model** page, Choose **Train model**\. The Amazon Resource Name \(ARN\) for your project should be in the **Choose project** edit box\. If not, enter the ARN for your project\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/tutorial-train-model-page-train-model.jpg)

1. In the **Do you want to train your model?** dialog box, choose **Train model**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/tutorial-dialog-train-model.jpg)

1. In the **Models** section of the project page, you can check the current status in the `Model Status` column, where the training's in progress\. Training a model takes a while to complete\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/tutorial-training-progress.jpg)

1. After training completes, choose the model name\. Training is finished when the model status is **TRAINING\_COMPLETED**\. If training fails, read [Debugging a failed model training](tm-debugging.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/get-started-choose-model.jpg)

1. Next step: Evaluate your model\. For more information, [Improving a trained Amazon Rekognition Custom Labels model](improving-model.md)\.

## Training a model \(SDK\)<a name="tm-sdk"></a>

You train a model by calling [CreateProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion)\. To train a model, the following information is needed:
+ Name – A unique name for the model version\.
+ Project ARN – The Amazon Resource Name \(ARN\) of the project that manages the model\.
+ Training results location – The Amazon S3 location where the results are placed\. You can use the same location as the console Amazon S3 bucket, or you can choose a different location\. We recommend choosing a different location because this allows you to set permissions and avoid potential naming conflicts with training output from using the Amazon Rekognition Custom Labels console\.

Training uses the training and test datasets associated with project\. For more information, see [Managing datasets](managing-dataset.md)\. 

**Note**  
Optionally, you can specify training and test dataset manifest files that are external to a project\. If you open the console after training a model with external manifest files, Amazon Rekognition Custom Labels creates the datasets for you by using the last set of manifest files used for training\. You can no longer train a model version for the project by specifying external manifest files\. For more information, see [CreatePrjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion)\. 

The response from `CreateProjectVersion` is an ARN that you use to identify the model version in subsequent requests\. You can also use the ARN to secure the model version\. For more information, see [Securing Amazon Rekognition Custom Labels projects](sc-introduction.md#sc-resources)\.

Training a model version takes a while to complete\. The Python and Java examples in this topic use waiters to wait for training to complete\. A waiter is a utility method that polls for a particular state to occur\. Alternatively, you can get the current status of training by calling `DescribeProjectVersions`\. Training is completed when the `Status` field value is `TRAINING_COMPLETED`\. After training is completed, you can evaluate model’s quality by reviewing the evaluation results\. 

**Topics**
+ [Training a model \(SDK\)](#tm-sdk-datasets)

### Training a model \(SDK\)<a name="tm-sdk-datasets"></a>

The following example shows how to train a model by using the training and test datasets associated with a project\.

**To train a model \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` and permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to train a project\.

------
#### [ AWS CLI ]

   The following example creates a model\. The training dataset is split to create the testing dataset\. Replace the following:
   + `my_project_arn` with the Amazon Resource Name \(ARN\) of the project\.
   + `version_name` with a unique version name of your choosing\.
   + `output_bucket` with the name of the Amazon S3 bucket where Amazon Rekognition Custom Labels saves the training results\.
   + `output_folder` with the name of the folder where the training results are saved\.
   + \(optional parameter\) `--kms-key-id` with identifier for your AWS Key Management Service customer master key\.

   ```
   aws rekognition create-project-version\
                               --project-arn "project_arn"\
                               --version-name "version_name"\
                               --output-config '{"S3Bucket":"output_bucket", "S3KeyPrefix":"output_folder"}'
   ```

------
#### [ Python ]

   The following example creates a model\. Supply the following command line arguments:
   + `project_arn` – The Amazon Resource Name \(ARN\) of the project\.
   + `version_name` – A unique version name for the model of your choosing\.
   + `output_bucket` – the name of the Amazon S3 bucket where Amazon Rekognition Custom Labels saves the training results\.
   + `output_folder` – the name of the folder where the training results are saved\.

   Optionally, supply the folowing command line parameters to attach a tag to your model:
   + `tag` – a tag name of your choosing that you want to attach to the model\.
   + `tag_value` the tag value\. 

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   from os import stat_result
   import boto3
   import argparse
   import logging
   import time
   import json
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def train_model(rek_client, project_arn, version_name, output_bucket, output_folder, tag_key, tag_key_value):
       """
       Trains an Amazon Rekognition Custom Labels model.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_arn: The ARN of the project in which you want to train a model.
       :param version_name: A version for the model.
       :param output_bucket: The S3 bucket that hosts training output.
       :param output_folder: The path for the training output within output_bucket
       :param tag_key: The name of a tag to attach to the model. Pass None to exclude
       :param tag_key_value: The value of the tag. Pass None to exclude
   
       """
   
       try:
           #Train the model
   
           status="" 
           logger.info(f"training model version {version_name} for project {project_arn}")
   
   
           output_config = json.loads(
               '{"S3Bucket": "'
               + output_bucket
               + '", "S3KeyPrefix": "'
               + output_folder
               + '" }  '
           )
   
           tags={}
   
           if tag_key!=None and tag_key_value !=None:
               tags = json.loads(
                   '{"' + tag_key + '":"' + tag_key_value + '"}'
               )
   
   
           response=rek_client.create_project_version(
               ProjectArn=project_arn, 
               VersionName=version_name,
               OutputConfig=output_config,
               Tags=tags
           )
   
           logger.info(f"Started training: {response['ProjectVersionArn']}")
   
           # Wait for the project version training to complete
   
           project_version_training_completed_waiter = rek_client.get_waiter('project_version_training_completed')
           project_version_training_completed_waiter.wait(ProjectArn=project_arn,
           VersionNames=[version_name])
       
   
           #Get the completion status
           describe_response=rek_client.describe_project_versions(ProjectArn=project_arn,
               VersionNames=[version_name])
           for model in describe_response['ProjectVersionDescriptions']:
               logger.info("Status: " + model['Status'])
               logger.info("Message: " + model['StatusMessage']) 
               status=model['Status']
   
   
           logger.info(f"finished training")
   
           return response['ProjectVersionArn'], status
       
       except ClientError as err:  
           logger.exception(f"Couldn't create dataset: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_arn", help="The ARN of the project in which you want to train a model"
       )
   
       parser.add_argument(
           "version_name", help="A version name of your choosing."
       )
   
       parser.add_argument(
           "output_bucket", help="The S3 bucket that receives the training results."
       )
   
       parser.add_argument(
           "output_folder", help="The folder in the S3 bucket where training results are stored."
       )
   
       parser.add_argument(
           "--tag_name",  help="The name of a tag to attach to the model", required=False
       )
   
       parser.add_argument(
           "--tag_value",  help="The value for the tag.", required=False
       )
   
   
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Training model version {args.version_name} for project {args.project_arn}")
   
           #Train the model
           rek_client=boto3.client('rekognition')
   
           model_arn, status=train_model(rek_client, 
               args.project_arn,
               args.version_name,
               args.output_bucket,
               args.output_folder,
               args.tag_name,
               args.tag_value)
   
   
           print(f"Finished training model: {model_arn}")
           print(f"Status: {status}")
   
   
       except ClientError as err:
           logger.exception(f"Problem training model: {err}")
           print(f"Problem training model: {err}")
       except Exception as err:
           logger.exception(f"Problem training model: {err}")
           print(f"Problem training model: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java ]

   The following example trains a model\. Supply the following command line arguments:
   + `project_arn` – The Amazon Resource Name \(ARN\) of the project\.
   + `version_name` – A unique version name for the model of your choosing\.
   + `output_bucket` – the name of the Amazon S3 bucket where Amazon Rekognition Custom Labels saves the training results\.
   + `output_folder` – the name of the folder where the training results are saved\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.core.waiters.WaiterResponse;
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.CreateProjectVersionRequest;
   import software.amazon.awssdk.services.rekognition.model.CreateProjectVersionResponse;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsResponse;
   import software.amazon.awssdk.services.rekognition.model.OutputConfig;
   import software.amazon.awssdk.services.rekognition.model.ProjectVersionDescription;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   import software.amazon.awssdk.services.rekognition.waiters.RekognitionWaiter;
   
   import java.net.URI;
   import java.util.Optional;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class TrainModel {
   
       public static final Logger logger = Logger.getLogger(TrainModel.class.getName());
   
       public static String trainMyModel(RekognitionClient rekClient, String projectArn, String versionName,
               String outputBucket, String outputFolder) {
   
           try {
   
               OutputConfig outputConfig = OutputConfig.builder().s3Bucket(outputBucket).s3KeyPrefix(outputFolder).build();
   
               logger.log(Level.INFO, "Training Model for project {0}", projectArn);
               CreateProjectVersionRequest createProjectVersionRequest = CreateProjectVersionRequest.builder()
                       .projectArn(projectArn).versionName(versionName).outputConfig(outputConfig).build();
   
               CreateProjectVersionResponse response = rekClient.createProjectVersion(createProjectVersionRequest);
   
               logger.log(Level.INFO, "Model ARN: {0}", response.projectVersionArn());
               logger.log(Level.INFO, "Training model...");
   
               // wait until training completes
   
               DescribeProjectVersionsRequest describeProjectVersionsRequest = DescribeProjectVersionsRequest.builder()
                       .versionNames(versionName)
                       .projectArn(projectArn)
                       .build();
   
               RekognitionWaiter waiter = rekClient.waiter();
   
               WaiterResponse<DescribeProjectVersionsResponse> waiterResponse = waiter
                       .waitUntilProjectVersionTrainingCompleted(describeProjectVersionsRequest);
   
               Optional<DescribeProjectVersionsResponse> optionalResponse = waiterResponse.matched().response();
   
               DescribeProjectVersionsResponse describeProjectVersionsResponse = optionalResponse.get();
   
               for (ProjectVersionDescription projectVersionDescription : describeProjectVersionsResponse
                       .projectVersionDescriptions()) {
                   System.out.println("ARN: " + projectVersionDescription.projectVersionArn());
                   System.out.println("Status: " + projectVersionDescription.statusAsString());
                   System.out.println("Message: " + projectVersionDescription.statusMessage());
               }
   
               return response.projectVersionArn();
   
           } catch (RekognitionException e) {
               logger.log(Level.SEVERE, "Could not train model: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
           String versionName = null;
           String projectArn = null;
           String projectVersionArn = null;
           String bucket = null;
           String location = null;
   
           final String USAGE = "\n" + "Usage: " + "<project_name> <version_name> <output_bucket> <output_folder>\n\n" + "Where:\n"
                   + "   project_arn - The ARN of the project that you want to use. \n\n"
                   + "   version_name - A version name for the model.\n\n"
                   + "   output_bucket - The S3 bucket in which to place the training output. \n\n"
                   + "   output_folder - The folder within the bucket that the training output is stored in. \n\n";
   
           if (args.length != 4) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           projectArn = args[0];
           versionName = args[1];
           bucket = args[2];
           location = args[3];
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
               // Train model
               projectVersionArn = trainMyModel(rekClient, projectArn, versionName, bucket, location);
   
               System.out.println(String.format("Created model: %s for Project ARN: %s", projectVersionArn, projectArn));
   
               rekClient.close();
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------

1. If training fails, read [Debugging a failed model training](tm-debugging.md)\. 