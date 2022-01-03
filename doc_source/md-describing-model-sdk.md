# Describing a model \(SDK\)<a name="md-describing-model-sdk"></a>

You can use the `DescribeProjectVersions` API to get information about a version of a model\. If you don't specify `VersionName`, `DescribeProjectVersions` returns descriptions for all model versions in the project\.

**To describe a model \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to describe a version of a model\.

------
#### [ AWS CLI ]

   Change the value of `project-arn` to the ARN of the project that you want to describe\. Change the value of `version-name` to the version of the model that you want to describe\.

   ```
   aws rekognition describe-project-versions --project-arn project_arn \
   --version-names version_name
   ```

------
#### [ Python ]

   Use the following code\. Supply the following command line parameters:
   + project\_arn — the ARN of the model that you want to describe\. 
   + model\_version — the version of the model that you want to describe\. 

   For example: `python describe_model.py project_arn model_version `

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def describe_model(rek_client, project_arn, version_name):
       """
       Describes an Amazon Rekognition Custom Labels model.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param dataset_arn: The ARN of the modelthat you want to describe.
       """
   
       try:
           #Describe the model
           logger.info(f"Describing model: {version_name} for project {project_arn}")
   
           describe_response=rek_client.describe_project_versions(ProjectArn=project_arn,
               VersionNames=[version_name])
           for model in describe_response['ProjectVersionDescriptions']:
               print(f"Created: {str(model['CreationTimestamp'])} ")
               print(f"ARN: {str(model['ProjectVersionArn'])} ")
               if 'BillableTrainingTimeInSeconds' in model:
                   print(f"Billing training time (minutes): {str(model['BillableTrainingTimeInSeconds']/60)} ")
               print("Evaluation results: ")
               if 'EvaluationResult' in model:
                   evaluation_results = model['EvaluationResult']
                   print(f"\tF1 score: {str(evaluation_results['F1Score'])}")
                   print(f"\tSummary location: s3://{evaluation_results['Summary']['S3Object']['Bucket']}/{evaluation_results['Summary']['S3Object']['Name']}")
               
               if 'ManifestSummary' in model:
                   print(f"Manifest summary location: s3://{model['ManifestSummary']['S3Object']['Bucket']}/{model['ManifestSummary']['S3Object']['Name']}")
               if 'OutputConfig' in model:
                   print(f"Training output location: s3://{model['OutputConfig']['S3Bucket']}/{model['OutputConfig']['S3KeyPrefix']}")
               if 'MinInferenceUnits' in model:
                   print(f"Inference units: {str(model['MinInferenceUnits'])}")
               print("Status: " + model['Status'])
               print("Message: " + model['StatusMessage']) 
   
   
       
       except ClientError as err:  
           logger.exception(f"Couldn't describe model: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_arn", help="The ARN of the project in which the model resides."
       )
       parser.add_argument(
           "version_name", help="The version of the model that you want to describe."
       )
   
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Describing model: {args.version_name} for project {args.project_arn}.")
   
           #Describe the model
           rek_client=boto3.client('rekognition')
   
           describe_model(rek_client, args.project_arn,
               args.version_name
           )
   
           print(f"Finished describing model: {args.version_name} for project {args.project_arn}.")
   
   
       except ClientError as err:
           logger.exception(f"Problem describing model: {err}")
           print(f"Problem describing model: {err}")
       except Exception as err:
           logger.exception(f"Problem describing model: {err}")
           print(f"Problem describing model: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]

   Use the following code\. Supply the following command line parameters:
   + project\_arn — the ARN of the model that you want to describe\. 
   + model\_version — the version of the model that you want to describe\. 

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsResponse;
   import software.amazon.awssdk.services.rekognition.model.EvaluationResult;
   import software.amazon.awssdk.services.rekognition.model.GroundTruthManifest;
   import software.amazon.awssdk.services.rekognition.model.OutputConfig;
   import software.amazon.awssdk.services.rekognition.model.ProjectVersionDescription;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class DescribeModel {
   
       public static final Logger logger = Logger.getLogger(DescribeModel.class.getName());
   
       public static void describeMyModel(RekognitionClient rekClient, String projectArn, String versionName) {
   
           try {
   
               // If a single version name is supplied, build request argument
   
               DescribeProjectVersionsRequest describeProjectVersionsRequest = null;
   
               if (versionName == null) {
                   describeProjectVersionsRequest = DescribeProjectVersionsRequest.builder().projectArn(projectArn)
                           .build();
               } else {
                   describeProjectVersionsRequest = DescribeProjectVersionsRequest.builder().projectArn(projectArn)
                           .versionNames(versionName).build();
               }
   
               DescribeProjectVersionsResponse describeProjectVersionsResponse = rekClient
                       .describeProjectVersions(describeProjectVersionsRequest);
   
               for (ProjectVersionDescription projectVersionDescription : describeProjectVersionsResponse
                       .projectVersionDescriptions()) {
   
                   System.out.println("ARN: " + projectVersionDescription.projectVersionArn());
                   System.out.println("Status: " + projectVersionDescription.statusAsString());
                   System.out.println("Message: " + projectVersionDescription.statusMessage());
   
                   if (projectVersionDescription.billableTrainingTimeInSeconds() != null) {
                       System.out.println(
                               "Billable minutes: " + (projectVersionDescription.billableTrainingTimeInSeconds() / 60));
                   }
   
                   if (projectVersionDescription.evaluationResult() != null) {
                       EvaluationResult evaluationResult = projectVersionDescription.evaluationResult();
   
                       System.out.println("F1 Score: " + evaluationResult.f1Score());
                       System.out.println("Summary location: s3://" + evaluationResult.summary().s3Object().bucket() + "/"
                               + evaluationResult.summary().s3Object().name());
                   }
   
                   if (projectVersionDescription.manifestSummary() != null) {
                       GroundTruthManifest manifestSummary = projectVersionDescription.manifestSummary();
                       System.out.println("Manifest summary location: s3://" + manifestSummary.s3Object().bucket() + "/"
                               + manifestSummary.s3Object().name());
   
                   }
   
                   if (projectVersionDescription.outputConfig() != null) {
                       OutputConfig outputConfig = projectVersionDescription.outputConfig();
                       System.out.println(
                               "Training output: s3://" + outputConfig.s3Bucket() + "/" + outputConfig.s3KeyPrefix());
                   }
   
                   if (projectVersionDescription.minInferenceUnits() != null) {
                       System.out.println("Min inference units: " + projectVersionDescription.minInferenceUnits());
                   }
   
                   System.out.println();
   
               }
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               throw rekError;
           }
   
       }
   
       public static void main(String args[]) {
   
           String projectArn = null;
           String versionName = null;
   
           final String USAGE = "\n" + "Usage: " + "<project_arn> <version_name>\n\n" + "Where:\n"
                   + "   project_arn - The ARN of the project that contains the models you want to describe.\n\n"
                   + "   version_name - (optional) The version name of the model that you want to describe. \n\n"
                   + "                  If you don't specify a value, all model versions are described.\n\n";
   
           if (args.length > 2 || args.length == 0) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           projectArn = args[0];
   
           if (args.length == 2) {
               versionName = args[1];
           }
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
                // Describe the model
               describeMyModel(rekClient, projectArn, versionName);
   
               rekClient.close();
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------