# Creating training and test datasets \(Console\)<a name="md-create-dataset"></a>

You can start with a project that has a single dataset, or a project that has separate training and test datasets\. If you start with a single dataset, Amazon Rekognition Custom Labels splits your dataset during training to create a training dataset \(80%\) and a test dataset \(%20\) for your project\. Start with a single dataset if you want Amazon Rekognition Custom Labels to decide where images are used for training and testing\. For complete control over training, testing, and performance tuning, we recommend that you start your project with separate training and test datasets\. 

You can create training and test datasets for a project by importing images from one of the following locations:
+ [Amazon S3 bucket](md-create-dataset-s3.md)
+ [Local computer](md-create-dataset-computer.md)
+ [Amazon SageMaker Ground Truth](md-create-dataset-ground-truth.md)
+ [Existing dataset](md-create-dataset-existing-dataset.md)

If you start your project with separate training and test datasets, you can use different source locations for each dataset\.

Depending on where you import your images from, your images might be unlabeled\. For example, images imported from a local computer aren't labeled\. Images imported from an Amazon SageMaker Ground Truth manifest file are labeled\. You can use the Amazon Rekognition Custom Labels console to add, change, and assign labels\. For more information, see [Labeling images](md-labeling-images.md)\.

If images are uploading with errors, images are missing, or labels are missing from images, read [Debugging a failed model training](tm-debugging.md)\.

For more information about datasets, see [Managing datasets](managing-dataset.md)\.