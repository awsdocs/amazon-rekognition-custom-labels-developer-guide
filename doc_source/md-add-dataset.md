# Adding a dataset to a project<a name="md-add-dataset"></a>

You can add a training dataset or a test dataset to an existing project\. If you want to replace an existing dataset, first delete the existing dataset\. For more information, see [Deleting a dataset](md-delete-dataset.md)\. Then, add the new dataset\. 

**Topics**
+ [Adding a dataset to a project \(Console\)](#md-add-dataset-console)
+ [Adding a dataset to a project \(SDK\)](#md-add-dataset-sdk)

## Adding a dataset to a project \(Console\)<a name="md-add-dataset-console"></a>

You can add a training or test dataset to a project by using the Amazon Rekognition Custom Labels console\.

**To add a dataset to a project**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose Use **Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\. 

1. In the left navigation pane, choose **Projects**\. The Projects view is shown\.

1. Choose the project to which you want to add a dataset\. 

1. In the left navigation pane, under the project name, choose **Datasets**\.

1. If the project doesn't have an existing dataset, the **Create dataset** page is shown\. Do the following:

   1. On the **Create dataset** page, enter the image source information\. For more information, see [Creating training and test datasets \(Console\)](md-create-dataset.md)\. 

   1. Choose **Create dataset** to create the dataset\.

1. If the project has an existing dataset \(training or test\), the project details page is shown\. Do the following: 

   1. On the project details page, choose **Actions**\.

   1. If you want to add a training dataset, choose **Create training dataset**\.

   1. If you want to add a test dataset, choose **Create test dataset**\.

   1. On the **Create dataset** page, enter the image source information\. For more information, see [Creating training and test datasets \(Console\)](md-create-dataset.md)\. 

   1. Choose **Create dataset** to create the dataset\.

1. Add images to your dataset\. For more information, see [Adding more images \(console\)](md-add-images.md#md-add-images-console)\.

1. Add labels to your dataset\. For more information, see [Add new labels \(Console\)](md-labels.md#md-add-new-labels)\.

1. Add labels to your images\. If you're adding image\-level labels, see [Assigning image\-level labels to an image](md-assign-image-level-labels.md)\. If you're adding bounding boxes, see [Locating objects with bounding boxes](md-localize-objects.md)\. For more information, see [Purposing datasets](md-dataset-purpose.md)\.

## Adding a dataset to a project \(SDK\)<a name="md-add-dataset-sdk"></a>

You can add a train or test dataset to an existing project in the following ways:
+ Create a dataset using a manifest file\. For more information, see [Creating a dataset with a manifest file \(SDK\)](md-create-dataset-ground-truth-sdk.md)\.
+ Create an empty dataset and populate the dataset afterwards\. The following example shows how to create an empty dataset\. To add entries after you create an empty dataset, see [Adding more images to a dataset](md-add-images.md)\.

**Topics**

**To add a dataset to a project \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following examples to add JSON lines to a dataset\.

------
#### [ CLI ]

   Replace `project_arn` with the project that you want to add the dataset set to\. Replace `dataset_type` with `TRAIN` to create a training dataset, or `TEST` to create a test dataset\. to escape any special characters within the JSON Line\.

   ```
   aws rekognition create-dataset --project-arn "project_arn" \
     --dataset-type dataset_type
   ```

------
#### [ Python ]

   Use the following code to create a dataset\. Supply the following command line options:
   + `project_arn` — the ARN of the project that you want to add the test dataset to\.
   + `type` — the type of dataset that you want to create \(train or test\)

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   import time
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def create_empty_dataset(rek_client, project_arn, dataset_type):
       """
       Creates an empty Amazon Rekognition Custom Labels dataset.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_arn: The ARN of the project in which you want to create a dataset.
       :param dataset_type: The type of the dataset that you wan to create (train or test).
       """
   
       try:
           #Create the dataset
           logger.info(f"Creating empty {dataset_type} dataset for project {project_arn}")
   
           dataset_type=dataset_type.upper()
   
           response = rek_client.create_dataset(
               ProjectArn=project_arn, DatasetType=dataset_type
           )
   
           dataset_arn=response['DatasetArn']
   
           logger.info(f"dataset ARN: {dataset_arn}")
   
           finished=False
           while finished==False:
   
               dataset=rek_client.describe_dataset(DatasetArn=dataset_arn)
   
               status=dataset['DatasetDescription']['Status']
               
               if status == "CREATE_IN_PROGRESS":
                   
                   logger.info((f"Creating dataset: {dataset_arn} "))
                   time.sleep(5)
                   continue
   
               if status == "CREATE_COMPLETE":
                   logger.info(f"Dataset created: {dataset_arn}")
                   finished=True
                   continue
   
               if status == "CREATE_FAILED":
                   logger.exception(f"Dataset creation failed: {status} : {dataset_arn}")
                   raise Exception (f"Dataset creation failed: {status} : {dataset_arn}")
                   
   
               logger.exception(f"Failed. Unexpected state for dataset creation: {status} : {dataset_arn}")
               raise Exception(f"Failed. Unexpected state for dataset creation: {status} : {dataset_arn}")
               
           return dataset_arn
          
       except ClientError as err:  
           logger.exception(f"Couldn't create dataset: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_arn", help="The ARN of the project in which you want to create the empty dataset."
       )
   
       parser.add_argument(
           "dataset_type", help="The type of the empty dataset that you want to create (train or test)."
       )
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Creating empty {args.dataset_type} dataset for project {args.project_arn}")
   
           #Create the empty dataset
           rek_client=boto3.client('rekognition')
   
           dataset_arn=create_empty_dataset(rek_client, 
               args.project_arn,
               args.dataset_type.lower())
   
           print(f"Finished creating empty dataset: {dataset_arn}")
   
   
       except ClientError as err:
           logger.exception(f"Problem creating empty dataset: {err}")
           print(f"Problem creating empty dataset: {err}")
       except Exception as err:
           logger.exception(f"Problem creating empty dataset: {err}")
           print(f"Problem creating empty dataset: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]

   Use the following code to create a dataset\. Supply the following command line options:
   + `project_arn` — the ARN of the project that you want to add the test dataset to\.
   + `type` — the type of dataset that you want to create \(train or test\)

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.CreateDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.CreateDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.DatasetDescription;
   import software.amazon.awssdk.services.rekognition.model.DatasetStatus;
   import software.amazon.awssdk.services.rekognition.model.DatasetType;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class CreateEmptyDataset {
   
       public static final Logger logger = Logger.getLogger(CreateEmptyDataset.class.getName());
   
       public static String createMyEmptyDataset(RekognitionClient rekClient, String projectArn, String datasetType)
               throws Exception, RekognitionException {
   
           try {
   
               logger.log(Level.INFO, "Creating empty {0} dataset for project : {1}",
                       new Object[] { datasetType.toString(), projectArn });
   
               DatasetType requestDatasetType = null;
   
               switch (datasetType) {
               case "train":
                   requestDatasetType = DatasetType.TRAIN;
                   break;
               case "test":
                   requestDatasetType = DatasetType.TEST;
                   break;
               default:
                   logger.log(Level.SEVERE, "Unrecognized dataset type: {0}", datasetType);
                   throw new Exception("Unrecognized dataset type: " + datasetType);
   
               }
   
               CreateDatasetRequest createDatasetRequest = CreateDatasetRequest.builder().projectArn(projectArn)
                       .datasetType(requestDatasetType).build();
   
               CreateDatasetResponse response = rekClient.createDataset(createDatasetRequest);
   
               boolean created = false;
               
               //Wait until updates finishes
   
               do {
   
                   DescribeDatasetRequest describeDatasetRequest = DescribeDatasetRequest.builder()
                           .datasetArn(response.datasetArn()).build();
                   DescribeDatasetResponse describeDatasetResponse = rekClient.describeDataset(describeDatasetRequest);
   
                   DatasetDescription datasetDescription = describeDatasetResponse.datasetDescription();
   
                   DatasetStatus status = datasetDescription.status();
   
                   logger.log(Level.INFO, "Creating dataset ARN: {0} ", response.datasetArn());
   
                   switch (status) {
   
                   case CREATE_COMPLETE:
                       logger.log(Level.INFO, "Dataset created");
                       created = true;
                       break;
   
                   case CREATE_IN_PROGRESS:
                       Thread.sleep(5000);
                       break;
   
                   case CREATE_FAILED:
                       String error = "Dataset creation failed: " + datasetDescription.statusAsString() + " "
                               + datasetDescription.statusMessage() + " " + response.datasetArn();
                       logger.log(Level.SEVERE, error);
                       throw new Exception(error);
   
                   default:
                       String unexpectedError = "Unexpected creation state: " + datasetDescription.statusAsString() + " "
                               + datasetDescription.statusMessage() + " " + response.datasetArn();
                       logger.log(Level.SEVERE, unexpectedError);
                       throw new Exception(unexpectedError);
                   }
   
               } while (created == false);
   
               return response.datasetArn();
   
           } catch (RekognitionException e) {
               logger.log(Level.SEVERE, "Could not create dataset: {0}", e.getMessage());
               throw e;
           }
   
       }
   
   
       public static void main(String args[]) {
   
           String datasetType = null;
           String datasetArn = null;
           String projectArn = null;
   
   
           final String USAGE = "\n" + "Usage: " + "<project_arn> <dataset_type>\n\n" + "Where:\n"
                   + "   project_arn - the ARN of the project that you want to add copy the datast to.\n\n"
                   + "   dataset_type - the type of the empty dataset that you want to create (train or test).\n\n";
                 
   
           if (args.length != 2) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           projectArn = args[0];
           datasetType = args[1];
           
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
               // Create the dataset
               datasetArn = createMyEmptyDataset(rekClient, projectArn, datasetType);
   
               System.out.println(String.format("Created dataset: %s", datasetArn));
   
               rekClient.close();
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           } catch (Exception rekError) {
               logger.log(Level.SEVERE, "Error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------

1. Add images to the dataset\. For more information, see [Adding more images \(SDK\)](md-add-images.md#md-add-images-sdk)\. 