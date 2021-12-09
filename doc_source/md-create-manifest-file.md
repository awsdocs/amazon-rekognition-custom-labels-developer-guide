# Creating a manifest file<a name="md-create-manifest-file"></a>

You can create a test or training dataset by importing a SageMaker Ground Truth format manifest file\. If your images are labeled in a format that isn't a SageMaker Ground Truth manifest file, use the following information to create a SageMaker Ground Truth format manifest file\. 

Manifest files are in [JSON lines](http://jsonlines.org) format where each line is a complete JSON object representing the labeling information for an image\. Amazon Rekognition Custom Labels supports SageMaker Ground Truth manifests with JSON lines in the following formats:
+ [Classification Job Output](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-output.html#sms-output-class) – Use to add image\-level labels to an image\. An image\-level label defines the class of scene, concept, or object \(if object location information isn't needed\) that's on an image\. An image can have more that one image\-level label\. For more information, see [Image\-Level labels in manifest files](md-create-manifest-file-classification.md)\.
+ [Bounding Box Job Output](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-output.html#sms-output-box) – Use to label the class and location of one or more objects on an image\. For more information, see [Object localization in manifest files](md-create-manifest-file-object-detection.md)\.

Image\-level and localization \(bounding\-box\) JSON lines can be chained together in the same manifest file\. 

**Note**  
The JSON line examples in this section are formatted for readability\. 

When you import a manifest file, Amazon Rekognition Custom Labels applies validation rules for limits, syntax, and semantics\. For more information, see [Validation rules for manifest files](md-create-manifest-file-validation-rules.md)\. 

The images referenced by a manifest file must be located in the same Amazon S3 bucket\. The manifest file can be located in a different Amazon S3 bucket than the Amazon S3 bucket that stores the images\. You specify the location of an image in the `source-ref` field of a JSON line\. 

Amazon Rekognition needs permissions to access the Amazon S3 bucket where your images are stored\. If you are using the console bucket set up for you by Amazon Rekognition Custom Labels, the required permissions are already set up\. If you are not using the console bucket, see [Accessing external Amazon S3 Buckets](su-console-policy.md#su-external-buckets)\.

## Creating a dataset with a manifest file<a name="md-create-manifest-file-console"></a>

The following procedure creates a project with a training and test dataset\. The datasets are created from training and test manifest files that you create\.

<a name="create-dataset-procedure-manifest-file"></a>

**To create a dataset using a SageMaker Ground Truth format manifest file \(console\)**

1. In the Console Bucket, [create a folder](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-folder.html) to hold your manifest files\. 

1. In the console bucket, create a folder to hold your images\.

1. Upload your images to the folder you just created\.

1. Create a SageMaker Ground Truth format manifest file for your training dataset\. For more information, see [Image\-Level labels in manifest files](md-create-manifest-file-classification.md) and [Object localization in manifest files](md-create-manifest-file-object-detection.md)\.
**Important**  
The `source-ref` field value in each JSON line must map to an image that you uploaded\.

1. Create an SageMaker Ground Truth format manifest file for your test dataset\. 

1. [Upload your manifest files](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) to the folder that you just created\.

1. Sign in to the AWS Management Console and open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose **Use Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\.

1. The Amazon Rekognition Custom Labels landing page, choose **Get started**\.

1. In the left pane, Choose **Projects**\. 

1. Choose **Create Project**\. 

1. In **Project name**, enter a name for your project\. 

1. Choose **Create project** to create your project\. 

1. Choose **Create dataset**\. The **Create dataset** page is shown\.

1. In **Starting configuration**, choose **Start with a training dataset and a test dataset**\. 

1. In the **Training dataset details** section, choose **Import images labeled by SageMaker Ground Truth**\.

1. In **\.manifest file location** enter the Amazon S3 location of the training manifest that you uploaded in step 6\. 

1. In the **Test dataset details** section, choose **Import images labeled by SageMaker Ground Truth**\.

1. In **\.manifest file location** enter the Amazon S3 location of the test manifest file that uploaded in step 6\.

1. Choose **Create Datasets**\. 

1. Train the model\. For more information, see [Training an Amazon Rekognition Custom Labels model](training-model.md)\.

## Creating a dataset with a manifest file \(SDK\)<a name="md-create-dataset-manifest-file-sdk"></a>

To create a dataset with a manifest file, use the `CreateDataset` API\. 

**To create a dataset with a manifest file\(SDK\)**
+ Use the following example code to create the dataset\.

------
#### [ AWS CLI ]

  Use the following code to create a dataset\. Supply the following command line options:
  + `project_arn` — the ARN of the project that you want to add the test dataset to\.
  + `type` — the type of dataset that you want to create \(train or test\)
  + `bucket` — the bucket that contains the manifest file for the dataset\.
  + manifest\_file — the path and file name of the manifest file\.

  ```
  aws rekognition create-dataset --project-arn project_arn \
    --dataset-type type\
    --dataset-source '{ "GroundTruthManifest": { "S3Object": { "Bucket": "bucket", "Name": "manifest_file" } } }' \
  ```

------
#### [ Python ]

  The following example creates a dataset\.

  Use the following values to create the training dataset\. Supply the following command line parameters:
  + `project_arn` — the ARN of the project that you want to add the test dataset to\.
  + `dataset_type` — the type of dataset that you want to create \(train or test\)
  + `bucket` — the bucket that contains the manifest file for the dataset\.
  + manifest\_file — the path and file name of the manifest file\.

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
#### [ Java 2 ]

  The following example creates a dataset\.

  Use the following values to create the training dataset\. Supply the following command line parameters:
  + `project_arn` — the ARN of the project that you want to add the test dataset to\.
  + `dataset_type` — the type of dataset that you want to create \(train or test\)
  + `bucket` — the bucket that contains the manifest file for the dataset\.
  + manifest\_file — the path and file name of the manifest file\.

  ```
  //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
  //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
  
  
  import software.amazon.awssdk.services.rekognition.RekognitionClient;
  import software.amazon.awssdk.services.rekognition.model.CreateDatasetRequest;
  import software.amazon.awssdk.services.rekognition.model.CreateDatasetResponse;
  import software.amazon.awssdk.services.rekognition.model.CreateProjectRequest;
  import software.amazon.awssdk.services.rekognition.model.CreateProjectResponse;
  import software.amazon.awssdk.services.rekognition.model.DatasetDescription;
  import software.amazon.awssdk.services.rekognition.model.DatasetSource;
  import software.amazon.awssdk.services.rekognition.model.DatasetStatus;
  import software.amazon.awssdk.services.rekognition.model.DescribeDatasetRequest;
  import software.amazon.awssdk.services.rekognition.model.DescribeDatasetResponse;
  import software.amazon.awssdk.services.rekognition.model.DescribeProjectsRequest;
  import software.amazon.awssdk.services.rekognition.model.DescribeProjectsResponse;
  import software.amazon.awssdk.services.rekognition.model.ProjectDescription;
  import software.amazon.awssdk.services.rekognition.model.RekognitionException;
  
  import java.util.List;
  import java.util.Objects;
  import java.util.logging.Level;
  import java.util.logging.Logger;
  
  public class CreateDataset {
  
      public static final Logger logger = Logger.getLogger(CreateDataset.class.getName());
  
      public static String createMyDataset(RekognitionClient rekClient, String projectArn, String datasetArn) {
  
          try {
  
              logger.log(Level.INFO, "Creating dataset for project : {0} from dataset {1} ",
              		new Object[] {projectArn,datasetArn});
              
              DatasetSource datasetSource = DatasetSource.builder()
              		.datasetArn(datasetArn).build();
              
              CreateDatasetRequest createDatasetRequest = CreateDatasetRequest.builder()
              		.datasetSource(datasetSource).build();
  
              CreateDatasetResponse response = rekClient.createDataset(createDatasetRequest);
              
              Boolean deleted = false;
  
              do {
  
                  DescribeDatasetRequest describeDatasetRequest = DescribeDatasetRequest.builder()
                  		.datasetArn(response.datasetArn())
                  		.build();
                  DescribeDatasetResponse describeDatasetResponse = rekClient.describeDataset(describeDatasetRequest);
    
                  DatasetStatus status = describeDatasetResponse.datasetDescription().status();
                  
                  switch (status) {
                  
                  case (DatasetStatus) status.CREATE_COMPLETE:
                  	logger.log(Level.INFO, "Dataset created");
                  
                  }
                  
  
                  deleted = true;
  
                  for (ProjectDescription projectDescription : projectDescriptions) {
  
                      if (Objects.equals(projectDescription.projectArn(), projectArn)) {
                          deleted = false;
                          logger.log(Level.INFO, "Not deleted: {0}", projectDescription.projectArn());
                          Thread.sleep(5000);
                          break;
  
                      }
                  }
  
              } while (Boolean.FALSE.equals(deleted));
  
              logger.log(Level.INFO, "Project ARN: {0} ", response.projectArn());
  
              return response.projectArn();
  
          } catch (RekognitionException e) {
              logger.log(Level.SEVERE, "Could not create project: {0}", e.getMessage());
              throw e;
          }
  
      }
  
      public static void main(String args[]) {
  
          final String USAGE = "\n" + "Usage: " + "<project_name> <bucket> <image>\n\n" + "Where:\n"
                  + "   project_name - A name for the new project\n\n";
  
          if (args.length != 1) {
              System.out.println(USAGE);
              System.exit(1);
          }
  
          String projectName = args[0];
          String projectArn = null;
          ;
  
          try {
  
              // Get the Rekognition client
              RekognitionClient rekClient = RekognitionClient.builder().build();
  
              // Create the project
              projectArn = createMyProject(rekClient, projectName);
  
              System.out.println(String.format("Created project: %s %nProject ARN: %s", projectName, projectArn));
  
              rekClient.close();
  
          } catch (RekognitionException rekError) {
              logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
              System.exit(1);
          }
  
      }
  
  }
  ```

------