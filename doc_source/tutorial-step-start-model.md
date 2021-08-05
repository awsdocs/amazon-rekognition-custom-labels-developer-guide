# Step 10: Start your model<a name="tutorial-step-start-model"></a>

If you're happy with the performance of your model, you make it available for use by starting it\. 

**Note**  
You are charged for the amount of time, in minutes, that the model is running\. For more information, see [Inference hours](https://aws.amazon.com/rekognition/pricing/#Amazon_Rekognition_Custom_Labels_pricing)\. 

Starting a model takes a while to complete\. You can use the console to determine if the model is running\. 

**To start a model \(console\)**

1. On the **Projects** resources page, choose the project that contains the trained model that you want to start\.

1. In the **Models** section, choose the model that you want to start\. The summary results are shown\. 

1. Choose the **Use model** tab\.

1. Select the number of inference units that you want to use\. For more information, see [Running a trained Amazon Rekognition Custom Labels model](rm-run-model.md)\.

1. Choose **Start**\.

1. In the **Start model** dialog box, choose **Start**\. 

1. Choose your project name at the top of the page to go back to the project overview page\.

1. In the **Model** section, check the status of the model\. When the model status is **RUNNING**, you can use the model to analyze images\. For more information, see [Analyzing an image with a trained model](detecting-custom-labels.md)\.