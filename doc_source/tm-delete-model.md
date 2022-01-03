# Deleting an Amazon Rekognition Custom Labels model<a name="tm-delete-model"></a>

You can delete a model by using the Amazon Rekognition Custom Labels console or by using the [DeleteProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_DeleteProjectVersion) API\. You can't delete a model if it is running or if it is training\. To stop a running model, use the [StopProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StopProjectVersion) API\. For more information, see [Stopping an Amazon Rekognition Custom Labels model \(SDK\)](rm-stop.md#rm-stop-sdk)\. If a model is training, wait until it finishes before you delete the model\.

A deleted model can't be undeleted\.

**Topics**
+ [Deleting an Amazon Rekognition Custom Labels model \(Console\)](#tm-delete-model-console)
+ [Deleting an Amazon Rekognition Custom Labels model \(SDK\)](#tm-delete-model-sdk)

## Deleting an Amazon Rekognition Custom Labels model \(Console\)<a name="tm-delete-model-console"></a>

The following procedure shows how to delete a model\(s\) from a project details page\. You can also delete a model from a model's detail page\.  

**To delete a model \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. Choose the project that contains the model that you want to delete\. The project details page opens\.

1. In the **Models** section, select the model\(s\) that you want to delete\.
**Note**  
If the model can't be selected, the model is either running or is training, and can't be deleted\. Check the **Status** field and try again after stopping the running model, or wait until training finishes\. 

1. Choose **Delete model** and the **Delete model dialog box is shown\.**

1. Enter **delete** to confirm deletion\. 

1. Choose **Delete** to delete the model\. Deleting the model might take a while to complete\.
**Note**  
If you **Close** the dialog box during model deletion, the models are still deleted\.

## Deleting an Amazon Rekognition Custom Labels model \(SDK\)<a name="tm-delete-model-sdk"></a>

You delete an Amazon Rekognition Custom Labels model by calling [DeleteProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_DeleteProjectVersion) and supplying the Amazon Resource Name \(ARN\) of the model that you want to delete\. You can get the model ARN from the **Use your model** section of the model details page in the Amazon Rekognition Custom Labels console\. Alternatively, call [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions) and supply the following\.
+ The ARN of the project \(`ProjectArn`\) that the model is associated with\.
+ The version name \(`VersionNames`\) of the model\. 

The model ARN is the `ProjectVersionArn` field in the [ProjectVersionDescription](https://docs.aws.amazon.com/rekognition/latest/dg/API_ProjectVersionDescription) object, from the `DescribeProjectVersions` response\.

You can't delete a model if it is running or is training\. To determine if the model is running or training, call [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions) and check the `Status` field of the model's [ProjectVersionDescription](https://docs.aws.amazon.com/rekognition/latest/dg/API_ProjectVersionDescription) object\. To stop a running model, use the [StopProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StopProjectVersion) API\. For more information, see [Stopping an Amazon Rekognition Custom Labels model \(SDK\)](rm-stop.md#rm-stop-sdk)\. You have to wait for a model to finishing training before you can delete it\. 

**To delete a model \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following code to delete a model\. 

------
#### [ AWS CLI ]

   Change the value of `project-version-arn` to the name of the project you want to delete\.

   ```
   aws rekognition delete-project-version --project-version-arn model_arn 
   ```

------
#### [ Python ]

   Supply the dollowing command line paramaters
   + `project_arn` – the ARN of the project that contains the model that you want to delete\.
   + `model_arn` – the ARN of the model version that you want to delete\.

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   """
   Purpose
   Amazon Rekognition Custom Labels model example used in the service documentation:
   https://docs.aws.amazon.com/rekognition/latest/customlabels-dg/tm-delete-model.html
   Shows how to delete an existing Amazon Rekognition Custom Labels model.
   """
   
   import boto3
   import argparse
   import logging
   import time
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def find_forward_slash(input_string, n):
       """
       Returns the location of '/' after n number of occurences. 
       :param input_string: The string you want to search
       : n: the occurence that you want to find.
       """
       position = input_string.find('/')
       while position >= 0 and n > 1:
           position = input_string.find('/', position + 1)
           n -= 1
       return position
   
   def delete_model(rek_client, project_arn, model_arn):
       """
       Deletes an Amazon Rekognition Custom Labels model.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param model_arn: The ARN of the model version that you want to delete.
       """
   
       try:
           #Delete the model
           logger.info(f"Deleting dataset: {model_arn}")
           
           rek_client.delete_project_version(ProjectVersionArn=model_arn)
   
           # Get the model version name
           start=find_forward_slash(model_arn,3) +1
           end=find_forward_slash(model_arn,4)
           version_name=model_arn[start:end]
           
           deleted=False
   
           #model might not be deleted yet, so wait deletion finishes.
           while deleted==False:
               describe_response=rek_client.describe_project_versions(ProjectArn=project_arn,
                   VersionNames=[version_name])
               if len(describe_response['ProjectVersionDescriptions']) == 0:
                       deleted =True
               else:
                   logger.info(f"Waiting for model deletion {model_arn}")  
                   time.sleep(5)
   
           logger.info(f"model deleted: {model_arn}")         
   
   
           return True
      
       
       except ClientError as err:  
           logger.exception(f"Couldn't delete model - {model_arn}: {err.response['Error']['Message']}")
           raise
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_arn", help="The ARN of the project that contains the model that you want to delete."
       )
   
       parser.add_argument(
           "model_arn", help="The ARN of the model version that you want to delete."
       )
   
   def confirm_model_deletion(model_arn):
       """
       Confirms deletion of the model. Returns True if delete entered.
       :param model_arn: The ARN of the model that you want to delete.
       """
       print(f"Are you sure you wany to delete model {model_arn} ?\n", model_arn)
   
       start = input("Enter delete to delete your model: ")
       if start == "delete":
           return True
       else:
           return False
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           if confirm_model_deletion(args.model_arn)==True:
               print(f"Deleting model: {args.model_arn}")
   
               #Delete the model
               rek_client=boto3.client('rekognition')
   
               delete_model(rek_client, 
                   args.project_arn,
                   args.model_arn)
           
               print(f"Finished deleting model: {args.model_arn}")
           else:
               print(f"Not deleting model {args.model_arn}")
   
     
       except ClientError as err:
           logger.exception(f"Problem deleting model: {err}")
           print(f"Problem deleting model: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]
   + `project_arn` – the ARN of the project that contains the model that you want to delete\.
   + `model_arn` – the ARN of the model version that you want to delete\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   
   import software.amazon.awssdk.services.rekognition.model.DeleteProjectVersionRequest;
   import software.amazon.awssdk.services.rekognition.model.DeleteProjectVersionResponse;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsResponse;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   
   public class DeleteModel {
   
       public static final Logger logger = Logger.getLogger(DeleteModel.class.getName());
   
       public static int findForwardSlash(String modelArn, int n) {
   
           int start = modelArn.indexOf('/');
           while (start >= 0 && n > 1) {
               start = modelArn.indexOf('/', start + 1);
               n -= 1;
           }
           return start;
   
       }
   
       public static void deleteMyModel(RekognitionClient rekClient, String projectArn, String modelArn)
               throws InterruptedException {
   
           try {
   
               logger.log(Level.INFO, "Deleting model: {0}", projectArn);
   
               // Delete the model
   
               DeleteProjectVersionRequest deleteProjectVersionRequest = DeleteProjectVersionRequest.builder()
                       .projectVersionArn(modelArn).build();
   
               DeleteProjectVersionResponse response =
                       rekClient.deleteProjectVersion(deleteProjectVersionRequest);
   
               logger.log(Level.INFO, "Status: {0}", response.status());
   
               // Get the model version
   
               int start = findForwardSlash(modelArn, 3) + 1;
               int end = findForwardSlash(modelArn, 4);
   
               String versionName = modelArn.substring(start, end);
   
               Boolean deleted = false;
   
               DescribeProjectVersionsRequest describeProjectVersionsRequest = DescribeProjectVersionsRequest.builder()
                       .projectArn(projectArn).versionNames(versionName).build();
   
               // Wait until model is deleted.
   
               do {
   
                   DescribeProjectVersionsResponse describeProjectVersionsResponse = rekClient
                           .describeProjectVersions(describeProjectVersionsRequest);
   
                   if (describeProjectVersionsResponse.projectVersionDescriptions().size()==0) {
                       logger.log(Level.INFO, "Waiting for model deletion: {0}", modelArn);
                       Thread.sleep(5000);
                   } else {
                       deleted = true;
                       logger.log(Level.INFO, "Model deleted: {0}", modelArn);
                   }
                   
               } while (Boolean.FALSE.equals(deleted));
   
               logger.log(Level.INFO, "Model deleted: {0}", modelArn);
   
           } catch (
   
           RekognitionException e) {
               logger.log(Level.SEVERE, "Client error occurred: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
           final String USAGE = "\n" + "Usage: " + "<project_arn> <model_arn>\n\n" + "Where:\n"
                   + "   project_arn - The ARN of the project that contains the model that you want to delete.\n\n"
                   + "   model_version - The ARN of the model that you want to delete.\n\n";
   
           if (args.length != 2) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           String projectArn = args[0];
           String modelVersion = args[1];
   
           try {
   
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
               // Delete the model
               deleteMyModel(rekClient, projectArn, modelVersion);
   
               System.out.println(String.format("model deleted: %s", modelVersion));
   
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