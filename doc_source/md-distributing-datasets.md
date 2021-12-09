# Distributing a training dataset \(SDK\)<a name="md-distributing-datasets"></a>

Amazon Rekognition Custom Labels requires a training dataset and a test dataset to train your model\. 

If you are using the API, you can use the `DistributeDatsetEntries` API to distribute 20% of the training dataset into an empty test dataset\. Distributing the training dataset can be useful if you only have a single manifest file available\. Use the single manifest file to create your training dataset\. Then create an empty test dataset and use `DistributeDatasetEntries` to populate the test dataset

**Note**  
If you are using the Amazon Rekognition Custom Labels console and start with a single dataset project, Amazon Rekognition Custom Labels splits \(distributes\) the training dataset, during training, to create a test dataset\. 20% of the training dataset entries are moved to the test dataset\.

**To distribute a training dataset \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Create a project\. For more information, see [ Creating an Amazon Rekognition Custom Labels project \(SDK\)Creating a project \(SDK\)  You create an Amazon Rekognition Custom Labels project by calling [CreateProject](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProject)\. The response is an Amazon Resource Name \(ARN\) that identifies the project\. After you create a project, you create datasets for training and testing a model\. For more information, see [Creating training and test datasets \(Console\)](md-create-dataset.md)\.  To create a project \(SDK\) If you haven't already:  Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.   Use the following code to create a project\.    AWS CLI  The following example creates a project and displays its ARN\. Change the value of `project-name` to the name of the project that you want to create\. 

   ```
   aws rekognition create-project --project-name my_project
   ```     Python  The following example creates a project and displays its ARN\. Supply the following command line arguments:  `project_name` – the name of the project you want to create\.  

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def create_project(rek_client, project_name):
       """
       Creates an Amazon Rekognition Custom Labels project
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_name: A name for the new prooject.
       """
   
       try:
           #Create the project
           logger.info(f"Creating project: {project_name}")
           
           response=rek_client.create_project(ProjectName=project_name)
           
           logger.info(f"project ARN: {response['ProjectArn']}")
   
           return response['ProjectArn']
      
       
       except ClientError as err:  
           logger.exception(f"Couldn't create project - {project_name}: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_name", help="A name for the new project."
       )
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Creating project: {args.project_name}")
   
           #Create the project
           rek_client=boto3.client('rekognition')
   
           project_arn=create_project(rek_client, 
               args.project_name)
   
           print(f"Finished creating project: {args.project_name}")
           print(f"ARN: {project_arn}")
   
       except ClientError as err:
           logger.exception(f"Problem creating model: {err}")
           print(f"Problem creating model: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```    Java V2  The following example creates a project and displays its ARN\. Supply the following command line argument:  `project_name` – the name of the project you want to create\.  

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.CreateProjectRequest;
   import software.amazon.awssdk.services.rekognition.model.CreateProjectResponse;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class CreateProject {
   
       public static final Logger logger = Logger.getLogger(CreateProject.class.getName());
   
       public static String createMyProject(RekognitionClient rekClient, String projectName) {
   
           try {
   
               logger.log(Level.INFO, "Creating project: {0}", projectName);
               CreateProjectRequest createProjectRequest = CreateProjectRequest.builder().projectName(projectName).build();
   
               CreateProjectResponse response = rekClient.createProject(createProjectRequest);
   
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
   ```     Note the name of the project ARN that's displayed in the response\. You'll need it to create a model\.  Follow the steps in [Create training and test datasets \(SDK\) ](cd-create-dataset-sdk.md) to create the training and test datasets for your project\.  ](mp-create-project.md#mp-create-project-sdk)\.

1. Create your training dataset\.

1. Create an empty test datset

1. Use the following example code to distribute 20% of the training dataset entries into the test dataset\. 

------
#### [ AWS CLI ]

   Change the value of `training_dataset-arn` and `test_dataset_arn` with the ARNS of the datasets that you want to use\.

   ```
   aws rekognition distribute-dataset-entries --datasets ['{"Arn": "training_dataset_arn"}, {"Arn": "test_dataset_arn"}']
   ```

------
#### [ Python ]

   Use the following code\. Supply the following command line parameters:
   + training\_dataset\_arn — the ARN of the training dataset that you distribute entries from\.
   + test\_dataset\_arn — the ARN of the test dataset that you distribute entries to\.

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
   
   
   def check_dataset_status(rek_client, dataset_arn):
       finished = False
       status = ""
       status_message = ""
   
       while finished == False:
   
           dataset = rek_client.describe_dataset(DatasetArn=dataset_arn)
   
           status = dataset['DatasetDescription']['Status']
           status_message = dataset['DatasetDescription']['StatusMessage']
   
           if status == "UPDATE_IN_PROGRESS":
   
               logger.info((f"Distributing dataset: {dataset_arn} "))
               time.sleep(5)
               continue
   
           if status == "UPDATE_COMPLETE":
               logger.info(
                   f"Dataset distribution complete: {status} : {status_message} : {dataset_arn}")
               finished = True
               continue
   
           if status == "UPDATE_FAILED":
               logger.exception(
                   f"Dataset distribution failed: {status} : {status_message} : {dataset_arn}")
               finished = True
               break
   
           logger.exception(
               f"Failed. Unexpected state for dataset distribution: {status} : {status_message} : {dataset_arn}")
           finished = True
           status_message = "An unexpected error occurred while distributing the dataset"
           break
   
       return status, status_message
   
   
   def distribute_dataset_entries(rek_client, training_dataset_arn, test_dataset_arn):
       """
       Distributes 20% of the supplied training dataset into the supplied test dataset.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param training_dataset_arn: The ARN of the training dataset that you distribute entries from.
       :param test_dataset_arn: The ARN of the test dataset that you distribute entries to.
       """
   
       try:
           # List dataset labels
           logger.info(f"Distributing training dataset entries ({training_dataset_arn}) into test dataset ({test_dataset_arn})."
                       )
   
           datasets = json.loads(
               '[{"Arn" : "' + str(training_dataset_arn) + '"},{"Arn" : "' + str(test_dataset_arn) + '"}]')
   
           rek_client.distribute_dataset_entries(
               Datasets=datasets
           )
   
           training_dataset_status, training_dataset_status_message = check_dataset_status(
               rek_client, training_dataset_arn)
           test_dataset_status, test_dataset_status_message = check_dataset_status(
               rek_client, test_dataset_arn)
   
           if training_dataset_status == 'UPDATE_COMPLETE' and test_dataset_status == "UPDATE_COMPLETE":
               print(f"Distribution complete")
           else:
               print("Distribution failed:")
               print(
                   f"\ttraining dataset: {training_dataset_status} : {training_dataset_status_message}")
               print(
                   f"\ttest dataset: {test_dataset_status} : {test_dataset_status_message}")
   
       except ClientError as err:
           logger.exception(
               f"Couldn't distribute dataset: {err.response['Error']['Message']}")
           raise
   
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "training_dataset_arn", help="The ARN of the training dataset that you want to distribute from."
       )
   
       parser.add_argument(
           "test_dataset_arn", help="The ARN of the test dataset that you want to distribute to."
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
               f"Distributing training dataset entries ({args.training_dataset_arn}) into test dataset ({args.test_dataset_arn}).")
   
           # Distribute the datasets
   
           rek_client = boto3.client('rekognition')
   
           distribute_dataset_entries(rek_client,
                                      args.training_dataset_arn,
                                      args.test_dataset_arn)
   
           print(f"Finished distributing datasets.")
   
       except ClientError as err:
           logger.exception(f"Problem distributing datasets: {err}")
           print(f"Problem listing dataset labels: {err}")
       except Exception as err:
           logger.exception(f"Problem distributing datasets: {err}")
           print(f"Problem distributing datasets: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]

   Use the following code\. Supply the following command line parameters:
   + training\_dataset\_arn — the ARN of the training dataset that you distribute entries from\.
   + test\_dataset\_arn — the ARN of the test dataset that you distribute entries to\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DatasetDescription;
   import software.amazon.awssdk.services.rekognition.model.DatasetStatus;
   import software.amazon.awssdk.services.rekognition.model.DatasetType;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeDatasetResponse;
   import software.amazon.awssdk.services.rekognition.model.DistributeDataset;
   import software.amazon.awssdk.services.rekognition.model.DistributeDatasetEntriesRequest;
   import software.amazon.awssdk.services.rekognition.model.DistributeDatasetEntriesResponse;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   
   import java.net.URI;
   import java.util.ArrayList;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class DistributeDatasetEntries {
   
       public static final Logger logger = Logger.getLogger(DistributeDatasetEntries.class.getName());
       
       
       
       public static DatasetStatus checkDatasetStatus(RekognitionClient rekClient, String datasetArn)
           throws Exception, RekognitionException {
   
   
           boolean distributed = false;
           DatasetStatus status = null;
              
           //Wait until distribution completes
   
           do {
   
               DescribeDatasetRequest describeDatasetRequest = DescribeDatasetRequest.builder()
                       .datasetArn(datasetArn).build();
               DescribeDatasetResponse describeDatasetResponse = rekClient.describeDataset(describeDatasetRequest);
   
               DatasetDescription datasetDescription = describeDatasetResponse.datasetDescription();
   
               status = datasetDescription.status();
   
               logger.log(Level.INFO, " dataset ARN: {0} ", datasetArn);
   
               switch (status) {
   
               case UPDATE_COMPLETE:
                   logger.log(Level.INFO, "Dataset updated");
                   distributed = true;
                   break;
   
               case UPDATE_IN_PROGRESS:
                   Thread.sleep(5000);
                   break;
   
               case UPDATE_FAILED:
                   String error = "Dataset distribution failed: " + datasetDescription.statusAsString() + " "
                           + datasetDescription.statusMessage() + " " + datasetArn;
                   logger.log(Level.SEVERE, error);
                   break;
   
   
               default:
                   String unexpectedError = "Unexpected distribution state: " + datasetDescription.statusAsString() + " "
                           + datasetDescription.statusMessage() + " " + datasetArn;
                   logger.log(Level.SEVERE, unexpectedError);
   
              
               }
                   
           } while (distributed == false);
           
           return status;
           
           
       
       }
   
       public static void distributeMyDatasetEntries(RekognitionClient rekClient, String trainingDatasetArn, String testDatasetArn)
               throws Exception, RekognitionException {
   
           try {
   
               logger.log(Level.INFO, "Distributing {0} dataset to {1} ",
                       new Object[] { trainingDatasetArn, testDatasetArn});
   
   
               
               DistributeDataset distributeTrainingDataset = DistributeDataset.builder()
                       .arn(trainingDatasetArn)
                       .build();
               
               DistributeDataset distributeTestDataset = DistributeDataset.builder()
                       .arn(testDatasetArn)
                       .build();
               
               ArrayList <DistributeDataset> datasets = new ArrayList();
               
               datasets.add(distributeTrainingDataset);
               datasets.add(distributeTestDataset);
               
               
               DistributeDatasetEntriesRequest distributeDatasetEntriesRequest = DistributeDatasetEntriesRequest.builder()
                       .datasets(datasets)
                       .build();
                       
               
               rekClient.distributeDatasetEntries(distributeDatasetEntriesRequest);
      
               
               DatasetStatus trainingStatus= checkDatasetStatus(rekClient, trainingDatasetArn);
               DatasetStatus testStatus= checkDatasetStatus(rekClient, testDatasetArn);
               
               if (trainingStatus ==DatasetStatus.CREATE_COMPLETE && testStatus == DatasetStatus.CREATE_COMPLETE) {
                   logger.log(Level.INFO, "Successfully distributed dataset: {0}");
                   
               }
               else {
                   
                   throw new Exception("Failed to distribute dataset: " + trainingDatasetArn);
               }
   
           } catch (RekognitionException e) {
               logger.log(Level.SEVERE, "Could not distribute dataset: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
     
           String trainingDatasetArn = null;
           String testDatasetArn = null;
   
   
           final String USAGE = "\n" + "Usage: " + "<training_dataset_arn> <test_dataset_arn>\n\n" + "Where:\n"
                   + "   training_dataset_arn - the ARN of the dataset that you want to distribute from.\n\n"
                   + "   test_dataset_arn - the ARN of the dataset that you want to distribute to.\n\n";
   
           if (args.length != 2) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           trainingDatasetArn = args[0];
           testDatasetArn = args[1];
      
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
                // Distribute the dataset
               distributeMyDatasetEntries(rekClient, trainingDatasetArn, testDatasetArn);
   
               System.out.println("Datasets distributed.");
   
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