# Deleting a dataset<a name="md-delete-dataset"></a>

You can delete the training and test datasets from a project\. 

**Topics**
+ [Deleting a dataset \(Console\)](#md-delete-dataset-console)
+ [Deleting an Amazon Rekognition Custom Labels dataset \(SDK\)](#md-delete-dataset-sdk)

## Deleting a dataset \(Console\)<a name="md-delete-dataset-console"></a>

Use the following procedure to delete a dataset\. Afterwards, if the project has one remaining dataset \(train or test\), the project details page is shown\. If the project has no remaining datasets, the **Create dataset** page is shown\. 

If you delete the training dataset, you must create a new training dataset for the project before you can train a model\. For more information, see [Creating training and test datasets \(Console\)](md-create-dataset.md)\. 

If you delete the test dataset, you can train a model without creating a new test dataset\. During training, the training dataset is split to create a new test dataset for the project\. Splitting the training dataset reduces the number of images available for training\. To maintain quality, we recommend creating a new test dataset before training a model\. For more information, see [Adding a dataset to a project](md-add-dataset.md)\.

**To delete a dataset**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose Use **Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\. 

1. In the left navigation pane, choose **Projects**\. The Projects view is shown\.

1. Choose the project that contains the dataset that you want to delete\. 

1. In the left navigation pane, under the project name, choose **Dataset**

1. Choose **Actions**

1. To delete the training dataset, choose **Delete training dataset\.**

1. To delete the test dataset, choose **Delete test dataset\.**

1. In the **Delete *train or test* dataset** dialog box, enter **delete** to confirm that you want to delete the dataset\.

1. Choose **Delete *train or test* dataset** to delete the dataset\. 

## Deleting an Amazon Rekognition Custom Labels dataset \(SDK\)<a name="md-delete-dataset-sdk"></a>

You delete an Amazon Rekognition Custom Labels dataset by calling [DeleteDataset](https://docs.aws.amazon.com/rekognition/latest/dg/API_DeleteDataset) and supplying the Amazon Resource Name \(ARN\) of the dataset that you want to delete\. To get the ARNs of the training and test datasets within a project, call [DescribeProjects](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjects)\. The response includes an array of [ProjectDescription](https://docs.aws.amazon.com/rekognition/latest/dg/API_ProjectDescription) objects\. The dataset ARNs \(`DatasetArn`\) and dataset types \(`DatasetType`\) are in the `Datasets` list\. 

If you delete the training dataset, you need to create a new training dataset for the project before you can train a model\. If you delete the test dataset, you need to create a new test dataset before you can train the model\. For more information, see [Adding a dataset to a project \(SDK\)](md-add-dataset.md#md-add-dataset-sdk)\.

**To delete a dataset \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following code to delete a dataset\. 

------
#### [ AWS CLI ]

   Change the value of `dataset-arn` with the ARN of the dataset that you want to delete\.

   ```
   aws rekognition delete-dataset --dataset-arn dataset-arn 
   ```

------
#### [ Python ]

   Use the following code\. Supply the following command line parameters:
   + dataset\_arn — the ARN of the dataset that you want to delete\.

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   import time
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def delete_dataset(rek_client, dataset_arn):
       """
       Deletes an Amazon Rekognition Custom Labels dataset.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param dataset_arn: The ARN of the dataset that you want to delete.
       """
   
       try:
           #Delete the dataset
           logger.info(f"Deleting dataset: {dataset_arn}")
           
           rek_client.delete_dataset(DatasetArn=dataset_arn)
           
           deleted=False
   
           logger.info(f"waiting for dataset deletion {dataset_arn}")    
   
           #dataset might not be deleted yet, so wait.
           while deleted==False:
               try:
                   rek_client.describe_dataset(DatasetArn=dataset_arn)
                   time.sleep(5)
               except ClientError as err:
                   if err.response['Error']['Code'] == 'ResourceNotFoundException':
                       logger.info(f"dataset deleted: {dataset_arn}") 
                       deleted=True 
                   else:
                       raise
   
           logger.info(f"dataset deleted: {dataset_arn}")         
   
   
           return True
      
       
       except ClientError as err:  
           logger.exception(f"Couldn't delete dataset - {dataset_arn}: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "dataset_arn", help="The ARN of the dataset that you want to delete."
       )
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Deleting dataset: {args.dataset_arn}")
   
           #Delete the dataset
           rek_client=boto3.client('rekognition')
   
           delete_dataset(rek_client, 
               args.dataset_arn)
           
           print(f"Finished deleting dataset: {args.dataset_arn}")
   
     
       except ClientError as err:
           logger.exception(f"Problem deleting dataset: {err}")
           print(f"Problem deleting dataset: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]

   Use the following code\. Supply the following command line parameters:
   + dataset\_arn — the ARN of the dataset that you want to delete\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DeleteDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.DeleteDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   public class DeleteDataset {
   
       public static final Logger logger = Logger.getLogger(DeleteDataset.class.getName());
   
       public static void deleteMyDataset(RekognitionClient rekClient, String datasetArn) throws InterruptedException {
   
           try {
   
               logger.log(Level.INFO, "Deleting dataset: {0}", datasetArn);
   
               // Delete the dataset
   
               DeleteDatasetRequest deleteDatasetRequest = DeleteDatasetRequest.builder().datasetArn(datasetArn).build();
   
               DeleteDatasetResponse response = rekClient.deleteDataset(deleteDatasetRequest);
   
               // Wait until deletion finishes
   
               DescribeDatasetRequest describeDatasetRequest = DescribeDatasetRequest.builder().datasetArn(datasetArn)
                       .build();
   
               Boolean deleted = false;
   
               do {
   
                   try {
   
                       rekClient.describeDataset(describeDatasetRequest);
                       Thread.sleep(5000);
                   } catch (RekognitionException e) {
                       String errorCode = e.awsErrorDetails().errorCode();
                       if (errorCode.equals("ResourceNotFoundException")) {
                           logger.log(Level.INFO, "Dataset deleted: {}", datasetArn);
                           deleted = true;
                       } else {
                           logger.log(Level.SEVERE, "Client error occurred: {0}", e.getMessage());
                           throw e;
                       }
   
                   }
   
               } while (Boolean.FALSE.equals(deleted));
   
               logger.log(Level.INFO, "Dataset deleted: {0} ", datasetArn);
   
           } catch (
   
           RekognitionException e) {
               logger.log(Level.SEVERE, "Client error occurred: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
           final String USAGE = "\n" + "Usage: " + "<dataset_arn>\n\n" + "Where:\n"
                   + "   dataset_arn - The ARN of the dataset that you want to delete.\n\n";
   
           if (args.length != 1) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           String datasetArn = args[0];
   
           try {
   
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
   
               // Delete the dataset
               deleteMyDataset(rekClient, datasetArn);
   
               System.out.println(String.format("Dataset deleted: %s", datasetArn));
   
               rekClient.close();
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
           catch (InterruptedException intError) {
               logger.log(Level.SEVERE, "Exception while sleeping: {0}", intError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------