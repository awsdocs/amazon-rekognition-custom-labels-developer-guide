# Getting started with Amazon Rekognition Custom Labels<a name="gs-introduction"></a>

You use Amazon Rekognition Custom Labels to train a machine learning model\. The trained model analyzes images to find the objects, scenes, and concepts that are unique to your business needs\. For example, you can train a model to classify images of houses, or find the location of electronic parts on a printed circuit board\.

To help you get started, Amazon Rekognition Custom Labels includes tutorial videos and example projects\.

**Topics**
+ [Tutorial videos](#gs-tutorial-videos)
+ [Example projects](#gs-example-projects)
+ [Using the example projects](#gs-example-using-projects)
+ [Step 1: Choose an example project](gs-step-choose-example-project.md)
+ [Step 2: Train your model](gs-step-train-model.md)
+ [Step 3: Start your model](gs-step-start-model.md)
+ [Step 4: Analyze an image with your model](gs-step-get-a-prediction.md)
+ [Step 5: Stop your model](gs-step-stop-model.md)
+ [Step 6: Next steps](gs-step-next.md)

## Tutorial videos<a name="gs-tutorial-videos"></a>

The videos show you how to use Amazon Rekognition Custom Labels to train and use a model\.

**To view the tutorial videos**

1. Sign in to the AWS Management Console and open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose **Use Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\. If you don't see **Use Custom Labels**, check that the [ AWS Region](https://docs.aws.amazon.com/general/latest/gr/rekognition_region.html) you are using supports Amazon Rekognition Custom Labels\. 

1. In the navigation pane, choose **Get started**\. 

1. In **What is Amazon Rekognition Custom Labels?**, choose the video to watch the overview video\. 

1. In the navigation pane, choose **Tutorials**\.

1. On the **Tutorials** page, choose the tutorial videos that you want to watch\.

## Example projects<a name="gs-example-projects"></a>

Amazon Rekognition Custom Labels provides the following example projects\. 

### Image classification<a name="gs-image-classification-example"></a>

The image classification project \(Rooms\) trains a model that finds one or more household locations in an image, such as *backyard*, *kitchen*, and *patio*\. The training and test images represent a single location\. Each image is labeled with a single image\-level label, such as *kitchen*, *patio*, or *living\_space*\. For an analyzed image, the trained model returns one or more matching labels from the set of image\-level labels used for training\. For example, the model might find the label *living\_space* in the following image\. For more information, see [Finding objects, scenes, and concepts](cd-create-dataset.md#cd-classification)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/image-classification.jpg)

### Multi\-label image classification<a name="gs-multi-label-image-classification-example"></a>

The multi\-label image classification project \(Flowers\) trains a model that categorizes images of flowers into three concepts \(flower type, leaf presence, and growth stage\)\. 

The training and test images have image\-level labels for each concept, such as *camellia* for a flower type, *with\_leaves* for a flower with leaves, and *fully\_grown* for a flower that is fully grown\.

For an analyzed image, the trained model returns matching labels from the set of image\-level labels used for training\. For example, the model returns the labels *mediterranean\_spurge* and *with\_leaves* for the following image\. For more information, see [Finding objects, scenes, and concepts](cd-create-dataset.md#cd-classification)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/multi-label-classification.jpg)

### Brand detection<a name="gs-brand-detection-example"></a>

The brand detection project \(Logos\) trains a model that model finds the location of certain AWS logos such as *Amazon Textract*, and *AWS lambda*\. The training images are of the logo only and have a single image level\-label, such as *lambda* or *textract*\. It is also possible to train a brand detection model with training images that have bounding boxes for brand locations\. The test images have labeled bounding boxes that represent the location of logos in natural locations, such as an architectural diagram\. The trained model finds the logos and returns a labeled bounding box for each logo found\. For more information, see [Finding brands](cd-create-dataset.md#cd-brand-detection)\.  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/brand-detection-lambda.png)

### Object localization<a name="gs-object-localization-example"></a>

The object localization project \(Circuit boards\) trains a model that finds the location of parts on a printed circuit board, such as a *comparator* or an *infra red light emitting diode*\. The training and test images include bounding boxes that surround the circuit board parts and a label that identifies the part within the bounding box\. The label names are *ir\_phototransistor*, *ir\_led*, *pot\_resistor*, and *comparator*\. The trained model finds the circuit board parts and returns a labeled bounding for each circuit part found\. For more information, see [Finding object locations](cd-create-dataset.md#cd-localization)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/localization-circuit-board.png)

## Using the example projects<a name="gs-example-using-projects"></a>

These Getting Started instructions show you how to train a model by using example projects that Amazon Rekognition Custom Labels creates for you\. It also shows you how to start the model and use it to analyze an image\. 

### Creating the example project<a name="gs-using-examples-creating-project"></a>

To get started, decide which project to use\. For more information, see [Step 1: Choose an example project](gs-step-choose-example-project.md)\.

Amazon Rekognition Custom Labels uses datasets to train and evaluate \(test\) a model\. A dataset manages images and the labels that identify the contents of images\. The example projects include a training dataset and a test dataset in which all images are labeled\. You don't need to make any changes before training your model\. The example projects show the two ways in which Amazon Rekognition Custom Labels uses labels to train different types of models\.
+ *image\-level* – The label identifies an object, scene, or concept that represents the entire image\. 
+ *bounding box* – The label identifies the contents of a bounding box\. A bounding box is a set of image coordinates that surround an object in an image\. 

Later, when you create a project with your own images, you must create training and test datasets, and also label your images\. For more information, see [Tutorials: Training an Amazon Rekognition Custom Labels model](tutorial-introduction.md)\. 

### Training the model<a name="gs-using-examples-training-model"></a>

After Amazon Rekognition Custom Labels creates the example project, you can train the model\. For more information, see [Step 2: Train your model](gs-step-train-model.md)\. After training finishes, you normally evaluate the performance of the model\. The images in the example dataset already create a high\-performance model, and you don't need to evaluate the model before running the model\. For more information, see [Evaluating a trained Amazon Rekognition Custom Labels model](tr-train-results.md)\. 

### Using the model<a name="gs-using-examples-using-model"></a>

Next you start the model\. For more information, see [Step 3: Start your model](gs-step-start-model.md)\. 

After you start running your model, you can use it to analyze new images\. For more information, see [Step 4: Analyze an image with your model](gs-step-get-a-prediction.md)\.

You are charged for the amount of time that your model runs\. When you finish using the example model, you should stop the model\. For more information, see [Step 5: Stop your model](gs-step-stop-model.md)\.

### Next steps<a name="gs-using-examples-next-steps"></a>

When you're ready, you can create your own projects\. For more information, see [Step 6: Next steps](gs-step-next.md)\. 