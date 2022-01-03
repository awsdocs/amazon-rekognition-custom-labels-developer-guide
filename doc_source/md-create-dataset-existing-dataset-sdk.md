# Creating a dataset using an existing dataset \(SDK\)<a name="md-create-dataset-existing-dataset-sdk"></a>

The following procedure shows you how to create a dataset from an existing dataset by using the `CreateDataset` API\.
+ Use the following example code to create a dataset by copying another dataset\.

------
#### [ AWS CLI ]

  Use the following code to create the dataset\. Replace the following:
  + `project_arn` — the ARN of the project that you want to add the dataset to\.
  + `dataset_type` — with the type of dataset \(`TRAIN` or `TEST`\) that you want to create in the project\.
  + `dataset_arn` — with the ARN of the dataset that you want to copy\.

  ```
  aws rekognition create-dataset --project-arn project_arn \
    --dataset-type dataset_type\
    --dataset-source '{ "DatasetArn" : "dataset_arn" }'
  ```

------
#### [ Python ]

  The following example creates a dataset using an existing dataset and displays its ARN\.

  To run the program, supply the following command line arguments: 
  + `project_arn` — the ARN of the project that you want to use\. 
  + `dataset_type` — the type of the project dataset you want to create \(`train` or `test`\)\. 
  + `dataset_arn` — the ARN of the dataset that you want to create the dataset from\. 

  ```
  # Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
  # PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
  
  import boto3
  import argparse
  import logging
  import time
  import json
  from botocore.exceptions import ClientError
  
  logger = logging.getLogger(__name__)
  
  
  def create_dataset_from_existing_dataset(rek_client, project_arn, dataset_type, dataset_arn):
      """
      Creates an Amazon Rekognition Custom Labels dataset using an existing dataset.
      :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
      :param project_arn: The ARN of the project in which you want to create a dataset.
      :param dataset_type: The type of the dataset that you wan to create (train or test).
      :param dataset_arn: The ARN of the existing dataset that you want to use.
      """
  
      try:
          # Create the dataset
  
          dataset_type=dataset_type.upper()
  
          logger.info(
              f"Creating {dataset_type} dataset for project {project_arn} from dataset {dataset_arn}.")
  
          dataset_source = json.loads(
              '{ "DatasetArn": "' + dataset_arn + '"}'
          )
  
          response = rek_client.create_dataset(
              ProjectArn=project_arn, DatasetType=dataset_type, DatasetSource=dataset_source
          )
  
          dataset_arn = response['DatasetArn']
  
          logger.info(f"New dataset ARN: {dataset_arn}")
  
          finished = False
          while finished == False:
  
              dataset = rek_client.describe_dataset(DatasetArn=dataset_arn)
  
              status = dataset['DatasetDescription']['Status']
  
              if status == "CREATE_IN_PROGRESS":
  
                  logger.info((f"Creating dataset: {dataset_arn} "))
                  time.sleep(5)
                  continue
  
              if status == "CREATE_COMPLETE":
                  logger.info(f"Dataset created: {dataset_arn}")
                  finished = True
                  continue
  
              if status == "CREATE_FAILED":
                  logger.exception(
                      f"Dataset creation failed: {status} : {dataset_arn}")
                  raise Exception(
                      f"Dataset creation failed: {status} : {dataset_arn}")
  
              logger.exception(
                  f"Failed. Unexpected state for dataset creation: {status} : {dataset_arn}")
              raise Exception(
                  f"Failed. Unexpected state for dataset creation: {status} : {dataset_arn}")
  
          return dataset_arn
  
      except ClientError as err:
          logger.exception(
              f"Couldn't create dataset: {err.response['Error']['Message']}")
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
          "dataset_arn", help="The ARN of the dataset that you want to copy from."
      )
  
  
  def main():
  
      logging.basicConfig(level=logging.INFO,
                          format="%(levelname)s: %(message)s")
  
      try:
  
          # get command line arguments
          parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
          add_arguments(parser)
          args = parser.parse_args()
  
          print(
              f"Creating {args.dataset_type} dataset for project {args.project_arn}")
  
          # Create the dataset
          rek_client=boto3.client('rekognition')
  
          dataset_arn = create_dataset_from_existing_dataset(rek_client,
                                       args.project_arn,
                                       args.dataset_type,
                                       args.dataset_arn)
  
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

  The following example creates a dataset using an existing dataset and displays its ARN\.

  To run the program, supply the following command line arguments: 
  + `project_arn` — the ARN of the project that you want to use\. 
  + `dataset_type` — the type of the project dataset you want to create \(`train` or `test`\)\. 
  + `dataset_arn` — the ARN of the dataset that you want to create the dataset from\. 

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
  import software.amazon.awssdk.services.rekognition.model.RekognitionException;
  
  import java.net.URI;
  import java.util.logging.Level;
  import java.util.logging.Logger;
  
  public class CreateDatasetExisting {
  
      public static final Logger logger = Logger.getLogger(CreateDatasetExisting.class.getName());
  
      public static String createMyDataset(RekognitionClient rekClient, String projectArn, String datasetType,
              String existingDatasetArn) throws Exception, RekognitionException {
  
          try {
  
              logger.log(Level.INFO, "Creating {0} dataset for project : {1} from dataset {2} ",
                      new Object[] { datasetType.toString(), projectArn, existingDatasetArn });
  
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
  
              DatasetSource datasetSource = DatasetSource.builder().datasetArn(existingDatasetArn).build();
  
              CreateDatasetRequest createDatasetRequest = CreateDatasetRequest.builder().projectArn(projectArn)
                      .datasetType(requestDatasetType).datasetSource(datasetSource).build();
  
              CreateDatasetResponse response = rekClient.createDataset(createDatasetRequest);
  
              boolean created = false;
              
              //Wait until create finishes
  
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
          String datasetSourceArn = null;
  
          final String USAGE = "\n" + "Usage: " + "<project_arn> <dataset_type> <dataset_arn>\n\n" + "Where:\n"
                  + "   project_arn - the ARN of the project that you want to add copy the datast to.\n\n"
                  + "   dataset_type - the type of the dataset that you want to create (train or test).\n\n"
                  + "   dataset_arn - the ARN of the dataset that you want to copy from.\n\n";
  
          if (args.length != 3) {
              System.out.println(USAGE);
              System.exit(1);
          }
  
          projectArn = args[0];
          datasetType = args[1];
          datasetSourceArn = args[2];
  
          try {
  
              // Get the Rekognition client
              RekognitionClient rekClient = RekognitionClient.builder().build();
  
              // Create the dataset
              datasetArn = createMyDataset(rekClient, projectArn, datasetType, datasetSourceArn);
  
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