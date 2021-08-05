# Stopping an Amazon Rekognition Custom Labels model<a name="stop-running-model"></a>

You can stop running an Amazon Rekognition Custom Labels model by using the console or by using the [StopProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StopProjectVersion) operation\.

**Topics**
+ [Stopping an Amazon Rekognition Custom Labels model \(Console\)](#rm-stop-model-console)
+ [Stopping an Amazon Rekognition Custom Labels model \(SDK\)](#rm-stop-model-sdk)

## Stopping an Amazon Rekognition Custom Labels model \(Console\)<a name="rm-stop-model-console"></a>

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

   1. Use the example code to stop your model\. For more information, see [Stopping an Amazon Rekognition Custom Labels model \(SDK\)](#rm-stop-model-sdk)\.

------

1. Choose your project name at the top of the page to go back to the project overview page\.

1. In the **Model** section, check the status of the model\. The model has stopped when the model status is **STOPPED**\.

## Stopping an Amazon Rekognition Custom Labels model \(SDK\)<a name="rm-stop-model-sdk"></a>

You stop a model by calling the [StopProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StopProjectVersion) API and passing the Amazon Resource Name \(ARN\) of the model in the `ProjectVersionArn` input parameter\. 

A model might take a while to stop\. To check the current status, use `DescribeProjectVersions`\. 

**To stop a model \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code to stop a running model\.

------
#### [ CLI ]

   Change the value of `project-version-arn` to the ARN of the model that you want to stop\.

   ```
   aws rekognition stop-project-version --project-version-arn "my project"
   ```

------
#### [ Python ]

   The following example stops a model that is already running\.

   Change the value of `model_arn` to the name of the model that you want to stop\.

   ```
   #Copyright 2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import time
   
   
   def stop_model(model_arn):
   
       client=boto3.client('rekognition')
   
       print('Stopping model:' + model_arn)
   
       #Stop the model
       try:
           response=client.stop_project_version(ProjectVersionArn=model_arn)
           status=response['Status']
           print ('Status: ' + status)
       except Exception as e:  
           print(e)  
   
       print('Done...')
       
   def main():
       
       model_arn='model_arn'
       stop_model(model_arn)
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java ]

   The following example stops a model that is already running\.

   Change the value of `projectVersionArn` to the name of the model that you want to stop\.

   ```
   //Copyright 2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   package com.amazonaws.samples;
   import com.amazonaws.services.rekognition.AmazonRekognition;
   import com.amazonaws.services.rekognition.AmazonRekognitionClientBuilder;
   import com.amazonaws.services.rekognition.model.StopProjectVersionRequest;
   import com.amazonaws.services.rekognition.model.StopProjectVersionResult;
   
   
   public class StopModel {
   
      public static void main(String[] args) throws Exception {
   
   
         String projectVersionArn ="model_arn";
   
         AmazonRekognition rekognitionClient = AmazonRekognitionClientBuilder.defaultClient();
          
         try {
             
            StopProjectVersionRequest request = new StopProjectVersionRequest()
                     .withProjectVersionArn(projectVersionArn); 
            StopProjectVersionResult result = rekognitionClient.stopProjectVersion(request);
     
            System.out.println(result.getStatus());
   
         } catch(Exception e) {
            System.out.println(e.toString());
         }
         System.out.println("Done...");
      }
   }
   ```

------