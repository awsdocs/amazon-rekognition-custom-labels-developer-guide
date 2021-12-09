# Starting an Amazon Rekognition Custom Labels model<a name="rm-start"></a>

You can start running an Amazon Rekognition Custom Labels model by using the console or by using the [StartProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StartProjectVersion) operation\.

**Note**  
You are charged for the amount of time your model is running and for the number of inference units that your model uses\. For more information, see [Running a trained Amazon Rekognition Custom Labels model](running-model.md)\.

**Topics**
+ [Starting an Amazon Rekognition Custom Labels model \(Console\)](#rm-start-console)
+ [Starting an Amazon Rekognition Custom Labels model \(SDK\)](#rm-start-sdk)

## Starting an Amazon Rekognition Custom Labels model \(Console\)<a name="rm-start-console"></a>

Use the following procedure to start running an Amazon Rekognition Custom Labels model with the console\. You can start the model directly from the console or use the AWS SDK code provided by the console\. 

**To start a model \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. On the **Projects** resources page, choose the project that contains the trained model that you want to start\.

1. In the **Models** section, choose the model that you want to start\. 

1. Choose the **Use model** tab\.

1. 

------
#### [ Start model using the console ]

   In the **Start or stop model** section do the following:

   1. Select the number of inference units that you want to use\. For more information, see [Running a trained Amazon Rekognition Custom Labels model](running-model.md)\.

   1. Choose **Start**\.

   1. In the **Start model** dialog box, choose **Start**\. 

------
#### [ Start model using the AWS SDK ]

   In the **Use your model** section do the following:

   1. Choose **API Code\.**

   1. Choose either **AWS CLI** or **Python**\.

   1. In **Start model** copy the example code\.

   1. Use the example code to start your model\. For more information, see [Starting an Amazon Rekognition Custom Labels model \(SDK\)](#rm-start-sdk)\.

------

1. Choose your project name at the top of the page to go back to the project overview page\.

1. In the **Model** section, check the status of the model\. When the model status is **RUNNING**, you can use the model to analyze images\. For more information, see [Analyzing an image with a trained model](detecting-custom-labels.md)\.

## Starting an Amazon Rekognition Custom Labels model \(SDK\)<a name="rm-start-sdk"></a>

You start a model by calling the [StartProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StartProjectVersion) API and passing the Amazon Resource Name \(ARN\) of the model in the `ProjectVersionArn` input parameter\. You also specify the number of inference units that you want to use\. For more information, see [Running a trained Amazon Rekognition Custom Labels model](running-model.md)\.

A model might take a while to start\. The Python and Java examples in this topic use waiters to wait for the model to start\. A waiter is a utility method that polls for a particular state to occur\. Alternatively, you can check the current status by calling [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\. 

**To start a model \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` and permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to start a model\.

------
#### [ CLI ]

   Change the value of `project-version-arn` to the ARN of the model that you want to start\. Change the value of `--min-inference-units` to the number of inference units that you want to use\.

   ```
   aws rekognition start-project-version  --project-version-arn model_arn\
     --min-inference-units "1"
   ```

------
#### [ Python ]

   Supply the following command line parameters:
   + `project_arn` – the ARN of the project that contains the model that you want to start\.
   + `model_arn` – the ARN of the model that you want to start\.
   + `min_inference_units` – the number of inference units that you want to use\.

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import argparse
   import logging
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
   
       models=rek_client.describe_project_versions(ProjectArn=project_arn,
       VersionNames=[version_name])
   
       for model in models['ProjectVersionDescriptions']: 
      
           logger.info(f"Status: {model['StatusMessage']}")
           return model["Status"]
   
           
       logger.exception(f"Model {model_arn} not found.")
       raise Exception(f"Model {model_arn} not found.")
   
   
   def start_model(rek_client, project_arn, model_arn, min_inference_units):
       """
       Starts the hosting of an Amazon Rekognition Custom Labels model.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param project_name:  The name of the project that contains the
       model that you want to start hosting.
       :param min_inference_units: The number of inference units to use for hosting.
       """
   
       try:
           # Start the model
           logger.info(f"Starting model: {model_arn}. Please wait....")
   
           rek_client.start_project_version(ProjectVersionArn=model_arn, MinInferenceUnits=min_inference_units)
           
           # Wait for the model to be in the running state
           version_name=(model_arn.split("version/",1)[1]).rpartition('/')[0]
           project_version_running_waiter = rek_client.get_waiter('project_version_running')
           project_version_running_waiter.wait(ProjectArn=project_arn, VersionNames=[version_name])
   
           #Get the running status
           return get_model_status(rek_client,project_arn, model_arn)
   
           
       except Exception as e:
           logger.exception(f"Couldn't start model - {model_arn}: {e.response['Error']['Message']}")
           
       print('Done...')
       
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "project_arn", help="The ARN of the project that contains that the model you want to start."
       )
       parser.add_argument(
           "model_arn", help="The ARN of the model that you want to start."
       )
       parser.add_argument(
           "min_inference_units", help="The minimum number of inference units to use."
       )
   
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       try:
   
           #get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           #start the model
           rek_client=boto3.client('rekognition')
           status=start_model(rek_client, 
               args.project_arn,args.model_arn, 
               int(args.min_inference_units))
   
           print(f"Finished starting model: {args.model_arn}")
           print(f"Status: {status}")
   
       except ClientError as err:
           logger.exception(f"Problem starting model:{err}")
           print(f"Problem starting model: {err}")
   
       except Exception as err:
           logger.exception(f"Problem starting model:{err}")
           print(f"Problem starting model: {err}")
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java ]

   Supply the following command line parameters:
   + `project_arn` – the ARN of the project that contains the model that you want to start\.
   + `model_arn` – the ARN of the model that you want to start\.
   + `min_inference_units` – the number of inference units that you want to use\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import software.amazon.awssdk.core.waiters.WaiterResponse;
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsRequest;
   import software.amazon.awssdk.services.rekognition.model.DescribeProjectVersionsResponse;
   import software.amazon.awssdk.services.rekognition.model.ProjectVersionDescription;
   import software.amazon.awssdk.services.rekognition.model.ProjectVersionStatus;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   import software.amazon.awssdk.services.rekognition.model.StartProjectVersionRequest;
   import software.amazon.awssdk.services.rekognition.model.StartProjectVersionResponse;
   import software.amazon.awssdk.services.rekognition.waiters.RekognitionWaiter;
   
   import java.net.URI;
   import java.util.Optional;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class StartModel {
   
       public static final Logger logger = Logger.getLogger(StartModel.class.getName());
       
       
       
       public static int findForwardSlash(String modelArn, int n) {
   
           int start = modelArn.indexOf('/');
           while (start >= 0 && n > 1) {
               start = modelArn.indexOf('/', start + 1);
               n -= 1;
           }
           return start;
   
       }
   
       public static void startMyModel(RekognitionClient rekClient, String projectArn, String modelArn, Integer minInferenceUnits
               ) throws Exception, RekognitionException {
   
           try {
               
               logger.log(Level.INFO, "Starting model: {0}", modelArn);
               
               StartProjectVersionRequest startProjectVersionRequest = StartProjectVersionRequest.builder()
                       .projectVersionArn(modelArn)
                       .minInferenceUnits(minInferenceUnits)
                       .build();
   
               StartProjectVersionResponse response = rekClient.startProjectVersion(startProjectVersionRequest);
   
               logger.log(Level.INFO, "Status: {0}", response.statusAsString() );
               
               
               // Get the model version
   
               int start = findForwardSlash(modelArn, 3) + 1;
               int end = findForwardSlash(modelArn, 4);
   
               String versionName = modelArn.substring(start, end);
   
   
               // wait until model starts
   
               DescribeProjectVersionsRequest describeProjectVersionsRequest = DescribeProjectVersionsRequest.builder()
                       .versionNames(versionName)
                       .projectArn(projectArn)
                       .build();
   
               RekognitionWaiter waiter = rekClient.waiter();
   
               WaiterResponse<DescribeProjectVersionsResponse> waiterResponse = waiter
                       .waitUntilProjectVersionRunning(describeProjectVersionsRequest);
   
               Optional<DescribeProjectVersionsResponse> optionalResponse = waiterResponse.matched().response();
   
               DescribeProjectVersionsResponse describeProjectVersionsResponse = optionalResponse.get();
   
               for (ProjectVersionDescription projectVersionDescription : describeProjectVersionsResponse
                       .projectVersionDescriptions()) {
                   if(projectVersionDescription.status() == ProjectVersionStatus.RUNNING) {
                       logger.log(Level.INFO, "Model is running" );
                    
                   }
                   else {
                       String error = "Model training failed: " + projectVersionDescription.statusAsString() + " "
                               + projectVersionDescription.statusMessage() + " " + modelArn;
                       logger.log(Level.SEVERE, error);
                       throw new Exception(error);
                   }
                   
               }
   
   
           } catch (RekognitionException e) {
               logger.log(Level.SEVERE, "Could not start model: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
           String modelArn = null;
           String projectArn = null;
           String minInferenceUnits=null;
           
   
   
   
           final String USAGE = "\n" + "Usage: " + "<project_name> <version_name> <min_inference_units>\n\n" + "Where:\n"
                   + "   project_arn - The ARN of the project that contains the model that you want to start. \n\n"
                   + "   model_arn - The ARN of the model version that you want to start.\n\n"
                   + "   min_inference_units - The ARN of the model version that you want to start.\n\n";
   
           if (args.length != 3) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           projectArn = args[0];
           modelArn = args[1];
           minInferenceUnits=args[2];
   
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
               
               Integer inferenceUnits=Integer.parseInt(minInferenceUnits);
               
               // Start model
               startMyModel(rekClient, projectArn, modelArn, inferenceUnits);
   
               System.out.println(String.format("Model started: %s", modelArn));
   
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