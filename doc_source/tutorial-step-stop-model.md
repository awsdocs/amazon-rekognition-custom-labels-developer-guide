# Step 12: Stop your model<a name="tutorial-step-stop-model"></a>

You are charged for the amount of time your model is running\. If you have finished using the model, you should stop it\. The console provides example code that you use to stop the model\. For more information, see [Running a trained Amazon Rekognition Custom Labels model](rm-run-model.md)\. 

To run the example code, you need to set up the AWS SDK\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

Stopping a model takes a while to complete\. Check the console to determine if the model is running\. Alternatively you can use the `DescribeProjectVersions` API to get the current status\.

**To stop a model \(console\)**

1. On the **Projects** resources page, choose the project that contains the trained model that you want to stop\.

1. In the **Models** section, choose the model that you want to stop\. The summary results are shown\. 

1. Choose the **Use model** tab\.

1. In the **Start or stop model** section choose **Stop**\.

1. In the **Stop model** dialog box, enter **stop** to confirm that you want to stop the model\.

1. Choose **Stop** to stop your model\. The model has stopped when the status in the **Start or stop model** section is **Stopped**\.

   You can also check the model status from the **Model** section of the project overview page\. The model has successfully stopped when the **Model status** is **STOPPED**\.