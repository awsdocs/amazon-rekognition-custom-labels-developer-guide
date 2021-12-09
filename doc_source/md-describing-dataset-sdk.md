# Describing a dataset \(SDK\)<a name="md-describing-dataset-sdk"></a>

You can use the `DescribeDataset` API to get information about a dataset\.

**To describe a dataset \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to describe a dataset\.

------
#### [ AWS CLI ]

   Change the value of `dataset-arn` to the ARN of the dataset that you want to describe\.

   ```
   aws rekognition describe-dataset --dataset-arn dataset_arn
   ```

------
#### [ Python ]

   Use the following code\. Supply the following command line parameters:
   + dataset\_arn — the ARN of the dataset that you want to describe\.

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def describe_dataset(rek_client, dataset_arn):
       """
       Describes an Amazon Rekognition Custom Labels dataset.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param dataset_arn: The ARN of the dataset that you want to describe.
   
       """
   
       try:
           #Describe the dataset
           logger.info(f"Describing dataset {dataset_arn}")
   
           dataset=rek_client.describe_dataset(DatasetArn=dataset_arn)
   
           description = dataset['DatasetDescription']
   
           print(f"Created: {str(description['CreationTimestamp'])}")
           print(f"Updated: {str(description['LastUpdatedTimestamp'])}")
           print(f"Status: {description['Status']}")
           print(f"Status message: {description['StatusMessage']}")
           print(f"Status code: {description['StatusMessageCode']}")
           print("Stats:")
           print(f"\tLabeled entries: {description['DatasetStats']['LabeledEntries']}")
           print(f"\tTotal entries: {description['DatasetStats']['TotalEntries']}")
           print(f"\tTotal labels: {description['DatasetStats']['TotalLabels']}")
   
       
       except ClientError as err:  
           logger.exception(f"Couldn't describe dataset: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "dataset_arn", help="The ARN of the dataset that you want to describe."
       )
   
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Describing dataset {args.dataset_arn}")
   
           #Describe the dataset
           rek_client=boto3.client('rekognition')
   
   
           describe_dataset(rek_client, 
               args.dataset_arn
           )
   
           print(f"Finished describing dataset: {args.dataset_arn}")
   
   
       except ClientError as err:
           logger.exception(f"Problem describing dataset: {err}")
           print(f"Problem describing dataset: {err}")
       except Exception as err:
           logger.exception(f"Problem describing dataset: {err}")
           print(f"Problem describing dataset: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java V2 ]
   + dataset\_arn — the ARN of the dataset that you want to describe\.

   ```
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DatasetDescription;
   import software.amazon.awssdk.services.rekognition.model.DatasetStats;
   import software.amazon.awssdk.services.rekognition.model.DatasetStatus;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class DescribeDataset {
   
       public static final Logger logger = Logger.getLogger(DescribeDataset.class.getName());
   
       public static void describeMyDataset(RekognitionClient rekClient, String datasetArn) {
   
           try {
   
               DescribeDatasetRequest describeDatasetRequest = DescribeDatasetRequest.builder().datasetArn(datasetArn)
                       .build();
               DescribeDatasetResponse describeDatasetResponse = rekClient.describeDataset(describeDatasetRequest);
   
               DatasetDescription datasetDescription = describeDatasetResponse.datasetDescription();
               DatasetStats datasetStats = datasetDescription.datasetStats();
   
               System.out.println("ARN: " + datasetArn);
               System.out.println("Created: " + datasetDescription.creationTimestamp().toString());
               System.out.println("Updated: " + datasetDescription.lastUpdatedTimestamp().toString());
               System.out.println("Status: " + datasetDescription.statusAsString());
               System.out.println("Message: " + datasetDescription.statusMessage());
               System.out.println("Total Labels: " + datasetStats.totalLabels().toString());
               System.out.println("Total entries: " + datasetStats.totalEntries().toString());
               System.out.println("Entries with labels: " + datasetStats.labeledEntries().toString());
               System.out.println("Entries with at least 1 error: " + datasetStats.errorEntries().toString());
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               throw rekError;
           }
   
       }
   
       public static void main(String args[]) {
   
           final String USAGE = "\n" + "Usage: " + "<dataset_arn>\n\n" + "Where:\n"
                   + "   dataset_arn - The ARN of the dataset that you want to describe.\n\n";
   
           if (args.length != 1) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           String datasetArn = args[0];
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
                // Describe the dataset
               describeMyDataset(rekClient, datasetArn);
   
               rekClient.close();
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------