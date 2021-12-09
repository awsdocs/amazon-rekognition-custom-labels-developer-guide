# Stopping an Amazon Rekognition Custom Labels model<a name="rm-stop"></a>

You can stop running an Amazon Rekognition Custom Labels model by using the console or by using the [StopProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StopProjectVersion) operation\.

**Topics**
+ [Stopping an Amazon Rekognition Custom Labels model \(Console\)](#rm-stop-console)
+ [Stopping an Amazon Rekognition Custom Labels model \(SDK\)](#rm-stop-sdk)

## Stopping an Amazon Rekognition Custom Labels model \(Console\)<a name="rm-stop-console"></a>

Use the following procedure to stop a running Amazon Rekognition Custom Labels model with the console\. You can stop the model directly from the console or use the AWS SDK code provided by the console\. 

**To stop a model \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. On the **Projects** resources page, choose the project that contains the trained model that you want to stop\.

1. In the **Models** section, choose the model that you want to stop\. 

1. Choose the **Use model** tab\.

1. 

------
#### [ Stop model using the console ]

   1. In the **Start or stop model** section choose **Stop**\.

   1. In the **Stop model** dialog box, enter **stop** to confirm that you want to stop the model\.

   1. Choose **Stop** to stop your model\.

------
#### [ Stop model using the AWS SDK ]

   In the **Use your model** section do the following:

   1. Choose **API Code\.**

   1. Choose either **AWS CLI** or **Python**\.

   1. In **Stop model** copy the example code\.

   1. Use the example code to stop your model\. For more information, see [Stopping an Amazon Rekognition Custom Labels model \(SDK\)](#rm-stop-sdk)\.

------

1. Choose your project name at the top of the page to go back to the project overview page\.

1. In the **Model** section, check the status of the model\. The model has stopped when the model status is **STOPPED**\.

## Stopping an Amazon Rekognition Custom Labels model \(SDK\)<a name="rm-stop-sdk"></a>

You stop a model by calling the [StopProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StopProjectVersion) API and passing the Amazon Resource Name \(ARN\) of the model in the `ProjectVersionArn` input parameter\. 

A model might take a while to stop\. To check the current status, use `DescribeProjectVersions`\. 

**To stop a model \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to stop a running model\.

------
#### [ CLI ]

   Change the value of `project-version-arn` to the ARN of the model version that you want to stop\.

   ```
   aws rekognition stop-project-version --project-version-arn "my model version"
   ```

------
#### [ Python ]

   The following example stops a model that is already running\.

   Supply the following command line parameters:
   + `project_arn` – the ARN of the project that contains the model that you want to stop\.
   + `model_arn` – the ARN of the model that you want to stop\.

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
   import time
   
   from botocore.exceptions import ClientError
   
   
   logger = logging.getLogger(__name__)
   
   
   def get_model_status(rek_client, project_arn, model_arn):
       """
       Gets the current status of an Amazon Rekognition Custom Labels model
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_name:  The name of the project that you want to use.
       :param model_arn:  The name of the model that you want the status for.
       """
   
       logger.info (f"Getting status for {model_arn}.")
   
       # extract the model version from the model arn.
       version_name=(model_arn.split("version/",1)[1]).rpartition('/')[0]
   
       # get the model status
       models=rek_client.describe_project_versions(ProjectArn=project_arn,
       VersionNames=[version_name])
   
       for model in models['ProjectVersionDescriptions']: 
      
           logger.info(f"Status: {model['StatusMessage']}")
           return model["Status"]
   
       # no model found    
       logger.exception(f"Model {model_arn} not found.")
       raise Exception(f"Model {model_arn} not found.")
   
   
   
   def stop_model(rek_client, project_arn, model_arn):
       """
       Stops a running Amazon Rekognition Custom Labels Model.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_arn: The ARN of the project that you want to stop running.
       :param model_arn:  The ARN of the model (ProjectVersion) that you want to stop running.
       """
   
       logger.info(f"Stopping model: {model_arn}")
   
       try:
           #Stop the model
           response=rek_client.stop_project_version(ProjectVersionArn=model_arn)
           
           logger.info(f"Status: {response['Status']}")
   
           # stops when hosting has stopped or failure.
           status = ""
           finished = False
   
           while finished is False:
   
               status=get_model_status(rek_client, project_arn, model_arn)
   
               if status == "STOPPING":
                       logger.info("Model stopping in progress...")
                       time.sleep(10)
                       continue
               if status == "STOPPED":
                       logger.info("Model is not running.")
                       finished = True
                       continue
               
               logger.exception(f"Error stopping model. Unexepected state: {status}")
               raise Exception(f"Error stopping model. Unexepected state: {status}")
   
       
           logger.info(f"finished. Status {status}")
           return status
   
   
       except ClientError as e:  
           logger.exception(f"Couldn't stop model - {model_arn}: {e.response['Error']['Message']}")
           raise
   
   
       
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_arn", help="The ARN of the project that contains the model that you want to stop."
       )
       parser.add_argument(
           "model_arn", help="The ARN of the model that you want to stop."
       )
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           #stop the model
           rek_client=boto3.client('rekognition')
           status=stop_model(rek_client, args.project_arn, args.model_arn)
   
           print(f"Finished stopping model: {args.model_arn}")
           print(f"Status: {status}")
   
       except ClientError as err:
           logger.exception(f"Problem stopping model:{err}")
           print(f"Failed to stop model: {err}")
       
       except Exception as err:
           logger.exception(f"Problem stopping model:{err}")
           print(f"Failed to stop model: {err}")
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]

   Supply the following command line parameters:
   + `project_arn` – the ARN of the project that contains the model that you want to stop\.
   + `model_arn` – the ARN of the model that you want to stop\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsResponse;
   import software.amazon.awssdk.services.rekognition.model.ProjectVersionDescription;
   import software.amazon.awssdk.services.rekognition.model.ProjectVersionStatus;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   import software.amazon.awssdk.services.rekognition.model.StopProjectVersionRequest;
   import software.amazon.awssdk.services.rekognition.model.StopProjectVersionResponse;
   
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class StopModel {
   
       public static final Logger logger = Logger.getLogger(StopModel.class.getName());
   
       public static int findForwardSlash(String modelArn, int n) {
   
           int start = modelArn.indexOf('/');
           while (start >= 0 && n > 1) {
               start = modelArn.indexOf('/', start + 1);
               n -= 1;
           }
           return start;
   
       }
   
       public static void stopMyModel(RekognitionClient rekClient, String projectArn, String modelArn)
               throws Exception, RekognitionException {
   
           try {
   
               logger.log(Level.INFO, "Stopping {0}", modelArn);
   
               StopProjectVersionRequest stopProjectVersionRequest = StopProjectVersionRequest.builder()
                       .projectVersionArn(modelArn).build();
   
               StopProjectVersionResponse response = rekClient.stopProjectVersion(stopProjectVersionRequest);
   
               logger.log(Level.INFO, "Status: {0}", response.statusAsString());
   
               // Get the model version
   
               int start = findForwardSlash(modelArn, 3) + 1;
               int end = findForwardSlash(modelArn, 4);
   
               String versionName = modelArn.substring(start, end);
   
               // wait until model stops
   
               DescribeProjectVersionsRequest describeProjectVersionsRequest = DescribeProjectVersionsRequest.builder()
                       .projectArn(projectArn).versionNames(versionName).build();
   
               boolean stopped = false;
   
               // Wait until create finishes
   
               do {
   
                   DescribeProjectVersionsResponse describeProjectVersionsResponse = rekClient
                           .describeProjectVersions(describeProjectVersionsRequest);
   
                   for (ProjectVersionDescription projectVersionDescription : describeProjectVersionsResponse
                           .projectVersionDescriptions()) {
   
                       ProjectVersionStatus status = projectVersionDescription.status();
   
                       logger.log(Level.INFO, "stopping model: {0} ", modelArn);
   
                       switch (status) {
   
                       case STOPPED:
                           logger.log(Level.INFO, "Model stopped");
                           stopped = true;
                           break;
   
                       case STOPPING:
                           Thread.sleep(5000);
                           break;
   
                       case FAILED:
                           String error = "Model stopping failed: " + projectVersionDescription.statusAsString() + " "
                                   + projectVersionDescription.statusMessage() + " " + modelArn;
                           logger.log(Level.SEVERE, error);
                           throw new Exception(error);
   
                       default:
                           String unexpectedError = "Unexpected stopping state: "
                                   + projectVersionDescription.statusAsString() + " "
                                   + projectVersionDescription.statusMessage() + " " + modelArn;
                           logger.log(Level.SEVERE, unexpectedError);
                           throw new Exception(unexpectedError);
                       }
                   }
   
               } while (stopped == false);
   
           } catch (RekognitionException e) {
               logger.log(Level.SEVERE, "Could not stop model: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
           String modelArn = null;
           String projectArn = null;
   
   
           final String USAGE = "\n" + "Usage: " + "<project_name> <version_name>\n\n" + "Where:\n"
                   + "   project_arn - The ARN of the project that contains the model that you want to stop. \n\n"
                   + "   model_arn - The ARN of the model version that you want to stop.\n\n";
   
           if (args.length != 2) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           projectArn = args[0];
           modelArn = args[1];
   
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
               // Stop model
               stopMyModel(rekClient, projectArn, modelArn);
   
               System.out.println(String.format("Model stopped: %s", modelArn));
   
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