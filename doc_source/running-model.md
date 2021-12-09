# Running a trained Amazon Rekognition Custom Labels model<a name="running-model"></a>

When you're satisfied with the performance of the model, you can start to use it\. You can start and stop a model by using the console or by using the AWS SDK\. The console also includes example SDK operations that you can use\.

You are charged for the amount of time, in minutes, that the model is running\. For more information, see [Inference hours](https://aws.amazon.com/rekognition/pricing/#Amazon_Rekognition_Custom_Labels_pricing)\. 

The transactions per second \(TPS\) that a single inference unit supports is affected by the following\.
+ A model that detects image\-level labels \(classification\) generally has a higher TPS than a model that detects and localizes objects with bounding boxes \(object detection\)\.
+ The complexity of the model\.
+ A higher resolution image requires more time for analysis\.
+ A Higher numbers of objects in an image requires more time for analysis\.
+ Smaller images are analyzed faster than larger images\. 
+ An image passed as image bytes is analyzed faster than first uploading the image to an Amazon S3 bucket and then referencing the uploaded image\. Images passed as image bytes must be smaller than 4\.0 MB\. We recommend that you use image bytes for near realtime processing of images and when the image size is less that 4\.0 MB\. For example, images captured from an IP camera\.
+ Processing images stored in an Amazon S3 bucket is faster than downloading the image, converting to image bytes, and then passing the image bytes for analysis\.
+ Analyzing an image that is already stored in an Amazon S3 bucket is probably faster than analyzing the same image passed as image bytes, especially if the image size is larger\.

 If required, you can use a higher number of inference units to increase the throughput of your model\. You are charged for the number of inference units that you use\. You specify the number of inference units to use when you start the model\. If you want to change the number of inference units that a running model uses, first stop the model and then restart with the required number of inference units\.

Amazon Rekognition Custom Labels distributes inference units across multiple Availability Zones within an AWS Region to provide increased availability\. For more information, see [Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/#Availability_Zones)\. To help protect your production models from Availability Zone outages and inference unit failures, we strongly recommend that you start your production models with at least 2 inference units\. 

If an Availability Zone outage occurs, all inference units in the Availability Zone are unavailable and model capacity is reduced\. Calls to [DetectCustomLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectCustomLabels) are redistributed across the remaining inference units and succeed if the calls don’t exceed the supported Transactions Per Seconds \(TPS\) of the remaining inference units\. After AWS repairs the Availability Zone, the inference units are restarted, and full capacity is restored\. 

If a single inference unit fails, Amazon Rekognition Custom Labels automatically starts a new inference unit in the same Availability Zone\. Model capacity is reduced until the new inference unit starts\.

For information about image limits, see [Detection](limits.md#limits-detection)\.

Starting a model might take a few minutes to complete\. To check the current status of the model readiness, check the details page for the project or use [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\.

After the model is started you use [DetectCustomLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectCustomLabels), to analyze images using the model\. For more information, see [Analyzing an image with a trained model](detecting-custom-labels.md)\. The console also provides example code to call `DetectCustomLabels`\. 

**Topics**
+ [Starting an Amazon Rekognition Custom Labels model](rm-start.md)
+ [Stopping an Amazon Rekognition Custom Labels model](rm-stop.md)