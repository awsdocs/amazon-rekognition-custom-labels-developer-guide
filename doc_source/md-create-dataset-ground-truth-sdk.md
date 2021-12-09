# Creating a dataset with a manifest file \(SDK\)<a name="md-create-dataset-ground-truth-sdk"></a>

The following procedure shows you how to create training or test datasets from a manifest using the `CreateDataset` API\.

You can use an existing manifest file, such as the output from an SageMaker Groundtruth job, or create your own manifest file\. To create your own manifest file, see [Creating a manifest file](md-create-manifest-file.md)\. 

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` and permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to create the training and test dataset\.

------
#### [ AWS CLI ]

   Use the following code to create a dataset\. Replace the following:
   + `project_arn` — the ARN of the project that you want to add the test dataset to\.
   + `type` — the type of dataset that you want to create \(TRAIN or TEST\)
   + `bucket` — the bucket that contains the manifest file for the dataset\.
   + `manifest_file` — the path and file name of the manifest file\.

   ```
   aws rekognition create-dataset --project-arn project_arn \
     --dataset-type type \
     --dataset-source '{ "GroundTruthManifest": { "S3Object": { "Bucket": "bucket", "Name": "manifest_file" } } }'
   ```

------
#### [ Python ]

   Use the following values to create a dataset\. Supply the following command line parameters:
   + `project_arn` — the ARN of the project that you want to add the test dataset to\.
   + `dataset_type` — the type of dataset that you want to create \(`train` or `test`\)\.
   + `bucket` — the bucket that contains the manifest file for the dataset\.
   + `manifest_file` — the path and file name of the manifest file\.

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   import time
   import json
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def create_dataset(rek_client, project_arn, dataset_type, bucket, manifest_file):
       """
       Creates an Amazon Rekognition Custom Labels dataset.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_arn: The ARN of the project in which you want to create a dataset.
       :param dataset_type: The type of the dataset that you wan to create (train or test).
       :param bucket: The S3 bucket that contains the manifest file.
       :param manifest_file: The path and filename of the manifest file.
       """
   
       try:
           #Create the project
           logger.info(f"Creating {dataset_type} dataset for project {project_arn}")
   
           dataset_type = dataset_type.upper()
   
           dataset_source = json.loads(
               '{ "GroundTruthManifest": { "S3Object": { "Bucket": "'
               + bucket
               + '", "Name": "'
               + manifest_file
               + '" } } }'
           )
   
           response = rek_client.create_dataset(
               ProjectArn=project_arn, DatasetType=dataset_type, DatasetSource=dataset_source
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
           "project_arn", help="The ARN of the project in which you want to create the dataset."
       )
   
       parser.add_argument(
           "dataset_type", help="The type of the dataset that you want to create (train or test)."
       )
   
       parser.add_argument(
           "bucket", help="The S3 bucket that contains the manifest file."
       )
       
       parser.add_argument(
           "manifest_file", help="The path and filename of the manifest file."
       )
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Creating {args.dataset_type} dataset for project {args.project_arn}")
   
           #Create the project
           rek_client=boto3.client('rekognition')
   
           dataset_arn=create_dataset(rek_client, 
               args.project_arn,
               args.dataset_type,
               args.bucket,
               args.manifest_file)
   
           print(f"Finished creating dataset: {dataset_arn}")
   
   
       except ClientError as err:
           logger.exception(f"Problem creating dataset: {err}")
           print(f"Problem creating dataset: {err}")
       except Exception as err:
           logger.exception(f"Problem creating dataset: {err}")
           print(f"Problem creating dataset: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java V2 ]

   Use the following values to create a dataset\. Supply the following command line parameters:
   + `project_arn` — the ARN of the project that you want to add the test dataset to\.
   + `dataset_type` — the type of dataset that you want to create \(`train` or `test`\)\.
   + `bucket` — the bucket that contains the manifest file for the dataset\.
   + `manifest_file` — the path and file name of the manifest file\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.CreateDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.CreateDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.DatasetDescription;
   import software.amazon.awssdk.services.rekognition.model.DatasetSource;
   import software.amazon.awssdk.services.rekognition.model.DatasetStatus;
   import software.amazon.awssdk.services.rekognition.model.DatasetType;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.GroundTruthManifest;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   import software.amazon.awssdk.services.rekognition.model.S3Object;
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class CreateDatasetManifestFiles {
   
       public static final Logger logger = Logger.getLogger(CreateDatasetManifestFiles.class.getName());
   
       public static String createMyDataset(RekognitionClient rekClient, String projectArn, String datasetType,
               String bucket, String name) throws Exception, RekognitionException {
   
           try {
   
               logger.log(Level.INFO, "Creating {0} dataset for project : {1} from s3://{2}/{3} ",
                       new Object[] { datasetType.toString(), projectArn, bucket, name });
   
               DatasetType requestDatasetType = null;
   
               switch (datasetType) {
               case "train":
                   requestDatasetType = DatasetType.TRAIN;
                   break;
               case "test":
                   requestDatasetType = DatasetType.TEST;
                   break;
               default:
                   logger.log(Level.SEVERE, "Couldn't create dataset. Unrecognized dataset type: {0}", datasetType);
                   throw new Exception("Couldn't create dataset. Unrecognized dataset type: " + datasetType);
   
               }
   
               GroundTruthManifest groundTruthManifest = GroundTruthManifest.builder()
                       .s3Object(S3Object.builder().bucket(bucket).name(name).build()).build();
   
               DatasetSource datasetSource = DatasetSource.builder().groundTruthManifest(groundTruthManifest).build();
   
               CreateDatasetRequest createDatasetRequest = CreateDatasetRequest.builder().projectArn(projectArn)
                       .datasetType(requestDatasetType).datasetSource(datasetSource).build();
   
               CreateDatasetResponse response = rekClient.createDataset(createDatasetRequest);
   
               boolean created = false;
   
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
           String bucket = null;
           String name = null;
           String projectArn = null;
           String datasetArn = null;
   
           final String USAGE = "\n" + "Usage: " + "<project_arn> <dataset_type> <dataset_arn>\n\n" + "Where:\n"
                   + "   project_arn - the ARN of the project that you want to add copy the datast to.\n\n"
                   + "   dataset_type - the type of the dataset that you want to create (train or test).\n\n"
                   + "   bucket - the S3 bucket that contains the manifest file.\n\n"
                   + "   name - the location and name of the manifest file within the bucket.\n\n";
   
           if (args.length != 4) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           projectArn = args[0];
           datasetType = args[1];
           bucket = args[2];
           name = args[3];
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
                // Create the dataset
               datasetArn = createMyDataset(rekClient, projectArn, datasetType, bucket, name);
   
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