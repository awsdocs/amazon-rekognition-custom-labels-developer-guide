# Tutorials: Training an Amazon Rekognition Custom Labels model<a name="tutorial-introduction"></a>

These tutorials show you how to train an Amazon Rekognition Custom Labels model by using the console and the AWS SDK\. To prepare, evaluate, train, and use the model to analyze images, you do the following\.



## Decide your model type<a name="ud-general-model-type"></a>

Decide which type of model you want to train\. You can train models that do the following\.
+ [Find objects, scenes, and concepts](ud-model-type.md#ud-classification)
+ [Find object locations](ud-model-type.md#ud-object-localization)
+ [Find the location of brands](ud-model-type.md#ud-brand-detection-localization)

For example, you can train a model to find your logo in social media posts, identify your products on store shelves, classify machine parts in an assembly line, distinguish healthy and infected plants, or detect animated characters\. 

 How you label the images in your datasets determines the type of model that Amazon Rekognition Custom Labels creates\.

To help you decide, Amazon Rekognition Custom Labels provides example projects that you can use\. For more information, see [Getting started with Amazon Rekognition Custom Labels](gs-introduction.md)\. 

## Prepare your Images<a name="ud-general-images"></a>

Collect the images that are specific to your business needs\. Later in these tutorials you label the images so that they can be used to train and test a model\.



## Create a project<a name="ud-general-project"></a>

Create a project to manage the files used to create a model\. A project is where you manage your model files, train and evaluate your model, and make it ready for use\. 

## Create a training dataset<a name="ud-general-dataset"></a>

To create a dataset, import labeled or unlabeled images\. A dataset is a set of images and labels that describe those images\. You use datasets to train and test the models you create\. A dataset is stored in the SageMaker Ground Truth format manifest\. It's important to know how you want to use your dataset\. For more information, see [Purposing datasets](cd-create-dataset.md#cd-dataset-purpose)\.



You create your dataset by importing the images used to train the model\. You can bring in images from Amazon S3, your local computer, an SageMaker Ground Truth manifest file, or an already labeled dataset\.

For unlabeled images, you have to annotate the images with the labels you want to train the model with\. This can be at the image level or, for objects within the image, bounding boxes that isolate the object within an image\.

For these tutorials, you use images loaded from your computer\. The images are unlabeled\. 



## Create a test dataset<a name="ud-general-test-dataset"></a>

Train your model by using the training dataset\. Before training your model, you choose the set of images that Amazon Rekognition Custom Labels uses to evaluate your model\. You can create a new test dataset, use an existing test dataset, or split the training dataset that you just created\. For these tutorials, you split the training dataset\. 

## Train your model<a name="ud-general-train-model"></a>

Train your model with the training dataset\. Training also evaluates your model by using the test dataset\.

## Evaluate your model<a name="ud-general-evaluate-model"></a>

Evaluate the performance of your model by using metrics created during testing\. 

Evaluations enable you to understand the performance of your trained model, and decide if you're ready to use it in production\. If improvements are needed, you can add more training data or improve labeling\. 

## Use your model<a name="ud-general-use-model"></a>

Start your trained model and use it to detect custom labels in new images\.

## Improve the performance of your model<a name="ud-general-improve-performance"></a>

You can give feedback on the predictions your model makes and use it to make improvements to your model\. For more information, see [Model feedback solution](ex-feedback.md)\. 