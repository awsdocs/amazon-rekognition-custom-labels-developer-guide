# Running a Trained Amazon Rekognition Custom Labels Model<a name="rm-run-model"></a>

When you're satisfied with the performance of the model, you can start to use it\. To use a model, you must first start it by using the [StartProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_StartProjectVersion) API operation\. The console provides example code for you to use\. 

You are charged for the amount of time, in minutes, that the model is running\. For more information, see [Inference hours](https://aws.amazon.com/rekognition/pricing/#Amazon_Rekognition_Custom_Labels_pricing)\. 

The transactions per second \(TPS\) that a single inference unit supports is affected by the following\.
+ A Model that detects image\-level labels \(classification\) generally has a higher TPS than a model that detects and localizes objects with bounding boxes \(object detection\)\.
+ The complexity of the model\.
+ A higher resolution image requires more time for analysis\.
+ A Higher numbers of objects in an image requires more time for analysis\.
+ Smaller images are analyzed faster than larger images\. 
+ An image passed as image bytes is analyzed faster than first uploading the image to an Amazon S3 bucket and then referencing the uploaded image\. Images passed as image bytes must be smaller than 4\.0 MB\. We recommend that you use image bytes for near realtime processing of images and when the image size is less that 4\.0 MB\. For example, images captured from an IP camera\.
+ Processing images stored in an Amazon S3 bucket is faster than downloading the image, converting to image bytes, and then passing the image bytes for analysis\.
+ Analyzing an image that is already stored in an Amazon S3 bucket is probably faster than analyzing the same image passed as image bytes, especially if the image size is larger\.

For information about image limits, see [Detection](limits.md#limits-detection)\.

 If required, you can use a higher number of inference units to increase the throughput of your model\. You are charged for the number of inference units that you use\. You specify the number of inference units to use in the `MinInferenceUnits` input parameter\. 

Starting a model might take a few minutes to complete\. To check the current status of the model readiness, use [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\.

After the model is started you use [DetectCustomLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectCustomLabels), to analyze images using the model\. For more information, see [Analyzing an Image with a Trained Model](detecting-custom-labels.md)\. The console also provides example code to call `DetectCustomLabels`\. 

**Topics**
+ [Starting or Stopping an Amazon Rekognition Custom Labels Model \(Console\)](rm-start-model-console.md)
+ [Starting an Amazon Rekognition Custom Labels Model \(SDK\)](rm-start-model-sdk.md)
+ [Stopping an Amazon Rekognition Custom Labels Model \(SDK\)](rm-stop-model-sdk.md)