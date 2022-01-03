# Describing a project \(SDK\)<a name="md-describing-project-sdk"></a>

You can use the `DescribeProjects` API to get information about your projects\.

**To describe a project \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to describe a project\. Replace `project_name` with the name of the project that you want to describe\. If you don't specify `--project-names`, desriptions for all projects are returned\.

------
#### [ AWS CLI ]

   ```
   aws rekognition describe-projects --project-names "project_name"
   ```

------
#### [ Python ]

   Use the following code\. Supply the following command line parameters:
   + project\_name``— the name of the project that you want to describe\. If you don't specify a name, descriptions for all projects are returned\.

   ```
   # Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   # PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   import json
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   
   def display_project_info(project):
       """
       Displays information about a Custom Labels project.
       :param project: The project that you want to display information about.
       """
       print(f"Arn: {project['ProjectArn']}")
       print(f"Status: {project['Status']}")
   
       if len(project['Datasets']) == 0:
           print("Datasets: None")
       else:
           print("Datasets:")
   
       for dataset in project['Datasets']:
           print(f"\tCreated: {str(dataset['CreationTimestamp'])}")
           print(f"\tType: {dataset['DatasetType']}")
           print(f"\tARN: {dataset['DatasetArn']}")
           print(f"\tStatus: {dataset['Status']}")
           print(f"\tStatus message: {dataset['StatusMessage']}")
           print(f"\tStatus code: {dataset['StatusMessageCode']}")
           print()
       print()
   
   
   def describe_projects(rek_client, project_name):
       """
       Describes an Amazon Rekognition Custom Labels project, or all projects.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_name: The project you want to describe. Pass None to describe all projects.
       """
   
       try:
           # Describe the project
           if project_name == None:
               logger.info(f"Describing all projects.")
           else:
               logger.info(f"Describing project: {project_name}.")
   
           if project_name == None:
               response = rek_client.describe_projects()
           else:
               project_names = json.loads('["' + project_name + '"]')
               response = rek_client.describe_projects(ProjectNames=project_names)
   
           print('Projects\n--------')
           if len(response['ProjectDescriptions']) == 0:
               print("Project(s) not found.")
           else:
               for project in response['ProjectDescriptions']:
                   display_project_info(project)
   
           logger.info(f"Finished project description.")
   
       except ClientError as err:
           logger.exception(
               f"Couldn't describe project - {project_name}: {err.response['Error']['Message']}")
           raise
   
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "--project_name",  help="The name of the project that you want to describe.", required=False
       )
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO,
                           format="%(levelname)s: %(message)s")
   
       try:
   
           # get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
   
           args = parser.parse_args()
   
           print(f"Describing projects: {args.project_name}")
   
           # Describe the project
           rek_client = boto3.client('rekognition')
   
           describe_projects(rek_client,
                             args.project_name)
   
           if args.project_name == None:
               print(f"Finished describing all projects.")
           else:
               print(f"Finished describing project {args.project_name}.")
   
       except ClientError as err:
           logger.exception(f"Problem describing project: {err}")
           print(f"Problem describing project: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java V2 ]

   Use the following code\. Supply the following command line parameters:
   + `project_name` — the ARN of the project that you want to describe\. If you don't specify a name, descriptions for all projects are returned\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import java.net.URI;
   import java.util.ArrayList;
   import java.util.List;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DatasetMetadata;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectsRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectsResponse;
   import software.amazon.awssdk.services.rekognition.model.ProjectDescription;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   public class DescribeProjects {
   
       public static final Logger logger = Logger.getLogger(DescribeProjects.class.getName());
   
       public static void describeMyProjects(RekognitionClient rekClient, String projectName) {
   
           DescribeProjectsRequest descProjects = null;
   
           // If a single project name is supplied, build projectNames argument
   
           List<String> projectNames = new ArrayList<String>();
   
           if (projectName == null) {
               descProjects = DescribeProjectsRequest.builder().build();
           } else {
               projectNames.add(projectName);
               descProjects = DescribeProjectsRequest.builder().projectNames(projectNames).build();
           }
   
           // Display useful information for each project.
   
           DescribeProjectsResponse resp = rekClient.describeProjects(descProjects);
   
           for (ProjectDescription projectDescription : resp.projectDescriptions()) {
   
               System.out.println("ARN: " + projectDescription.projectArn());
               System.out.println("Status: " + projectDescription.statusAsString());
               if (projectDescription.hasDatasets()) {
                   for (DatasetMetadata datasetDescription : projectDescription.datasets()) {
                       System.out.println("\tdataset Type: " + datasetDescription.datasetTypeAsString());
                       System.out.println("\tdataset ARN: " + datasetDescription.datasetArn());
                       System.out.println("\tdataset Status: " + datasetDescription.statusAsString());
                   }
               }
               System.out.println();
           }
   
       }
   
       public static void main(String[] args) {
   
           String projectArn = null;
   
           // Get command line arguments
   
           final String USAGE = "\n" + "Usage: " + "<project_name>\n\n" + "Where:\n"
                   + "   project_arn - (Optional) The name of the project that you want to describe. If not specified, all projects "
                   + "are described.\n\n";
   
           if (args.length > 1) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           if (args.length == 1) {
               projectArn = args[0];
           }
   
           try {
   
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
               
               // Describe projects
   
               describeMyProjects(rekClient, projectArn);
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------