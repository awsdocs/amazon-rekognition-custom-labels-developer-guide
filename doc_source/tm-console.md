# Training a model \(Console\)<a name="tm-console"></a>

You can train a model by using the Amazon Rekognition Custom Labels console\.

**To train a model \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. On the **Projects** page, choose the project that contains the dataset you want to train\.

1. In the **Models** section, choose **Train new model**\. The **Train Model** page is displayed\. 

1. In **Choose project**, choose the model that you want to train\. 

1. On the **Train model** page, do the following:

   1. In **Choose Project**, choose the project you created earlier\.

   1. In **Choose training set**, choose the training dataset that you want to use\.

   1. In **Create test set**, choose how you want to create the testing dataset, and follow the instructions\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/train-model.png)

   1. \(Optional\) if you want to add tags to your model do the following:

      1. In the **Tags** section, choose **Add new tag**\.

      1. Enter the following:

         1. The name of the key in **Key**\.

         1. The value of the key in **Value**\.

      1. To add more tags, repeat steps i and ii\.

      1. \(Optional\) If you want to remove a tag, choose **Remove** next to the tag that you want to remove\. If you are removing a previously saved tag, it is removed when you save your changes\.

   1. \(Optional\) If you want to use your own AWS KMS encryption key, do the following:

      1. In **Image data encryption** choose **Customize encryption settings \(advanced\)**\.

      1. In **Choose an AWS KMS key** enter the Amazon Resource Name \(ARN\) of your key, or choose an existing AWS KMS key\. To create a new key, choose **Create an AWS KMS key**\.

1. Choose **Train** to start training the model\. The project resources page is shown\.

1. Wait until training has finished\. You can check the current status in the **Status** field of the **Model** section\.

1. If training fails, read [Debugging a failed model training](tm-debugging.md)\. 